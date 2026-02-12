# 🐧 Linux for Cybersecurity – Master Documentation

Comprehensive theoretical reference for students, penetration testers, SOC analysts, system administrators, and security engineers.

---

# Table of Contents

1. Why Linux Matters  
2. Linux Architecture  
3. Shell & Command Line  
4. Filesystem Hierarchy  
5. Users & Groups  
6. Permissions & Ownership  
7. Sudo & Privilege Escalation  
8. Processes & Resource Monitoring  
9. Networking  
10. Firewalls  
11. Logs & Forensics  
12. Package Management  
13. Services & Daemons  
14. Bash & Automation  
15. Security Tools  
16. Linux in Cloud  
17. Advanced Security Concepts  
18. Learning Roadmap  

---

# 1. Why Linux Matters

Linux powers:

- Most web servers  
- Cloud virtual machines  
- Containers  
- Routers & IoT  
- Enterprise infrastructure  

Nearly all professional security tooling is Linux-first.

If you understand Linux deeply, you understand how real systems are attacked and defended.

---

# 2. Linux Architecture

Hardware
↓
Kernel
↓
Shell
↓
User Applications

## Kernel Responsibilities
- CPU scheduling  
- Memory allocation  
- Device management  
- Filesystems  
- Networking  

Security implication: kernel flaws can give attackers full control.

---

# 3. Shell & Command Line

The shell allows interaction with the operating system.

Common shells:
- bash
- sh
- zsh

Used for:
- administration  
- automation  
- exploitation  
- forensic investigation  

---

# 4. Filesystem Hierarchy

| Directory | Description | Security View |
|-----------|-------------|---------------|
| `/` | root | base of everything |
| `/bin` | user commands | execution paths |
| `/sbin` | admin commands | privilege targets |
| `/etc` | configs | high-value |
| `/home` | user files | data theft |
| `/var` | logs | incident evidence |
| `/tmp` | temporary | malware usage |
| `/proc` | runtime info | recon |
| `/opt` | optional software | third-party risk |

---

# 5. Users & Groups

Linux is multi-user.

## Types
- root (UID 0)  
- service accounts  
- standard users  

## Important files
- `/etc/passwd`
- `/etc/shadow`
- `/etc/group`

---

# 6. Permissions & Ownership

r = read
w = write
x = execute
 

Applied to:
- owner  
- group  
- others  

Common exploitation paths:
- world writable files  
- writable scripts  
- incorrect ownership  

---

# 7. Sudo & Privilege Escalation

Sudo allows temporary administrative access.


Attackers search for:
- NOPASSWD entries  
- SUID files  
- writable cron jobs  
- weak PATH settings  

---

# 8. Processes & Resource Monitoring

Every running program is a process.

## Viewing
ps aux
top
htop


## Killing


kill PID
kill -9 PID

Security use:
- detect malware  
- find persistence  
- analyze unusual CPU/memory  

---

# 9. Networking

## Interfaces
ip a


## Listening ports


netstat -tulnp
ss -lntup


## Connectivity


ping
traceroute


Used in:
- enumeration  
- lateral movement  
- detection  

---

# 10. Firewalls

Control incoming/outgoing traffic.

Tools:
- iptables  
- nftables  
- ufw  
- firewalld  

---

# 11. Logs & Forensics

Logs are critical after breaches.

| Location | Usage |
|----------|------|
| `/var/log/auth.log` | login attempts |
| `/var/log/syslog` | system messages |
| `/var/log/kern.log` | kernel |
| `/var/log/apache2/` | web attacks |

## Commands


tail -f file
grep "failed" file
journalctl


---

# 12. Package Management

Install and update software.

| Distribution | Manager |
|-------------|--------|
| Debian/Ubuntu | apt |
| RedHat/CentOS | yum / dnf |

Risks:
- outdated versions  
- vulnerable dependencies  
- malicious repos  

---

# 13. Services & Daemons

Background programs started by the system.



systemctl status ssh
systemctl start nginx
systemctl enable apache2


Misconfiguration can expose systems.

---

# 14. Bash & Automation

Used to automate:

- scans  
- updates  
- monitoring  
- data parsing  

Example:
```bash
for ip in $(cat hosts.txt); do
 ping -c 1 $ip
done

---

# 15. Security Tools (Explained)

## Offensive Tools

### nmap
Network discovery and security auditing tool.

Used for:
- host discovery  
- port scanning  
- service/version detection  
- OS fingerprinting  
- vulnerability scripts (NSE)

Why it matters: almost every penetration test starts with reconnaissance.

---

### metasploit
Exploitation framework.

Provides:
- exploit modules  
- payloads  
- post-exploitation tools  
- privilege escalation helpers  

Why it matters: standard platform for validating vulnerabilities.

---

### hydra
Online password brute-force tool.

Supports services like:
- SSH  
- FTP  
- HTTP  
- RDP  
- SMB  

Why it matters: used to test authentication strength.

---

### sqlmap
Automates SQL injection detection and exploitation.

Capabilities:
- database fingerprinting  
- dumping data  
- bypassing authentication  
- gaining OS shell (in some cases)

Why it matters: removes manual complexity in database attacks.

---

### nikto
Web server vulnerability scanner.

Detects:
- outdated software  
- dangerous files  
- misconfigurations  
- default installations  

Why it matters: quick overview of web exposure.

---

## Defensive Tools

### fail2ban
Intrusion prevention system.

Works by:
- monitoring logs  
- detecting repeated failures  
- banning IP addresses via firewall rules

Why it matters: protects against brute-force attacks.

---

### auditd
Native Linux auditing system.

Tracks:
- file access  
- command execution  
- permission changes  

Why it matters: critical for compliance and incident investigations.

---

### tcpdump
Command-line packet analyzer.

Used for:
- capturing network traffic  
- debugging connections  
- detecting suspicious communication

Why it matters: raw network visibility.

---

### wireshark
Graphical packet analyzer.

Features:
- deep protocol inspection  
- filtering  
- stream reconstruction  

Why it matters: powerful for malware traffic analysis.

---

### clamav
Open-source antivirus engine.

Used for:
- malware scanning  
- email gateway protection  
- file server defense

Why it matters: baseline threat detection.

---

## Forensics Tools

### strings
Extracts human-readable text from binaries.

Useful for:
- finding URLs  
- commands  
- hidden messages  
- indicators of compromise

---

### binwalk
Analyzes binary files and firmware.

Detects:
- embedded files  
- compressed data  
- executables

Used heavily in IoT and malware research.

---

### foremost
File carving tool.

Recovers deleted data based on file headers.

Important in:
- disk investigations  
- recovery after compromise

---

# 16. Linux in Cloud (Security Responsibilities)

Most cloud workloads run Linux, making administrators responsible for strong security posture.

---

## SSH Key Security
- disable password authentication  
- use strong key pairs  
- rotate keys  
- restrict root login  
- enforce least privilege  

---

## Firewall Configuration
- allow only required ports  
- deny unnecessary inbound traffic  
- restrict management interfaces  
- log blocked attempts  

---

## User Isolation
- separate service accounts  
- apply minimal permissions  
- avoid shared credentials  

---

## Patching
- regularly update OS and packages  
- remove unused software  
- monitor vulnerability disclosures  

Unpatched systems are among the most common breach causes.

---

## Logging & Alerting
- centralize logs  
- detect anomalies  
- create alerts for suspicious behavior  
- retain logs for investigations  

Without logs, incidents cannot be reconstructed.

---

# 17. Advanced Security Concepts

These topics differentiate junior engineers from senior professionals.

---

## DAC vs MAC

### Discretionary Access Control (DAC)
Resource owners decide who can access their files.

Example: traditional Linux permissions.

### Mandatory Access Control (MAC)
System enforces policy regardless of user ownership.

Example: SELinux or AppArmor.

MAC provides stronger containment.

---

## SELinux
Label-based access control system.

Every process and file has a security context.

Even root can be restricted.

Goal: minimize damage if a service is compromised.

---

## AppArmor
Path-based mandatory access control.

Profiles define what files and capabilities a program may access.

Simpler to configure than SELinux in many environments.

---

## Linux Capabilities
Breaks root privileges into smaller units.

Examples:
- bind to low ports  
- modify network settings  
- load kernel modules  

Principle: grant only what is required.

---

## Namespaces
Provide isolation between processes.

Types include:
- mount  
- network  
- PID  
- user  

Foundation of container security.

---

## Containers
Lightweight isolated environments sharing the host kernel.

Benefits:
- application isolation  
- portability  
- rapid deployment  

Security concerns:
- escape vulnerabilities  
- misconfigured privileges  

---

## ASLR (Address Space Layout Randomization)
Randomizes memory locations.

Makes exploitation harder by preventing predictable addresses.

---

## NX (No Execute Bit)
Marks memory regions as non-executable.

Stops many buffer overflow attacks from running injected code.

---

## Kernel Hardening
Techniques to reduce attack surface.

Includes:
- patching  
- disabling unused modules  
- restricting debug interfaces  
- enabling exploit mitigations  

---

# Summary

Mastery of these tools and concepts enables professionals to:

- detect attacks  
- prevent compromise  
- investigate incidents  
- build hardened systems  

They form the backbone of real-world cybersecurity operations.
