### CPTS / HTB Penetration Tester Path <br>
### Metasploit Framework - MSF Updates (2020) <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>

<br>
<br>

---

### Metasploit Framework Updates - August 2020



MSF6 release (Aug 2020) — breaking change. MSF5 payload sessions / generated payloads do NOT work with MSF6 comms. Highlights below.

#### Generation Features

- End-to-end encryption across Meterpreter sessions for all 5 implementations: Windows, Python, Java, Mettle, PHP
- SMBv3 client support (modern exploitation paths)
- New polymorphic Windows shellcode generation routine for AV/IDS evasion

#### Expanded Encryption

- All Meterpreter payloads use AES for C2 traffic
- SMBv3 encryption integration breaks signature-based detection of SMB ops
- Increased complexity for signature creators on payload binaries

#### Cleaner Payload Artifacts

- Windows Meterpreter DLLs resolve functions by ordinal instead of name
- `ReflectiveLoader` export no longer present as text data in reflective DLLs
- Meterpreter commands encoded as integers instead of strings on the wire

#### Plugins

- Old `mimikatz` Meterpreter extension removed — replaced by `kiwi`
- `load mimikatz` → loads `kiwi` instead

#### Payloads

- Static shellcode generation replaced with randomization routine
- Polymorphic instruction shuffling on each generation pass
- Full changelog: [Rapid7 blog post](https://blog.rapid7.com/2020/08/06/metasploit-6-now-under-active-development/)

#### Closing Thoughts

MSF is a powerful, extensible framework — great for tracking engagement data, post-exploitation, and pivoting. Use it where it fits, ignore it where it doesn't. Practice with the HTB boxes tagged at the end of this module or in the Dante Pro Lab for pivoting reps.
