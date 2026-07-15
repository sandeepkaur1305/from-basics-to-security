# 10 - Email Protocols

## Table of Contents

* What is Email?
* How Email Works
* SMTP (Simple Mail Transfer Protocol)
* POP3 (Post Office Protocol Version 3)
* IMAP (Internet Message Access Protocol)
* SMTP vs POP3 vs IMAP
* Email Security
* Common Email Ports
* Cybersecurity and SOC Relevance
* Useful Commands
* What I Learned

---

# What is Email?

Email (Electronic Mail) is a method of sending and receiving digital messages over a network or the Internet.

An email can contain:

* Text
* Images
* Documents
* Audio
* Video
* Other file attachments

Example:

```text
Alice
   |
   | Email
   |
Internet
   |
   |
Bob
```

Email is one of the most widely used Internet services.

---

# How Email Works

Sending an email involves multiple servers.

Example:

```text
Sender
   |
SMTP
   |
Mail Server
   |
Internet
   |
Recipient Mail Server
   |
IMAP / POP3
   |
Receiver
```

Simple process:

1. User writes an email.
2. SMTP sends the email to the sender's mail server.
3. The sender's mail server forwards it to the recipient's mail server.
4. The recipient accesses the email using IMAP or POP3.

---

# SMTP (Simple Mail Transfer Protocol)

SMTP stands for **Simple Mail Transfer Protocol**.

SMTP is used to **send emails**.

SMTP is responsible for:

* Sending emails from users to mail servers
* Sending emails between mail servers

SMTP **does not retrieve emails**.

Example:

```text
Email Client
      |
      | SMTP
      |
Mail Server
```

Common SMTP Ports:

| Port | Purpose                                          |
| ---- | ------------------------------------------------ |
| 25   | Mail server communication                        |
| 587  | Secure email submission                          |
| 465  | SMTP over SSL/TLS (legacy but still widely used) |

Remember:

```text
SMTP = Send Mail
```

---

# POP3 (Post Office Protocol Version 3)

POP3 stands for **Post Office Protocol Version 3**.

POP3 is used to **download emails** from a mail server to a device.

Example:

```text
Mail Server
     |
POP3
     |
Laptop
```

Characteristics:

* Downloads emails to the local device.
* Can remove emails from the server after download.
* Suitable when using one device.

Default Ports:

| Port | Purpose           |
| ---- | ----------------- |
| 110  | POP3              |
| 995  | POP3 over SSL/TLS |

Remember:

```text
POP3 = Download Mail
```

---

# IMAP (Internet Message Access Protocol)

IMAP stands for **Internet Message Access Protocol**.

IMAP allows users to access emails while keeping them stored on the mail server.

Example:

```text
Laptop
      |
      |
Mail Server
      |
      |
Phone
```

Both devices see the same mailbox.

Characteristics:

* Emails remain on the server.
* Supports multiple devices.
* Synchronizes folders and read/unread status.

Default Ports:

| Port | Purpose           |
| ---- | ----------------- |
| 143  | IMAP              |
| 993  | IMAP over SSL/TLS |

Remember:

```text
IMAP = Sync Mail
```

---

# SMTP vs POP3 vs IMAP

| Feature              | SMTP       | POP3           | IMAP              |
| -------------------- | ---------- | -------------- | ----------------- |
| Purpose              | Send Email | Download Email | Synchronize Email |
| Upload               | Yes        | No             | No                |
| Download             | No         | Yes            | Yes               |
| Keeps Mail on Server | No         | Usually No     | Yes               |
| Multiple Devices     | No         | Limited        | Yes               |

Simple memory trick:

```text
SMTP
Send
```

```text
POP3
Download
```

```text
IMAP
Synchronize
```

---

# Email Security

Modern email communication commonly uses encryption.

Examples:

* SMTP with TLS
* IMAPS
* POP3S

Encryption helps protect:

* Usernames
* Passwords
* Email contents

Without encryption:

```text
Username

Password

Email Content
```

may be visible if intercepted on an untrusted network.

---

## Email Attachments

Emails often contain attachments.

Examples:

* PDF
* DOCX
* ZIP
* Images

Attachments should always be treated carefully.

Never open unexpected attachments from unknown senders.

---

## Spam

Spam refers to unwanted or unsolicited emails.

Examples:

* Advertisements
* Fake offers
* Scams

Spam filters help reduce unwanted emails.

---

## Phishing

Phishing is an attack where attackers send fake emails to trick users.

Example:

```text
Bank Email
      ↓
Click Here
      ↓
Fake Login Page
```

Attackers may attempt to steal:

* Passwords
* Credit card information
* Personal data

Always verify the sender and links before clicking.

---

# Common Email Ports

| Protocol | Default Port | Secure Port |
| -------- | -----------: | ----------: |
| SMTP     |           25 |   587 / 465 |
| POP3     |          110 |         995 |
| IMAP     |          143 |         993 |

These port numbers are commonly seen in firewalls, SIEM logs, and packet captures.

---

# Cybersecurity and SOC Relevance

Email is one of the most common attack vectors.

## 1. Phishing Emails

SOC analysts investigate suspicious emails containing:

* Fake login pages
* Malicious links
* Malware attachments

---

## 2. Malware Delivery

Attackers often send malware through email attachments.

Example:

```text
Attacker
    |
Malicious Attachment
    |
Victim
```

Possible attachment types include:

* ZIP archives
* Office documents with macros
* Executable files
* PDF documents

---

## 3. Email Spoofing

Attackers may forge the sender's address to make an email appear legitimate.

Example:

```text
support@bank.com
```

may actually be sent by an attacker.

Email authentication technologies such as SPF, DKIM, and DMARC help reduce spoofing.

---

## 4. Failed Login Attempts

Mail servers generate authentication logs.

Example:

```text
User: admin

Result: Login Failed

Source IP: 203.x.x.x
```

Repeated failures may indicate password guessing or brute-force attempts.

---

## 5. Suspicious Attachments

SOC analysts examine:

* File names
* File hashes
* File types
* Antivirus detections
* Sandbox results

before concluding whether an attachment is malicious.

---

## 6. Email Logs in Splunk

Example event:

```text
src_ip=192.168.1.50

protocol=SMTP

sender=user@example.com

recipient=admin@example.com

status=Delivered
```

Analysts may investigate:

* High email volume
* Failed logins
* Suspicious senders
* Large attachments
* Unusual destinations

---

# Useful Commands

## Linux

Connect to an SMTP server:

```bash
telnet mail.example.com 25
```

Check an SMTP service:

```bash
nc mail.example.com 25
```

View open email-related ports:

```bash
ss -tuln
```

---

## Windows

Display active connections:

```cmd
netstat -ano
```

Test SMTP connectivity:

```cmd
telnet mail.example.com 25
```

---

# What I Learned

After completing this section, I learned:

* Email allows users to send and receive digital messages.
* SMTP stands for Simple Mail Transfer Protocol.
* SMTP is used to send emails.
* SMTP commonly uses ports 25, 587, and 465.
* POP3 stands for Post Office Protocol Version 3.
* POP3 downloads emails from the server.
* POP3 commonly uses ports 110 and 995.
* IMAP stands for Internet Message Access Protocol.
* IMAP synchronizes emails across multiple devices.
* IMAP commonly uses ports 143 and 993.
* SMTP sends email, while POP3 and IMAP retrieve email.
* IMAP keeps emails on the server, whereas POP3 commonly downloads them to the client.
* Modern email systems commonly use encryption to protect communication.
* Spam refers to unsolicited email.
* Phishing emails attempt to steal sensitive information.
* Email attachments can contain malicious files.
* Email spoofing attempts to impersonate trusted senders.
* SPF, DKIM, and DMARC help improve email authentication.
* SOC analysts investigate phishing, suspicious attachments, failed logins, and unusual email activity using mail server and SIEM logs.
