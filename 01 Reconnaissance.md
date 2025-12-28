# Phase 1 – Reconnaissance & Information Gathering

## Objective
The objective of this phase was to identify exposed services, enumerate service versions, and understand the attack surface of the target system prior to exploitation. Only reconnaissance and enumeration techniques were used during this phase.

---

## Target Overview

| Role            | IP Address       |
|-----------------|------------------|
| Attacker (Kali) | 10.128.0.52      |
| Target          | 10.128.0.226     |

Target availability was confirmed via ICMP prior to active scanning.

---

## Network Enumeration

### TCP Port Scanning
A full TCP port scan was performed to identify exposed services.

**Tool:** Nmap  
**Command:**
```bash
nmap -sV -p- 10.128.0.226
```

## Discovered Open Ports

| Port | Service | Version / Notes |
|------|---------|-----------------|
| 21   | FTP     | vsftpd 2.3.4 |
| 22   | SSH     | OpenSSH 8.4p1 (Debian 11) |
| 23   | Telnet  | Linux telnetd |
| 6200 | Unknown | Backdoor listener (vsftpd) |

---

## Metasploit Service Enumeration

Metasploit auxiliary scanner modules were used to confirm service banners and versions identified during Nmap scanning.

### FTP Version Detection
```bash
use auxiliary/scanner/ftp/ftp_version
set RHOSTS 10.128.0.226
run
```

![FTP Banner Grabbing](Screenshots/Screenshot%20FTP-banner-grabbing.png)


**Result:**
- FTP service identified as **vsftpd 2.3.4**
- Version banner exposed without authentication

---

### SSH Version Detection

```bash
use auxiliary/scanner/ssh/ssh_version
set RHOSTS 10.128.0.226
run
```

![FTP Banner Grabbing](Screenshots/Screenshot%20SSH-banner-grabbing.png)

**Result:**

- **OpenSSH 8.4p1** detected

- Modern SSH implementation observed

### TELNET Version Detection

```bash
use auxiliary/scanner/telnet/telnet_version
set RHOSTS 10.128.0.226
run
```

![FTP Banner Grabbing](Screenshots/Screenshot%20TELNET-banner-grabbing.png)

**Result:**

- Telnet service exposed
- Login banner visible prior to authentication

## Anonymous FTP Access Test

Anonymous FTP access was tested to determine whether unauthenticated file access was permitted.

```bash
use auxiliary/scanner/ftp/anonymous
set RHOSTS 10.128.0.226
run
```

![Anonymous FTP Access](Screenshots/Screenshot%20AnonFTPaccess.png)


**Result:**
- Anonymous FTP access was not permitted

---

## Automated Reconnaissance (Resource Script)

A custom Metasploit resource script was created to automate reconnaissance tasks.

**Script:** advanced_scan.rc

**Purpose:**
- TCP port scanning
- FTP version enumeration
- Repeatable and consistent reconnaissance

This script was executed using:

```bash
resource advanced_scan.rc
```

## Custom Metasploit Resource Script – Advanced Lab

The following Metasploit resource script was created to automate reconnaissance
tasks identified during the engagement. The script focuses exclusively on
service discovery and version enumeration.


> [!NOTE]
>
> ```rc
> # script 
>
> use auxiliary/scanner/portscan/tcp
> set RHOSTS 10.128.0.226
> set PORTS 21,22,23,6200
> run
>
> use auxiliary/scanner/ftp/ftp_version
> set RHOSTS 10.128.0.226
> run
>
> use auxiliary/scanner/ssh/ssh_version
> set RHOSTS 10.128.0.226
> run
>
> use auxiliary/scanner/telnet/telnet_version
> set RHOSTS 10.128.0.226
> run
> ```



## Key Findings

- Multiple network services were exposed to the network.
- FTP service was running **vsftpd 2.3.4**, a version known to contain a backdoor vulnerability.
- Telnet was enabled, significantly increasing attack surface.
- SSH was present but appeared hardened against basic enumeration and brute-force attacks.
- A suspicious listener on port **6200** was identified.

---

## Reconnaissance Summary

The reconnaissance phase successfully identified critical exposed services and precise version information. The presence of an outdated FTP service and insecure remote management protocols indicated a high-risk security posture and justified further vulnerability assessment and exploitation.

*No exploitation activities were performed during this phase.*


