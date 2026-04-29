### CPTS / HTB Penetration Tester Path <br>
### Metasploit Framework - Writing and Importing Modules <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>

<br>
<br>

---

### Writing and Importing Modules



Two ways to add new modules:

1. **Import** an existing `.rb` Metasploit module from ExploitDB
2. **Port** an exploit from another language (Python/PHP/etc.) into Ruby for MSF

ExploitDB has a [Metasploit Framework tag](https://www.exploit-db.com/?tag=3) that filters to MSF-compatible modules only.

Search inside msfconsole or with `searchsploit`:

```diff
+ msf6 > search nagios
+ $ searchsploit nagios3
+ $ searchsploit -t Nagios3 --exclude=".py"
```

Default module paths:

```diff
+ $ ls /usr/share/metasploit-framework/modules/
+ $ ls ~/.msf4/modules/
```

If `~/.msf4/modules/` is missing subfolders, recreate the directory tree to match `/usr/share/metasploit-framework/modules/`. Naming rules: `snake_case`, alphanumeric + underscores only.

Drop a downloaded module into the right path & rename:

```diff
+ $ cp ~/Downloads/9861.rb /usr/share/metasploit-framework/modules/exploits/unix/webapp/nagios3_command_injection.rb
```

Load custom modules — three ways:

```diff
+ $ msfconsole -m /usr/share/metasploit-framework/modules/
+ msf6 > loadpath /usr/share/metasploit-framework/modules/
+ msf6 > reload_all
```

Then:

```diff
+ msf6 > use exploit/unix/webapp/nagios3_command_injection
+ msf6 exploit(...) > show options
```

#### Porting a Script to a Module

Module skeleton — `class MetasploitModule < Msf::Exploit::Remote` with mixins via `include`:

| Mixin | Purpose |
|---|---|
| `Msf::Exploit::Remote::HttpClient` | HTTP client functions |
| `Msf::Exploit::PhpEXE` | First-stage PHP payload generation |
| `Msf::Exploit::FileDropper` | File transfer + cleanup |
| `Msf::Auxiliary::Report` | Report data to MSF DB |

Required sections in every module:

- `initialize(info={})` block — Name, Description, Author, License, References (CVE/URL), Platform, Arch, Targets, DisclosureDate, DefaultTarget
- `register_options([...])` — required user inputs (`OptString`, `OptPath`, `OptInt`, etc.)
- Exploit code — the actual logic, calling mixin methods (`send_request_cgi`, `report_vuln`, etc.)

Example option block:

	register_options(
	  [
	    OptString.new('TARGETURI', [true, 'The base path', '/']),
	    OptString.new('USERNAME', [true, 'The username']),
	    OptPath.new('PASSWORDS', [true, 'Password list',
	      File.join(Msf::Config.data_directory, "wordlists", "passwords.txt")])
	  ])

#### Style rules

- ALWAYS hard tabs in Ruby source (MSF convention)
- Cross-reference [Metasploit API docs](https://docs.metasploit.com/api/) for every mixin/method
- Use an existing module in the same category as boilerplate
- Test in a sandbox before deployment

| Method | Description | Use Case |
|---|---|---|
| Importing | Drop pre-written `.rb` from ExploitDB into MSF path | Quick deployment of public modules |
| Porting | Rewrite Python/PHP/etc. exploit in Ruby | Exploit exists, but no MSF module yet |
