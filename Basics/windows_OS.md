# 🪟 Windows Operating System – Security & Architecture Documentation

This repository contains clear, beginner‑friendly documentation on Windows Operating System internals, security, and attack concepts. It is designed for students, cybersecurity beginners, and learners who want a structured understanding of how Windows works under the hood, from boot process and kernel to security mechanisms and common attack techniques.

The notes are written in simple language, focused on concept clarity, exams, interviews, and incident‑response fundamentals, making this repo suitable for learning, revision, and reference.

---

## 1️⃣ Windows Architecture

Windows follows a **hybrid kernel architecture** with clear separation between **User Mode** and **Kernel Mode**.

### User Mode

* Runs applications
* No direct hardware access
* Crash does NOT crash the system

Examples:

* Browsers, editors, games

### Kernel Mode

* Full hardware access
* Runs OS core components
* Crash causes **BSOD**

Components:

* Windows Kernel
* Executive services
* Device drivers
* HAL (Hardware Abstraction Layer)

---

## 2️⃣ Kernel (ntoskrnl.exe)

* Core of Windows OS
* Manages:

  * CPU scheduling
  * Memory
  * Hardware communication
  * System calls

📌 Linux equivalent: **Linux kernel (`/boot/vmlinuz`)**

---

## 3️⃣ Windows Boot Process

1. Power ON
2. BIOS / UEFI (POST)
3. Windows Boot Manager (`bootmgr`)
4. OS Loader (`winload.exe`)
5. Kernel (`ntoskrnl.exe`)
6. System services
7. User login

Security features:

* Secure Boot
* TPM
* BitLocker

---

## 4️⃣ Secure Boot

* Ensures only **trusted, digitally signed software** runs at boot
* Blocks bootkits & rootkits
* Uses UEFI + cryptographic signatures

📌 Required for Windows 11

---

## 5️⃣ TPM (Trusted Platform Module)

* Hardware-based security chip
* Stores encryption keys securely
* Keys are **never readable**
* Used by:

  * BitLocker
  * Windows Hello
  * Secure Boot

---

## 6️⃣ BitLocker Disk Encryption

* Encrypts the **entire disk**
* Uses:

  * FVEK (encrypts data)
  * VMK (protects FVEK)
* TPM protects VMK

📌 TPM = key protector
📌 BitLocker = disk lock

---

## 7️⃣ Windows File System (NTFS)

### NTFS Features

* Permissions (ACLs)
* Encryption (EFS)
* Compression
* Journaling
* Large file support

### NTFS Metadata

Stored in **MFT (Master File Table)**:

* File names
* Permissions
* Timestamps
* Disk locations

---

## 8️⃣ ADS (Alternate Data Streams)

* NTFS feature to store **hidden data** with files
* Not visible in File Explorer

Used for:

* Metadata (Zone.Identifier)
* Sometimes abused by malware

Command to view:

```
dir /r
```

---

## 9️⃣ Important Windows Directories

| Directory                 | Purpose                 |
| ------------------------- | ----------------------- |
| C:\Windows                | OS files                |
| System32                  | Core DLLs & executables |
| SysWOW64                  | 32-bit system files     |
| Program Files             | 64-bit apps             |
| Program Files (x86)       | 32-bit apps             |
| Users                     | User data               |
| AppData                   | App configs & cache     |
| ProgramData               | Shared app data         |
| System Volume Information | Restore points          |

---

## 🔟 DLL (Dynamic Link Library)

* Shared code used by multiple programs
* Saves space & improves performance

Security risk:

* **DLL hijacking** (fake DLL loaded instead of real one)

---

## 1️⃣1️⃣ Users, Groups & Privileges

### Users

* Administrator
* Standard User

### Groups

* Administrators
* Users
* Backup Operators
* Remote Desktop Users

### Privileges

* Shutdown system
* Load drivers
* Take ownership

---

## 1️⃣2️⃣ UAC (User Account Control)

* Prevents unauthorized admin actions
* Even admins run with limited rights
* Requires confirmation for elevated tasks

---

## 1️⃣3️⃣ Authentication & Authorization

* **Authentication** → Who are you?
* **Authorization** → What can you do?

Used by:

* NTFS permissions
* UAC
* Security tokens

---

## 1️⃣4️⃣ Windows Hello

* Passwordless login system
* Uses:

  * Face recognition
  * Fingerprint
  * PIN
* Protected by TPM

---

## 1️⃣5️⃣ Security Identifiers (SID)

* Unique ID for users & groups
* Windows trusts SID, not username

Example:

```
S-1-5-21-...
```

---

## 1️⃣6️⃣ Windows Registry

* Central configuration database

### Registry Structure

* Keys → folders
* Values → settings

### Important Hives

* HKLM – system-wide
* HKCU – current user
* HKU – all users
* HKCR – file associations

---

## 1️⃣7️⃣ Processes & Services

* **Process** → running program
* **Service** → background system task

Tools:

* Task Manager
* services.msc

---

## 1️⃣8️⃣ Windows Firewall

### Features

* Network traffic control
* Inbound & outbound rules

### Profiles

* Domain (office)
* Private (home)
* Public (public Wi‑Fi)

---

## 1️⃣9️⃣ Windows Defender Security

### Built-in Protections

* Antivirus
* Firewall
* Exploit Guard
* Controlled Folder Access

### Attacker Attempts

* Defender evasion
* AMSI bypass

---

## 2️⃣0️⃣ Malware Types

### Malware

* Any malicious software

### Virus

* Infects files

### Trojan

* Disguised as legit software

### Worm

* Self-spreading malware

### Rootkit

* Hides malware deeply in system

### Bootkit

* Infects boot process

---

## 2️⃣1️⃣ AMSI (Antimalware Scan Interface)

* Scans scripts before execution
* Used with PowerShell, JS, VBA

AMSI bypass = attacker attempts to evade script scanning

---

## 2️⃣2️⃣ Event Logs & Monitoring

### Log Types

* Security
* System
* Application

### Important Security Events

* Logon failures
* Privilege use
* Process creation

📁 Stored at:

```
C:\Windows\System32\winevt\Logs
```

---

## 2️⃣3️⃣ Common Windows Attacks

* Password dumping (LSASS)
* Pass-the-Hash
* DLL hijacking
* UAC bypass
* RDP brute force

---

## 2️⃣4️⃣ RDP (Remote Desktop Protocol)

* Allows remote control of Windows systems
* Uses port 3389

Attack risk:

* Brute force
* Credential reuse

---

## 2️⃣5️⃣ Active Directory (AD)

* Centralized management system
* Manages:

  * Users
  * Computers
  * Permissions

Used in:

* Companies
* Enterprises

---

## 📌 Final Note

This document is designed for:

* Cybersecurity beginners
* Windows internals understanding
* Incident response basics
* GitHub documentation

---

✅ **End of Windows OS Documentation**
