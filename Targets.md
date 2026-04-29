### CPTS / HTB Penetration Tester Path <br>
### Metasploit Framework - Targets <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>

<br>
<br>

---

### Targets



`Targets` = OS/service version identifiers per exploit module. They tell MSF which return address / offset / payload variant to use for the victim's exact build.

List targets for the currently-selected exploit:

```diff
+ msf6 exploit(...) > show targets
```

`Automatic` lets msfconsole pick via service detection. If you know the build, force it:

```diff
+ msf6 exploit(windows/browser/ie_execcommand_uaf) > set target 6
```

Inspect the module before firing — author, refs, supported targets, side effects:

```diff
+ msf6 exploit(...) > info
```

Example target list (`exploit/windows/browser/ie_execcommand_uaf`):

	Id  Name
	--  ----
	0   Automatic
	1   IE 7 on Windows XP SP3
	2   IE 8 on Windows XP SP3
	3   IE 7 on Windows Vista
	4   IE 8 on Windows Vista
	5   IE 8 on Windows 7
	6   IE 9 on Windows 7

Targets vary on:

- OS version & service pack
- Architecture (x86/x64)
- Language pack (return-address shifts)
- Software version / patched offsets
- Hooks present in memory

To identify a target manually for custom exploit dev:

- Obtain a copy of the target binaries
- Use `msfpescan` to locate a suitable return address (`jmp esp`, `pop/pop/ret`)
