### CPTS / HTB Penetration Tester Path <br>
### Metasploit Framework - Plugins and Mixins <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>

<br>
<br>

---

### Plugins and Mixins



Plugins extend `msfconsole` by integrating third-party tools / commands directly into the framework. They hook the MSF API, store results in the DB, and add new commands to `help`.

Default plugin path:

```diff
+ $ ls /usr/share/metasploit-framework/plugins
```

	aggregator.rb  beholder.rb  db_credcollect.rb  db_tracker.rb
	event_tester.rb  ffautoregen.rb  ips_filter.rb  komand.rb  lab.rb
	libnotify.rb  msfd.rb  msgrpc.rb  nessus.rb  nexpose.rb  openvas.rb
	pcap_log.rb  request.rb  rssfeed.rb  sample.rb  session_notifier.rb
	session_tagger.rb  socket_logger.rb  sounds.rb  sqlmap.rb  thread.rb
	token_adduser.rb  token_hunter.rb  wiki.rb  wmap.rb

Load a plugin in msfconsole and view its commands:

```diff
+ msf6 > load nessus
+ msf6 > nessus_help
```

Failed load means the `.rb` isn't in the plugins dir:

	[-] Failed to load plugin from /usr/share/metasploit-framework/plugins/Plugin.rb: cannot load such file

Install a custom plugin from a repo (DarkOperator's set):

```diff
+ $ git clone https://github.com/darkoperator/Metasploit-Plugins
+ $ sudo cp ./Metasploit-Plugins/pentest.rb /usr/share/metasploit-framework/plugins/
+ $ msfconsole -q
+ msf6 > load pentest
```

Common plugins:

| Plugin | Purpose |
|---|---|
| `nmap` | Port scanning integration |
| `nessus` / `nexpose` / `openvas` | Vuln scanner integration |
| `mimikatz` (replaced by `kiwi`) | Credential extraction |
| `incognito` | Token manipulation |
| `railgun` | Windows API interaction from Meterpreter |
| `stdapi` / `priv` | Default Meterpreter extensions |

Naming rules: snake_case, alphanumeric + underscores only.

`Mixins` — Ruby modules that get `include`'d into classes for shared methods (NOT inheritance). Used heavily inside MSF source to avoid duplicated exploit/post code. As a user you don't write them, but they're the reason the framework is so extensible.
