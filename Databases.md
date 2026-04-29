### CPTS / HTB Penetration Tester Path <br>
### Metasploit Framework - Databases <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>

<br>
<br>

---

### Databases



`msfconsole` ships with built-in PostgreSQL integration — track hosts, services, creds, loot, vulns, and notes per engagement. Imports & exports XML for cross-tool workflows.

Bring up PostgreSQL & init the MSF DB:

```diff
+ $ sudo service postgresql status
+ $ sudo systemctl start postgresql
+ $ sudo msfdb init
+ $ sudo msfdb status
+ $ sudo msfdb run
```

If the DB is stuck — reinit, copy config to the user dir, restart:

```diff
+ $ msfdb reinit
+ $ cp /usr/share/metasploit-framework/config/database.yml ~/.msf4/
+ $ sudo service postgresql restart
+ $ msfconsole -q
+ msf6 > db_status
```

Inside `msfconsole` — DB commands:

```diff
+ msf6 > help database
+ msf6 > db_status
+ msf6 > db_connect
+ msf6 > db_disconnect
+ msf6 > db_import Target.xml
+ msf6 > db_export -f xml backup.xml
+ msf6 > db_nmap -sV -sS 10.10.10.8
```

Workspaces — separate folders per engagement:

```diff
+ msf6 > workspace
+ msf6 > workspace -a Target_1
+ msf6 > workspace Target_1
+ msf6 > workspace -d Target_1
+ msf6 > workspace -h
```

Hosts table — auto-populated by scans, manually editable:

```diff
+ msf6 > hosts
+ msf6 > hosts -a 10.10.10.40
+ msf6 > hosts -d 10.10.10.40
+ msf6 > hosts -R
```

Services — same pattern, filter by port/proto/name:

```diff
+ msf6 > services
+ msf6 > services -p 445
+ msf6 > services -s smb
+ msf6 > services -R
```

Credentials — stores plaintext, NTLM, SSH keys, postgres MD5, JTR-formatted hashes:

```diff
+ msf6 > creds
+ msf6 > creds add user:admin password:notpassword realm:workgroup
+ msf6 > creds add user:admin ntlm:E2FC15074BF7751DD408E6B105741864:A1074A69B1BDE45403AB680504BBDD1A
+ msf6 > creds add user:sshadmin ssh-key:/path/to/id_rsa
+ msf6 > creds -t ntlm
+ msf6 > creds -d
```

Loot — hashdumps, passwd/shadow files, captured configs:

```diff
+ msf6 > loot
+ msf6 > loot -a -f hashes.txt -t hashdump -i "domain hashes"
+ msf6 > loot -d
+ msf6 > loot -S admin
```

Quick command map:

| Command | Purpose |
|---|---|
| `db_import` | Import scans (XML preferred) |
| `db_export` | Backup / export results |
| `db_nmap` | Run integrated Nmap |
| `db_status` | Check DB connection state |
| `hosts` | Manage discovered hosts |
| `services` | Manage discovered services |
| `creds` | Manage credentials & hashes |
| `loot` | Manage gathered loot |
| `workspace` | Per-engagement folders |
