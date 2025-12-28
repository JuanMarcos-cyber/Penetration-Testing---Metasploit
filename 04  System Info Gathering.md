## Phase 4: Post-Exploitation & Persistence

### Step 1: Session Management

After successful exploitation, Metasploit session management was used to control, interact with, and terminate active shells.

#### List Active Sessions

```bash
sessions -l
```

#### Interact With a Session

```bash
sessions -i 1
```

#### Background a Session

```bash
background
```

#### Kill a Session

```bash
sessions -k 1
```

#### Execute a Command on a Session

```bash
sessions -c "whoami"
```

---

### Step 2: System Information Gathering

After gaining shell access, comprehensive post-exploitation enumeration was performed.

#### Basic System Information

```bash
whoami
id
uname -a
```

![Environment Variables Enumeration](Screenshots/Screenshot%20systemInfoGathering1.png)

#### User and Group Information

```bash
cat /etc/passwd
cat /etc/shadow
cat /etc/group
```


![Environment Variables Enumeration](Screenshots/Screenshot%20systemInfoGathering2.png)

![Environment Variables Enumeration](Screenshots/Screenshot%20systemInfoGathering3.png)

![Environment Variables Enumeration](Screenshots/Screenshot%20systemInfoGathering4.png)

#### Process and Network Information

```bash
ps aux
netstat -an
ss -tulpn
```

![Environment Variables Enumeration](Screenshots/Screenshot%20InfoGatheringPSAUX1.png)

![Environment Variables Enumeration](Screenshots/Screenshot%20InfoGatheringPSAUX2.png)

![Environment Variables Enumeration](Screenshots/Screenshot%20InfoGatheringSS%20TULPN.png)

#### File System Information

```bash
mount
df -h
ls -la /
```

![Environment Variables Enumeration](Screenshots/Screenshot%20InfoGatheringMount.png)


![Environment Variables Enumeration](Screenshots/Screenshot%20InfoGatheringDF-H.png)

![Environment Variables Enumeration](Screenshots/Screenshot%20InfoGatheringLS-LA.png)


#### Scheduled Tasks and Privileges

```bash
crontab -l
sudo -l
```

![Environment Variables Enumeration](Screenshots/Screenshot%20InfoGatheringCrontab&Sudo.png)

#### Environment and OS Configuration

```bash
env
cat /etc/issue
cat /etc/os-release
```

![Environment Variables Enumeration](Screenshots/Screenshot%20InfoGatheringEnv.png)

---

### Step 3: Advanced Enumeration

Additional enumeration was conducted to identify privilege escalation vectors and persistence opportunities.

#### SUID Binary Discovery

```bash
find / -perm -4000 -type f 2>/dev/null
```

![Environment Variables Enumeration](Screenshots/Screenshot%20SUIDbinaries.png)

#### Configuration and Log File Discovery

```bash
find / -name "*.conf" 2>/dev/null | head -20
find / -name "*.log" 2>/dev/null | head -20
```

![Environment Variables Enumeration](Screenshots/Screenshot%20Conf&LogFiles.png)

#### Service Enumeration

```bash
service --status-all
systemctl list-units --type=service
```
![Environment Variables Enumeration](Screenshots/Screenshot%20RunningServices.png)

![Environment Variables Enumeration](Screenshots/Screenshot%20RunningServices2.png)

---

### Phase Summary

The post-exploitation phase confirmed full root-level compromise of the target system. System, user, process, network, and service enumeration provided complete situational awareness. This phase demonstrated how an attacker can transition from initial access to full control and reconnaissance of a compromised host in a real-world penetration testing scenario.
