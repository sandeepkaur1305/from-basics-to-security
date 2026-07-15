# 12 - Network Security Fundamentals

## Table of Contents

* What is Network Security?
* Why Network Security is Important
* CIA Triad
* Types of Network Attacks
* Firewalls
* Intrusion Detection System (IDS)
* Intrusion Prevention System (IPS)
* Network Segmentation
* Virtual Private Network (VPN)
* Network Access Control (NAC)
* Zero Trust
* Security Best Practices
* Cybersecurity and SOC Relevance
* Common Network Security Ports
* What I Learned

---

# What is Network Security?

Network security is the practice of protecting computer networks, devices, and data from unauthorized access, attacks, and misuse.

Its goal is to ensure that only authorized users and devices can communicate on the network.

Example:

```text id="m2y1ab"
Users
   |
Secure Network
   |
Servers
```

---

# Why Network Security is Important

Without network security, attackers may:

* Steal sensitive information
* Install malware
* Disrupt business operations
* Gain unauthorized access

Good network security helps protect:

* Devices
* Users
* Applications
* Data

---

# CIA Triad

The CIA Triad is the foundation of information security.

| Principle       | Meaning                                      |
| --------------- | -------------------------------------------- |
| Confidentiality | Prevent unauthorized access to data          |
| Integrity       | Prevent unauthorized modification of data    |
| Availability    | Ensure systems remain accessible when needed |

Example:

```text id="o91jmk"
Confidentiality
      ↓
Only Authorized Users
```

```text id="jlwm9d"
Integrity
      ↓
Data Cannot Be Modified Without Authorization
```

```text id="ep4vla"
Availability
      ↓
Systems Stay Online
```

---

# Types of Network Attacks

Some common network attacks include:

* Brute-force attacks
* Denial of Service (DoS)
* Distributed Denial of Service (DDoS)
* Man-in-the-Middle (MITM)
* ARP Spoofing
* DNS Spoofing
* Packet Sniffing
* Port Scanning

These attacks may attempt to:

* Steal credentials
* Intercept traffic
* Disrupt services
* Gather information

---

# Firewalls

A firewall monitors and controls network traffic based on security rules.

Example:

```text id="lq3n4p"
Internet
    |
Firewall
    |
Internal Network
```

A firewall can:

* Allow traffic
* Block traffic
* Log traffic

Example rule:

```text id="zt2ykr"
Allow HTTPS (443)

Block Telnet (23)
```

Firewalls can be:

* Host-based
* Network-based

---

# Intrusion Detection System (IDS)

An IDS monitors network traffic for suspicious activity.

It generates alerts but does not automatically block traffic.

Example:

```text id="b64mte"
Network Traffic
      |
IDS
      |
Alert
```

Example detection:

```text id="q8zp4y"
Port Scan Detected
```

---

# Intrusion Prevention System (IPS)

An IPS monitors traffic like an IDS but can also block malicious activity.

Example:

```text id="o0x7ns"
Attacker
    |
Malicious Traffic
    |
IPS
    |
Blocked
```

Difference:

| IDS        | IPS                 |
| ---------- | ------------------- |
| Detects    | Detects and blocks  |
| Alert only | Can prevent attacks |

---

# Network Segmentation

Network segmentation divides a network into smaller sections.

Example:

```text id="4u6l8a"
HR VLAN

Finance VLAN

IT VLAN

Guest VLAN
```

Benefits:

* Limits attacker movement
* Improves security
* Reduces broadcast traffic
* Simplifies access control

---

# Virtual Private Network (VPN)

A VPN creates an encrypted tunnel between a user and a network.

Example:

```text id="z4nqtc"
Remote User
      |
Encrypted Tunnel
      |
Company Network
```

Benefits:

* Encrypts traffic
* Protects data on public Wi-Fi
* Enables secure remote access

---

# Network Access Control (NAC)

NAC controls which devices can connect to a network.

It checks whether a device meets security requirements before allowing access.

Example checks:

* Registered device
* Antivirus installed
* Updated operating system

---

# Zero Trust

Zero Trust follows the principle:

> Never Trust, Always Verify.

Instead of automatically trusting devices inside the network, every access request is verified.

Key ideas:

* Verify identity
* Use least privilege
* Continuously validate users and devices

---

# Security Best Practices

Common network security practices include:

* Use strong passwords
* Enable Multi-Factor Authentication (MFA)
* Keep systems updated
* Disable unused services
* Use HTTPS instead of HTTP
* Use SSH instead of Telnet
* Regularly back up important data
* Monitor logs
* Apply the Principle of Least Privilege

---

# Cybersecurity and SOC Relevance

SOC analysts monitor network traffic for suspicious activity.

Common investigations include:

### Brute-force attacks

Multiple failed login attempts from one IP.

### Port scanning

One host attempts to connect to many ports.

### Malware communication

An infected device communicates with external servers.

### Unusual outbound traffic

Large uploads to unknown destinations may indicate data exfiltration.

### VPN logins

Analysts review:

* Login time
* Source IP
* User
* Location

### Firewall logs

Firewall logs commonly contain:

* Source IP
* Destination IP
* Port
* Protocol
* Action (Allow or Deny)

Example:

```text id="w8i2ph"
src_ip=192.168.1.50

dest_ip=8.8.8.8

dest_port=443

action=Allowed
```

### IDS/IPS Alerts

Example:

```text id="p0s8vl"
Alert:
Possible Port Scan

Source:
203.x.x.x
```

Analysts investigate whether the activity is malicious or legitimate.

---

# Common Network Security Ports

| Service | Port |
| ------- | ---: |
| SSH     |   22 |
| Telnet  |   23 |
| HTTP    |   80 |
| HTTPS   |  443 |
| FTP     |   21 |
| SFTP    |   22 |
| DNS     |   53 |
| SMTP    |   25 |
| POP3    |  110 |
| IMAP    |  143 |

Remember:

* 22 → SSH
* 23 → Telnet
* 53 → DNS
* 80 → HTTP
* 443 → HTTPS

---

# What I Learned

After completing this section, I learned:

* Network security protects networks, devices, and data from unauthorized access and attacks.
* The CIA Triad consists of Confidentiality, Integrity, and Availability.
* Common network attacks include brute-force attacks, DoS, DDoS, ARP spoofing, DNS spoofing, packet sniffing, and port scanning.
* Firewalls filter network traffic based on security rules.
* IDS detects suspicious activity and generates alerts.
* IPS detects and can block malicious traffic.
* Network segmentation improves security by dividing a network into smaller sections.
* VPNs provide encrypted communication over untrusted networks.
* NAC verifies devices before allowing them to access the network.
* Zero Trust follows the principle of "Never Trust, Always Verify."
* Security best practices include strong passwords, MFA, software updates, secure protocols, backups, and log monitoring.
* SOC analysts investigate firewall logs, IDS/IPS alerts, VPN activity, brute-force attacks, malware communication, and unusual network traffic.
* Common security-related ports include SSH (22), Telnet (23), DNS (53), HTTP (80), and HTTPS (443).
