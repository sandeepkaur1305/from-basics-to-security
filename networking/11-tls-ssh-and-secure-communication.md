# 11 - TLS, SSH, and Secure Communication

## Table of Contents

* What is Secure Communication?
* Why Secure Communication is Needed
* TLS (Transport Layer Security)
* SSH (Secure Shell)
* SSH Authentication
* TLS vs SSH
* Telnet vs SSH
* Common Secure Ports
* Cybersecurity and SOC Relevance
* Useful Commands
* What I Learned

---

# What is Secure Communication?

Secure communication protects data while it travels across a network.

It ensures that unauthorized users cannot read or modify the data.

Secure communication provides:

* Confidentiality
* Integrity
* Authentication

Example:

```text id="d2m1gk"
Client
   |
Encrypted Communication
   |
Server
```

---

# Why Secure Communication is Needed

Without security, attackers may intercept network traffic.

Risks include:

* Password theft
* Data theft
* Session hijacking
* Man-in-the-Middle (MITM) attacks

Example:

```text id="bc7z2p"
Client
   |
Plain Text
   |
Attacker
   |
Server
```

With encryption:

```text id="pj9w4n"
Client
   |
Encrypted Data
   |
Attacker
(Cannot Read Data)
   |
Server
```

---

# TLS (Transport Layer Security)

TLS stands for **Transport Layer Security**.

TLS secures communication between applications.

It is commonly used by:

* HTTPS
* Email services
* APIs
* VPNs

TLS provides:

* Encryption
* Integrity
* Authentication

HTTPS is simply:

```text id="4g91yq"
HTTP + TLS
```

Default HTTPS port:

```text id="3z7lcv"
TCP 443
```

---

## TLS Handshake (Simple)

Before encrypted communication begins, the client and server perform a TLS handshake.

Simplified process:

```text id="x8kbzm"
Client
   |
Client Hello
   |
Server Hello
Certificate
   |
Shared Keys Created
   |
Encrypted Communication
```

The handshake helps both sides establish secure communication.

---

# SSH (Secure Shell)

SSH stands for **Secure Shell**.

SSH allows secure remote access to another computer.

Example:

```text id="7q4nmb"
Laptop
   |
SSH
   |
Linux Server
```

SSH is commonly used by:

* Linux administrators
* Network engineers
* SOC analysts
* Cloud administrators

Default Port:

```text id="0cmxfr"
TCP 22
```

---

# SSH Authentication

SSH supports two common authentication methods.

## Password Authentication

The user enters:

* Username
* Password

Example:

```text id="g0e1iw"
Username:
alex

Password:
********
```

---

## Key-Based Authentication

Instead of a password, SSH can use a pair of cryptographic keys.

```text id="3j0z0v"
Private Key
     +
Public Key
```

The public key is stored on the server.

The private key stays with the user.

Key-based authentication is generally more secure than password authentication.

---

# TLS vs SSH

| TLS                               | SSH                          |
| --------------------------------- | ---------------------------- |
| Secures application communication | Secure remote login          |
| Commonly used with HTTPS          | Remote server administration |
| Default Port 443                  | Default Port 22              |
| Protects web traffic              | Protects terminal sessions   |

Simple memory:

```text id="40yr2i"
TLS
Secure Websites
```

```text id="prhmp8"
SSH
Secure Remote Access
```

---

# Telnet vs SSH

| Telnet            | SSH         |
| ----------------- | ----------- |
| No encryption     | Encrypted   |
| Port 23           | Port 22     |
| Insecure          | Secure      |
| Rarely used today | Widely used |

Example:

```text id="n8yzgz"
Telnet

Username
Password

Plain Text
```

```text id="b4b6n8"
SSH

Username
Password

Encrypted
```

SSH has largely replaced Telnet for remote administration.

---

# Common Secure Ports

| Service     | Port |
| ----------- | ---: |
| SSH         |   22 |
| HTTPS (TLS) |  443 |
| SFTP        |   22 |
| SMTPS       |  465 |
| IMAPS       |  993 |
| POP3S       |  995 |

---

# Cybersecurity and SOC Relevance

Secure communication is an important part of security monitoring.

## 1. Failed SSH Logins

Repeated failed SSH login attempts may indicate:

* Brute-force attacks
* Password guessing

Example:

```text id="5mxv5f"
User: root

Status: Failed

Source IP: 203.x.x.x
```

---

## 2. Suspicious SSH Access

SOC analysts investigate:

* Logins outside business hours
* Unknown source IP addresses
* Multiple failed logins followed by a successful login
* Unexpected root logins

---

## 3. Weak TLS Versions

Older protocols such as:

* SSL 2.0
* SSL 3.0
* TLS 1.0

are considered outdated and should be replaced with modern TLS versions where possible.

---

## 4. Certificate Issues

Analysts may investigate:

* Expired certificates
* Self-signed certificates
* Certificate mismatches
* Invalid certificate chains

These issues can affect secure communication and may indicate configuration problems or, in some cases, suspicious activity.

---

## 5. Splunk Example

Example event:

```text id="3kg1e4"
src_ip=192.168.1.25

protocol=SSH

user=root

status=Failed
```

Possible investigation questions:

* How many failed logins occurred?
* Which IP generated them?
* Was there a successful login afterward?
* Is the source IP trusted?

---

# Useful Commands

Generate SSH keys:

```bash id="phmqm7"
ssh-keygen
```

Connect to a server:

```bash id="iwy40f"
ssh user@192.168.1.10
```

Copy a file securely:

```bash id="eqarfw"
scp file.txt user@192.168.1.10:/home/user/
```

Start an SFTP session:

```bash id="vq8m7e"
sftp user@192.168.1.10
```

Inspect a TLS certificate:

```bash id="zwzjbm"
openssl s_client -connect example.com:443
```

---

# What I Learned

After completing this section, I learned:

* Secure communication protects data transmitted over a network.
* Encryption helps prevent unauthorized users from reading network traffic.
* TLS stands for Transport Layer Security.
* TLS is commonly used to secure HTTPS, email, and other network services.
* HTTPS uses TLS to encrypt web communication.
* SSH stands for Secure Shell.
* SSH provides secure remote access to systems.
* SSH commonly uses TCP port 22.
* HTTPS commonly uses TCP port 443.
* SSH supports both password and key-based authentication.
* Key-based authentication is generally more secure than password authentication.
* Telnet sends data in plain text, while SSH encrypts communication.
* TLS protects application traffic, whereas SSH is primarily used for secure remote administration.
* SOC analysts monitor failed SSH logins, unusual remote access, weak TLS versions, and certificate issues during investigations.
