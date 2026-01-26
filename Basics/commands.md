# Command Line Cheat Sheet & Notes

This repository documents essential **Windows CMD**, **PowerShell**, and **Linux** commands I have learned, with short explanations and common usage patterns.

---

## 📌 Windows Command Prompt (CMD)

### 🔧 System & Environment

* **`set` / `set PATH`** – View or set environment variables (PATH controls where commands are searched).
* **`ver`** – Show Windows version.
* **`systeminfo`** – Display detailed system configuration.

### 🧠 Drivers & Processes

* **`driverquery`** – List installed device drivers.
* **`driverquery | more`** – View driver list page by page (Space = next page, `Ctrl + C` = exit).
* **`tasklist`** – Show running processes.
* **`taskkill /PID <PID>`** – Kill a process by PID.

### 🌐 Networking

* **`ipconfig /all`** – Show complete network configuration.
* **`nslookup`** – Query DNS information.
* **`netstat`** – Display network connections and listening ports.

### 📁 File & Directory Management

* **`cd`** – Show or change current directory.
* **`dir`** – List files and directories.
* **`dir /a`** – Show hidden files.
* **`dir /s`** – List files in current directory and all subdirectories.
* **`copy`** – Copy files.
* **`type <file>`** – Display file contents.
* **`del` / `erase`** – Delete files.

### 🛠 Disk & System Health

* **`chkdsk`** – Check disk and file system for errors.
* **`sfc /scannow`** – Scan and repair corrupted system files.

### 📄 Help & Paging

* **`help <command>`** – Get help for a command.
* **`/?`** – Display command help page.
* **`more <file>.txt`** – View file contents page by page.

---

## ⚡ PowerShell

### 🧩 PowerShell Basics

* **Verb–Noun syntax** (e.g., `Get-Process`, `Set-Location`).
* PowerShell is **object-based**, not text-based like CMD.

### 🔍 Discovery & Help

* **`Get-Command`** – List available commands.

  * `Get-Command -CommandType Function`
  * `Get-Command -Name "<command>"`
* **`Get-Help`** – View help for cmdlets.
* **`Get-Alias`** – List all aliases.

### 📦 Modules

* **`Find-Module -Name "PowerShell*"`** – Search modules.
* **`Install-Module -Name PowerShellGet`** – Install module.

### 📁 Files & Navigation

* **`Get-ChildItem`** – List items (alias: `dir`).
* **`Set-Location`** – Change directory (alias: `cd`).
* **`New-Item`** – Create file or directory.
* **`Remove-Item`** – Delete file or directory.
* **`Copy-Item`** – Copy files or directories.
* **`Get-Content`** – Read file content (like `cat`).

### 🔎 Filtering & Sorting

* **`Sort-Object Length`** – Sort by file size.
* **`Where-Object`** – Filter objects:

  * By extension: `Where-Object Extension -eq ".txt"`
  * By name: `Where-Object Name -like "ship*"`
* **`Select-Object`** – Select output:

  * `-First 1` → first item
  * `-Skip 1 -First 1` → second item
* **`Select-String`** – Search text (like `grep`).

### 🖥 System & Network Cmdlets

* **`Get-ComputerInfo` / `Get-SystemInfo`** – System information.
* **`Get-LocalUser`** – List local users.
* **`Get-Process`** – Show running processes.
* **`Get-Service`** – List services.
* **`Get-NetIPConfiguration`** – Network config.
* **`Get-NetIPAddress`** – IP address info.
* **`Get-NetTCPConnection`** – Active TCP connections.
* **`Get-FileHash`** – Generate file hash.
* **`Invoke-Command`** – Run commands locally or remotely.

---

## 🐧 Linux Commands

### 🖥 Shell & Environment

* **`echo $SHELL`** – Display current shell.
* **`cat /etc/shells`** – List available shells.
* **`chsh -s /usr/bin/zsh`** – Change default shell.

### 📜 History & Input

* **`history`** – Show previously executed commands.
* **`read`** – Take user input.

---

## 📚 Notes

* **CMD** → Text-based, legacy Windows shell.
* **PowerShell** → Object-based, powerful for automation.
* **Linux shell** → Flexible, script-friendly, widely used on servers.

---

## 🌱 Additional Beginner-Friendly Commands (Recommended)

### 🪟 Windows CMD (Beginner Boost)

* **`cls`** – Clear the screen.
* **`echo`** – Print text or variables to the console.
* **`whoami`** – Display current logged-in user.
* **`hostname`** – Show computer name.
* **`ping <host>`** – Check network connectivity.
* **`tracert <host>`** – Trace route to a destination.
* **`shutdown /s /t 0`** – Shut down system immediately.
* **`shutdown /r /t 0`** – Restart system immediately.
* **`tree`** – Display directory structure in tree format.
* **`assoc`** – Display file extension associations.
* **`fc file1 file2`** – Compare two files.

---

### ⚡ PowerShell (Beginner Boost)

* **`Clear-Host`** – Clear the screen.
* **`Get-Date`** – Display current date and time.
* **`Get-History`** – Show command history.
* **`Measure-Object`** – Count, sum, average objects.
* **`Test-Path`** – Check if file or path exists.
* **`Start-Process`** – Start a new process.
* **`Stop-Process -Name <name>`** – Stop a process by name.
* **`Get-ExecutionPolicy`** – Check script execution policy.
* **`Set-ExecutionPolicy`** – Change execution policy (admin).
* **`Export-Csv`** – Export data to CSV file.
* **`Import-Csv`** – Import data from CSV file.

---

### 🐧 Linux (Beginner Boost)

* **`pwd`** – Print current directory path.
* **`ls`** – List files and directories.
* **`ls -la`** – List including hidden files.
* **`cd ..`** – Move one directory up.
* **`touch file.txt`** – Create empty file.
* **`mkdir dir_name`** – Create directory.
* **`rm file.txt`** – Delete file.
* **`rm -r dir_name`** – Delete directory recursively.
* **`cp src dest`** – Copy files.
* **`mv src dest`** – Move or rename files.
* **`clear`** – Clear terminal screen.
* **`whoami`** – Show current user.
* **`uname -a`** – Display system information.
* **`df -h`** – Disk space usage.
* **`free -h`** – Memory usage.

---

## ⭐ Purpose of This Repository

* Quick revision before exams/interviews
* Personal reference while practicing system administration
* Foundation for Linux & Windows administration learning

---

Feel free to fork ⭐ and contribute!
