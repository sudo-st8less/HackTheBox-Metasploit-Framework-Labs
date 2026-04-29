### CPTS / HTB Penetration Tester Path <br>
### Metasploit Framework - Intro <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>

<br>
<br>

---

### Preface



Tools save time but breed tunnel vision. Audit every tool before use, read its docs end-to-end, and verify it leaves no artifacts on the client or your attacker box. Use the tool as a tool, never as the backbone of an assessment.

<br>

---

<br>

### Introduction to Metasploit



`Metasploit Framework` (MSF) — Ruby-based, modular pentest platform. Free, open source. `Metasploit Pro` adds GUI, task chains, social engineering, vuln validation, Nexpose integration (paid).

Base files live at `/usr/share/metasploit-framework/`:

| Folder | Contents |
|---|---|
| `data` / `documentation` / `lib` | Core framework files |
| `modules` | `auxiliary`, `encoders`, `evasion`, `exploits`, `nops`, `payloads`, `post` |
| `plugins` | Optional third-party integrations (Nessus, OpenVAS, etc.) |
| `scripts` | Meterpreter / shell / resource scripts |
| `tools` | CLI utilities callable from `msfconsole` |

List installed module categories:

```diff
+ $ ls /usr/share/metasploit-framework/modules
```

	auxiliary  encoders  evasion  exploits  nops  payloads  post

<br>

---

<br>

### MSFconsole



`msfconsole` — primary interface to MSF. All-in-one CLI, full readline + tab complete, runs external commands inline.

Launch:

```diff
+ $ msfconsole
+ $ msfconsole -q
```

`-q` skips the banner. Update via apt (no more `msfupdate`):

```diff
+ $ sudo apt update && sudo apt install metasploit-framework
```

MSF engagement structure (5 phases):

| Phase | Focus |
|---|---|
| Enumeration | Service Validation, Vulnerability Research |
| Preparation | Code Auditing |
| Exploitation | Module Execution |
| Privilege Escalation | Local exploit suggester, token theft |
| Post-Exploitation | Pivoting, Data Exfiltration |

<br>

---

<br>

### Intro Exercise

---

### Question 1:
Which version of Metasploit comes equipped with a GUI interface?

&#x1F6A9; found **Metasploit Pro**.

---

### Question 2:
What command do you use to interact with the free version of Metasploit?

&#x1F6A9; found **msfconsole**.
