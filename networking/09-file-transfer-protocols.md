# 09 - File Transfer Protocols

## Table of Contents

* What is File Transfer?
* Why File Transfer Protocols are Needed
* FTP (File Transfer Protocol)
* FTP Working
* FTP Ports
* FTP Commands
* TFTP (Trivial File Transfer Protocol)
* SFTP (SSH File Transfer Protocol)
* SCP (Secure Copy Protocol)
* FTP vs TFTP vs SFTP vs SCP
* Cybersecurity and SOC Relevance
* Useful Commands
* What I Learned

---

# What is File Transfer?

File transfer is the process of sending files from one computer to another over a network.

Examples:

* Uploading a website to a server
* Downloading software
* Backing up router configurations
* Copying files between Linux servers

Example:

```text
Computer A
     |
     | File
     |
Network
     |
     |
Computer B
```

---

# Why File Transfer Protocols are Needed

Computers need standard rules to exchange files.

These rules are called **file transfer protocols**.

They define:

* How files are transferred
* How users authenticate
* Whether data is encrypted
* How errors are handled

Common file transfer protocols include:

* FTP
* TFTP
* SFTP
* SCP

---

# FTP (File Transfer Protocol)

FTP stands for **File Transfer Protocol**.

It is one of the oldest protocols used for transferring files.

FTP works at the **Application Layer (Layer 7)**.

By default, FTP **does not encrypt** usernames, passwords, or files.

Example:

```text
Client
   |
   | FTP
   |
Server
```

Common uses:

* Uploading website files
* Downloading software
* File sharing inside trusted networks

---

# FTP Working

A typical FTP session works like this:

```text
Client
   |
Connect to Server
   |
Login
   |
Upload / Download Files
   |
Disconnect
```

FTP uses two separate connections:

* Control Connection
* Data Connection

The control connection manages commands.

The data connection transfers files.

---

# FTP Ports

FTP commonly uses:

| Port   | Purpose                      |
| ------ | ---------------------------- |
| TCP 21 | Control Connection           |
| TCP 20 | Data Connection (Active FTP) |

Remember:

```text
21 → Commands

20 → File Data
```

---

# Common FTP Commands

| Command | Purpose                |
| ------- | ---------------------- |
| USER    | Username               |
| PASS    | Password               |
| LIST    | List files             |
| PWD     | Show current directory |
| CWD     | Change directory       |
| GET     | Download file          |
| PUT     | Upload file            |
| DELETE  | Delete file            |
| QUIT    | Close connection       |

Example:

```text
USER admin
PASS password123
LIST
GET report.pdf
QUIT
```

---

# Why FTP is Insecure

FTP sends data in plain text.

That means an attacker capturing network traffic may see:

```text
Username

Password

Transferred Files
```

Example:

```text
USER admin

PASS admin123
```

Because of this, FTP is generally avoided on untrusted networks.

---

# TFTP (Trivial File Transfer Protocol)

TFTP stands for **Trivial File Transfer Protocol**.

It is a simplified version of FTP.

Unlike FTP:

* Uses UDP
* No authentication
* No encryption
* Very simple

Uses:

* Router configuration backup
* Network device firmware updates
* PXE network boot

Default Port:

```text
UDP 69
```

---

# SFTP (SSH File Transfer Protocol)

SFTP stands for **SSH File Transfer Protocol**.

It transfers files securely using SSH.

Default Port:

```text
TCP 22
```

Features:

* Encrypted communication
* Secure login
* Secure file transfer
* Commonly used in Linux servers

Example:

```text
Client
    |
Encrypted SSH Connection
    |
Server
```

Unlike FTP, usernames and passwords are encrypted.

---

# SCP (Secure Copy Protocol)

SCP stands for **Secure Copy Protocol**.

It also works over SSH.

Default Port:

```text
TCP 22
```

It is mainly used to quickly copy files between Linux systems.

Example:

```bash
scp report.pdf user@192.168.1.10:/home/user/
```

SCP is simple and fast but provides fewer file management features than SFTP.

---

# FTP vs TFTP vs SFTP vs SCP

| Feature         | FTP    | TFTP     | SFTP   | SCP    |
| --------------- | ------ | -------- | ------ | ------ |
| Default Port    | TCP 21 | UDP 69   | TCP 22 | TCP 22 |
| Encryption      | ❌ No   | ❌ No     | ✅ Yes  | ✅ Yes  |
| Authentication  | ✅ Yes  | ❌ No     | ✅ Yes  | ✅ Yes  |
| File Management | ✅ Yes  | Limited  | ✅ Yes  | Basic  |
| Security        | Low    | Very Low | High   | High   |

Simple rule:

```text
FTP
Old and insecure
```

```text
TFTP
Very simple
```

```text
SFTP
Secure and recommended
```

```text
SCP
Secure file copy
```

---

# Cybersecurity and SOC Relevance

File transfer protocols are important because attackers often use them to steal or upload files.

## 1. FTP Credentials

Since FTP sends credentials in plain text, packet captures may reveal:

```text
Username

Password
```

SOC analysts should identify FTP traffic on untrusted networks.

---

## 2. Data Exfiltration

Attackers may upload sensitive company files to external FTP or SFTP servers.

Example:

```text
Employee PC
      |
Sensitive Files
      |
External FTP Server
```

Possible indicators:

* Large file uploads
* Connections to unknown FTP servers
* Transfers outside business hours

---

## 3. Secure File Transfer

Organizations usually prefer:

* SFTP
* SCP

because both encrypt transferred data.

---

## 4. Wireshark

Wireshark can capture FTP traffic.

You may observe:

* USER command
* PASS command
* LIST
* File uploads
* File downloads

For SFTP, only encrypted SSH traffic is visible.

---

## 5. Splunk Logs

SOC analysts may search for:

* FTP login failures
* Large uploads
* Multiple download attempts
* Connections to unusual external servers

Example event:

```text
src_ip=192.168.1.50

protocol=FTP

action=UPLOAD

file=finance.xlsx
```

---

# Useful Commands

## Linux

Connect to an FTP server:

```bash
ftp server_ip
```

Connect using SFTP:

```bash
sftp user@server_ip
```

Copy file using SCP:

```bash
scp file.txt user@server_ip:/home/user/
```

Copy directory:

```bash
scp -r folder user@server_ip:/home/user/
```

---

## Windows

FTP client:

```cmd
ftp
```

Open an FTP connection:

```cmd
open 192.168.1.10
```

Show active connections:

```cmd
netstat -ano
```

---

# What I Learned

After completing this section, I learned:

* File transfer protocols allow computers to exchange files over a network.
* FTP stands for File Transfer Protocol.
* FTP commonly uses TCP port 21 for control connections.
* FTP transfers data without encryption.
* FTP credentials can be captured on untrusted networks.
* TFTP stands for Trivial File Transfer Protocol.
* TFTP uses UDP port 69.
* TFTP is commonly used for network devices and PXE boot.
* SFTP stands for SSH File Transfer Protocol.
* SFTP uses TCP port 22.
* SFTP encrypts both authentication and file transfers.
* SCP also uses SSH and TCP port 22.
* SCP is mainly used for secure file copying.
* SFTP and SCP are preferred over FTP on modern networks.
* Attackers may use file transfer protocols for data exfiltration.
* SOC analysts monitor file transfer logs for suspicious uploads, downloads, and authentication attempts.
* Wireshark can easily analyze FTP traffic because it is not encrypted, while SFTP traffic appears encrypted.
