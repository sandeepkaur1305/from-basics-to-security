# 06 - TCP, UDP, Ports and Sockets

## Table of Contents

* [Transport Layer](#transport-layer)
* [What is TCP?](#what-is-tcp)
* [TCP Three-Way Handshake](#tcp-three-way-handshake)
* [TCP Flags](#tcp-flags)
* [TCP Connection Termination](#tcp-connection-termination)
* [TCP Reliability](#tcp-reliability)
* [Sequence and Acknowledgment Numbers](#sequence-and-acknowledgment-numbers)
* [What is UDP?](#what-is-udp)
* [TCP vs UDP](#tcp-vs-udp)
* [What is a Port?](#what-is-a-port)
* [Port Number Ranges](#port-number-ranges)
* [Common Port Numbers](#common-port-numbers)
* [Source Port and Destination Port](#source-port-and-destination-port)
* [What is a Socket?](#what-is-a-socket)
* [Socket Pair](#socket-pair)
* [Listening Ports](#listening-ports)
* [Open, Closed and Filtered Ports](#open-closed-and-filtered-ports)
* [TCP Connection Example](#tcp-connection-example)
* [UDP Communication Example](#udp-communication-example)
* [Cybersecurity and SOC Relevance](#cybersecurity-and-soc-relevance)
* [Useful Commands](#useful-commands)
* [Practical Lab](#practical-lab)
* [What I Learned](#what-i-learned)

---

# Transport Layer

The **Transport Layer is Layer 4 of the OSI model**.

Its main purpose is to provide communication between applications running on different devices.

Example:

```text
Laptop
Web Browser
    |
    |
Network
    |
    |
Web Server
Apache / Nginx
```

The Transport Layer helps the browser communicate with the correct application on the server.

The two main Transport Layer protocols are:

```text
TCP
UDP
```

The Transport Layer uses **port numbers**.

Example:

```text
IP Address → Identifies the Device

Port Number → Identifies the Application
```

Simple example:

```text
192.168.1.10:80
```

Here:

```text
192.168.1.10 = IP Address

80 = Port Number
```

Think of it like:

```text
IP Address = House Address

Port Number = Room Number
```

The IP address finds the device.

The port number finds the application running on that device.

---

# What is TCP?

**TCP stands for Transmission Control Protocol.**

TCP is a:

```text
Connection-Oriented Protocol
```

Before sending application data, TCP creates a connection between two devices.

TCP provides:

```text
Reliable Delivery
Ordered Delivery
Error Detection
Flow Control
Connection Management
```

Example protocols that commonly use TCP:

```text
HTTP
HTTPS
SSH
FTP
SMTP
```

Example:

```text
Browser
   |
   | TCP Connection
   ↓
Web Server
```

TCP first creates a connection.

Then data is transferred.

---

## Connection-Oriented Protocol

A connection-oriented protocol creates a connection before sending data.

Simple analogy:

```text
Phone Call
```

Before talking:

```text
Person A calls Person B

Person B answers

Conversation starts
```

TCP works in a similar way.

```text
Create Connection
       ↓
Transfer Data
       ↓
Close Connection
```

TCP uses the **Three-Way Handshake** to create a connection.

---

# TCP Three-Way Handshake

The TCP Three-Way Handshake creates a connection between a client and a server.

The three steps are:

```text
SYN
SYN-ACK
ACK
```

Diagram:

```text
Client                         Server

   | -------- SYN -----------> |
   |                           |
   | <------ SYN-ACK --------- |
   |                           |
   | -------- ACK -----------> |
   |                           |

        Connection Established
```

---

## Step 1 - SYN

The client sends a TCP packet with the:

```text
SYN Flag
```

Example:

```text
Client → Server

SYN
```

Simple meaning:

> I want to create a TCP connection.

The client also sends an initial sequence number.

Example:

```text
Sequence Number = 1000
```

Packet:

```text
Source IP: 192.168.1.10
Destination IP: 142.x.x.x

Source Port: 50000
Destination Port: 443

Flag: SYN

Sequence Number: 1000
```

---

## Step 2 - SYN-ACK

The server receives the SYN packet.

If the server accepts the connection, it sends:

```text
SYN-ACK
```

Simple meaning:

> I received your request and I also want to establish the connection.

Example:

```text
Server → Client

SYN-ACK
```

The server may send:

```text
Sequence Number = 5000

Acknowledgment Number = 1001
```

Why `1001`?

The client sent:

```text
Sequence Number = 1000
```

The SYN consumes one sequence number.

Therefore:

```text
1000 + 1 = 1001
```

The server expects the next sequence number to be `1001`.

---

## Step 3 - ACK

The client sends:

```text
ACK
```

Simple meaning:

> I received your SYN-ACK.

Example:

```text
Client → Server

ACK
```

The connection is now established.

```text
TCP Connection Established
```

Complete process:

```text
Client → SYN → Server

Client ← SYN-ACK ← Server

Client → ACK → Server
```

Remember:

```text
SYN
SYN-ACK
ACK
```

---

# TCP Flags

TCP flags are used to control TCP connections.

Common TCP flags include:

| Flag | Meaning                      |
| ---- | ---------------------------- |
| SYN  | Start a connection           |
| ACK  | Acknowledge received data    |
| FIN  | Finish a connection          |
| RST  | Reset a connection           |
| PSH  | Push data to the application |
| URG  | Urgent data                  |

---

## SYN Flag

SYN means:

```text
Synchronize
```

It is used to start a TCP connection.

Example:

```text
Client → SYN → Server
```

---

## ACK Flag

ACK means:

```text
Acknowledgment
```

It confirms that data or a TCP packet was received.

Example:

```text
Server → Data → Client

Server ← ACK ← Client
```

Simple meaning:

> I received the data.

---

## FIN Flag

FIN means:

```text
Finish
```

It is used to close a TCP connection normally.

Example:

```text
Client → FIN → Server
```

Simple meaning:

> I have finished sending data.

---

## RST Flag

RST means:

```text
Reset
```

It immediately resets or rejects a TCP connection.

Example:

```text
Client → SYN → Server

Client ← RST ← Server
```

This may happen when a port is closed.

Simple meaning:

> This connection is not accepted.

---

## PSH Flag

PSH means:

```text
Push
```

It tells TCP to quickly pass received data to the application.

Example:

```text
Network Data
     ↓
TCP
     ↓
Application
```

The PSH flag requests that the data be delivered to the application without unnecessary waiting.

---

# TCP Connection Termination

TCP connections must also be closed.

TCP commonly uses a **Four-Way Termination Process**.

Example:

```text
Client                         Server

   | -------- FIN -----------> |
   |                           |
   | <------- ACK ------------ |
   |                           |
   | <------- FIN ------------ |
   |                           |
   | -------- ACK -----------> |
```

The steps are:

```text
FIN
ACK
FIN
ACK
```

---

## Step 1

The client sends:

```text
FIN
```

Meaning:

> I have finished sending data.

---

## Step 2

The server sends:

```text
ACK
```

Meaning:

> I received your FIN.

---

## Step 3

The server sends:

```text
FIN
```

Meaning:

> I have also finished sending data.

---

## Step 4

The client sends:

```text
ACK
```

Meaning:

> I received your FIN.

The TCP connection is closed.

---

# TCP Reliability

TCP provides reliable communication.

Suppose a sender sends:

```text
Packet 1
Packet 2
Packet 3
```

The receiver receives:

```text
Packet 1
Packet 3
```

Packet 2 is missing.

TCP can detect this problem.

The missing data can be retransmitted.

```text
Sender → Packet 2 → Receiver
```

TCP reliability uses concepts such as:

```text
Sequence Numbers
Acknowledgment Numbers
Retransmission
Checksums
Flow Control
```

---

## Retransmission

Suppose a client sends data.

```text
Client → Data → Server
```

The client waits for an acknowledgment.

If the ACK does not arrive:

```text
No ACK
```

TCP may send the data again.

```text
Client → Data Again → Server
```

This is called:

```text
TCP Retransmission
```

Network analysts may see retransmissions in packet captures.

A large number of retransmissions may indicate:

```text
Packet Loss
Network Congestion
Poor Network Connection
Server Problems
```

---

# Sequence and Acknowledgment Numbers

TCP uses sequence numbers to track data.

Suppose a sender sends:

```text
100 bytes
```

Starting sequence number:

```text
Sequence Number = 1000
```

The data covers:

```text
1000 - 1099
```

The receiver may respond:

```text
Acknowledgment Number = 1100
```

Simple meaning:

> I received everything before 1100. Send me data starting from 1100.

Example:

```text
Sender

SEQ = 1000
Data = 100 bytes

        ↓

Receiver

ACK = 1100
```

The acknowledgment number normally represents:

```text
Next Expected Sequence Number
```

---

## Simple TCP Data Example

Client sends:

```text
SEQ = 100
Data = 50 bytes
```

The server receives bytes:

```text
100 - 149
```

The next expected byte is:

```text
150
```

The server sends:

```text
ACK = 150
```

Simple formula:

```text
ACK = SEQ + Data Length
```

For this example:

```text
100 + 50 = 150
```

---

# What is UDP?

**UDP stands for User Datagram Protocol.**

UDP is a:

```text
Connectionless Protocol
```

UDP does not create a connection before sending data.

There is no TCP Three-Way Handshake.

Example:

```text
Sender → Data → Receiver
```

The sender simply sends the data.

UDP does not guarantee:

```text
Delivery
Order
Retransmission
```

However, UDP is faster and has less overhead than TCP.

Common protocols that use UDP include:

```text
DNS
DHCP
VoIP
Online Gaming
Video Streaming
```

---

## Connectionless Protocol

UDP does not establish a connection.

Simple analogy:

```text
Sending a Letter
```

You send the letter.

You do not know if:

```text
The letter arrived
The letter was lost
The letter arrived in order
```

UDP works similarly.

```text
Send Data
   ↓
Continue
```

There is no built-in acknowledgment.

---

## UDP Example

Suppose a device sends:

```text
Packet 1
Packet 2
Packet 3
```

The receiver may receive:

```text
Packet 1
Packet 3
```

Packet 2 is lost.

UDP does not automatically retransmit Packet 2.

```text
Packet 2 Lost
      ↓
No Automatic Retransmission
```

The application must handle the problem if required.

---

# TCP vs UDP

| TCP                   | UDP                             |
| --------------------- | ------------------------------- |
| Connection-oriented   | Connectionless                  |
| Reliable              | No delivery guarantee           |
| Ordered delivery      | Packets may arrive out of order |
| Uses acknowledgments  | No built-in acknowledgments     |
| Retransmits lost data | No automatic retransmission     |
| More overhead         | Less overhead                   |
| Generally slower      | Generally faster                |
| Three-way handshake   | No handshake                    |

Simple explanation:

```text
TCP
Reliable but more overhead
```

```text
UDP
Fast but less reliable
```

---

## TCP Example

Downloading a file.

Suppose the file contains:

```text
A B C D E
```

Receiving:

```text
A B D E
```

is not acceptable.

The missing data must be retransmitted.

Therefore, TCP is useful.

---

## UDP Example

Live video call.

Suppose one small packet is lost.

```text
Frame 1
Frame 2
Frame 4
```

Waiting to retransmit Frame 3 may create delay.

For real-time communication:

```text
Low Delay
```

may be more important than perfect delivery.

Therefore, UDP may be used.

---

# What is a Port?

A **port is a logical number used to identify an application or network service on a device**.

Port numbers work at the:

```text
Transport Layer
```

TCP and UDP use port numbers.

Port numbers range from:

```text
0 - 65535
```

Example:

```text
192.168.1.10:22
```

Here:

```text
192.168.1.10 = Device

22 = SSH Service
```

Another example:

```text
192.168.1.10:80
```

Here:

```text
80 = HTTP Service
```

A single device can run multiple services.

Example:

```text
192.168.1.10:22   → SSH

192.168.1.10:80   → HTTP

192.168.1.10:443  → HTTPS
```

The IP address is the same.

The port numbers are different.

---

# Port Number Ranges

Port numbers are divided into three major ranges.

| Range         | Name                       |
| ------------- | -------------------------- |
| 0 - 1023      | Well-Known Ports           |
| 1024 - 49151  | Registered Ports           |
| 49152 - 65535 | Dynamic or Ephemeral Ports |

---

## Well-Known Ports

Range:

```text
0 - 1023
```

These ports are commonly used by standard network services.

Examples:

```text
22  → SSH
53  → DNS
80  → HTTP
443 → HTTPS
```

---

## Registered Ports

Range:

```text
1024 - 49151
```

These ports may be used by specific applications and services.

Example:

```text
3306 → MySQL
3389 → RDP
8080 → Common alternative HTTP port
```

---

## Dynamic or Ephemeral Ports

Range:

```text
49152 - 65535
```

These ports are commonly used temporarily by client applications.

Example:

```text
Browser Source Port:
52000
```

The browser connects to:

```text
Web Server Destination Port:
443
```

Connection:

```text
192.168.1.10:52000
        ↓
142.x.x.x:443
```

The source port is temporary.

---

# Common Port Numbers

| Port | Protocol or Service  |
| ---- | -------------------- |
| 20   | FTP Data             |
| 21   | FTP Control          |
| 22   | SSH                  |
| 23   | Telnet               |
| 25   | SMTP                 |
| 53   | DNS                  |
| 67   | DHCP Server          |
| 68   | DHCP Client          |
| 80   | HTTP                 |
| 110  | POP3                 |
| 123  | NTP                  |
| 143  | IMAP                 |
| 161  | SNMP                 |
| 389  | LDAP                 |
| 443  | HTTPS                |
| 445  | SMB                  |
| 993  | IMAPS                |
| 995  | POP3S                |
| 1433 | Microsoft SQL Server |
| 3306 | MySQL                |
| 3389 | RDP                  |
| 5432 | PostgreSQL           |
| 8080 | Alternative HTTP     |

These ports are very important for:

```text
Networking
Nmap
Firewalls
SOC Analysis
SIEM Logs
Packet Analysis
```

---

# Source Port and Destination Port

Every TCP or UDP communication normally contains:

```text
Source Port

Destination Port
```

Example:

```text
Client
192.168.1.10:52000

Server
142.x.x.x:443
```

The client sends:

```text
Source Port:
52000

Destination Port:
443
```

Why?

The destination port tells the server:

> Send this data to the HTTPS service.

The source port tells the client operating system:

> Return the response to this application connection.

---

## Response Packet

The server sends a response.

Original packet:

```text
Source Port: 52000
Destination Port: 443
```

Response:

```text
Source Port: 443
Destination Port: 52000
```

Notice:

```text
Source and Destination Ports are Reversed
```

Example:

```text
Client → Server

52000 → 443
```

Response:

```text
Server → Client

443 → 52000
```

---

# What is a Socket?

A **socket is a communication endpoint used by an application to send or receive network data**.

A socket commonly contains:

```text
IP Address
Port Number
Protocol
```

Example:

```text
192.168.1.10:5000 TCP
```

This represents a TCP socket.

Another example:

```text
192.168.1.10:53 UDP
```

This represents a UDP socket.

Simple explanation:

```text
Socket = IP + Port + Protocol
```

A socket allows applications to communicate through the network.

---

## Socket Example

Suppose a Python server runs on:

```text
IP:
192.168.1.10

Port:
5000
```

The server socket is:

```text
192.168.1.10:5000
```

A client may connect from:

```text
192.168.1.20:52000
```

Connection:

```text
192.168.1.20:52000
        ↓
192.168.1.10:5000
```

The Python `socket` module can be used to create this communication.

---

# Socket Pair

A TCP connection can be identified using:

```text
Source IP
Source Port
Destination IP
Destination Port
```

Example:

```text
192.168.1.10:52000
        ↓
142.x.x.x:443
```

The connection information is:

```text
Source IP:
192.168.1.10

Source Port:
52000

Destination IP:
142.x.x.x

Destination Port:
443
```

This is commonly called a:

```text
4-Tuple
```

The four values are:

```text
Source IP
Source Port
Destination IP
Destination Port
```

If the protocol is also included:

```text
TCP
```

the information may be considered a:

```text
5-Tuple
```

The five values are:

```text
Source IP
Source Port
Destination IP
Destination Port
Protocol
```

The **5-tuple is extremely important in cybersecurity and network analysis**.

---

# Listening Ports

A server application can listen on a port.

Example:

```text
SSH Server
Listening on Port 22
```

This means the server is waiting for incoming connections.

Example:

```text
Client → Port 22 → SSH Server
```

A Python server may listen on:

```text
Port 5000
```

Example:

```python
server.listen()
```

The server waits for clients.

Simple meaning:

```text
Listening Port
      ↓
Waiting for Incoming Connections
```

---

# Open, Closed and Filtered Ports

During port scanning, ports may appear as:

```text
Open
Closed
Filtered
```

---

## Open Port

An open port means an application is listening on the port.

Example:

```text
Port 22 Open
```

Possible service:

```text
SSH
```

Communication:

```text
Scanner → SYN → Server

Scanner ← SYN-ACK ← Server
```

The SYN-ACK indicates that the TCP port is accepting connections.

---

## Closed Port

A closed port means the device is reachable, but no application is listening on the port.

Example:

```text
Port 9999 Closed
```

Communication may look like:

```text
Scanner → SYN → Server

Scanner ← RST ← Server
```

The server resets the connection.

---

## Filtered Port

A filtered port means a firewall or security device may be blocking the traffic.

Example:

```text
Scanner → SYN → Firewall

No Response
```

The scanner may not know whether the port is open or closed.

Therefore, the port may be shown as:

```text
Filtered
```

---

## Open vs Closed vs Filtered

| State    | Meaning                          |
| -------- | -------------------------------- |
| Open     | Application is listening         |
| Closed   | No application is listening      |
| Filtered | Firewall may be blocking traffic |

Simple TCP example:

```text
SYN → SYN-ACK
Open
```

```text
SYN → RST
Closed
```

```text
SYN → No Response
Possibly Filtered
```

---

# TCP Connection Example

Suppose a browser connects to a web server.

Client:

```text
192.168.1.10:52000
```

Server:

```text
142.x.x.x:443
```

## Step 1 - TCP Handshake

```text
Client → SYN → Server

Client ← SYN-ACK ← Server

Client → ACK → Server
```

Connection established.

---

## Step 2 - HTTPS Data Transfer

The browser sends application data.

```text
Client → HTTPS Data → Server
```

The server responds.

```text
Client ← HTTPS Data ← Server
```

TCP uses sequence and acknowledgment numbers.

---

## Step 3 - Connection Termination

```text
FIN
ACK
FIN
ACK
```

The TCP connection is closed.

Complete process:

```text
Three-Way Handshake
        ↓
Data Transfer
        ↓
Four-Way Termination
```

---

# UDP Communication Example

Suppose a client performs a DNS query.

Client:

```text
192.168.1.10:53000
```

DNS Server:

```text
8.8.8.8:53
```

The client sends:

```text
DNS Query
```

Example:

```text
What is the IP address of example.com?
```

Packet:

```text
Source Port:
53000

Destination Port:
53

Protocol:
UDP
```

The DNS server sends a response.

```text
Source Port:
53

Destination Port:
53000
```

There is no TCP three-way handshake.

Communication:

```text
Client → DNS Query → Server

Client ← DNS Response ← Server
```

---

# Cybersecurity and SOC Relevance

TCP, UDP, ports, and sockets are extremely important for cybersecurity.

---

## 1. Port Scanning

Attackers and security professionals may scan ports to discover services.

Example:

```text
22 Open
80 Open
443 Open
3306 Open
```

This may indicate:

```text
22   → SSH
80   → Web Server
443  → HTTPS
3306 → MySQL
```

A security analyst should understand what services may be exposed.

---

## 2. Suspicious Open Ports

Suppose a workstation normally uses:

```text
80
443
53
```

Suddenly a security tool detects:

```text
Port 4444 Listening
```

This may require investigation.

Port `4444` is sometimes associated with security testing tools or reverse shell activity.

Important:

> A port number alone does not prove malicious activity.

The analyst should investigate:

```text
Process Name
Executable Path
User
Network Connection
Destination IP
Logs
```

---

## 3. TCP SYN Scan

A scanner may send SYN packets to multiple ports.

Example:

```text
SYN → Port 20
SYN → Port 21
SYN → Port 22
SYN → Port 23
SYN → Port 24
```

A large number of SYN packets to many ports may indicate:

```text
Port Scanning
```

SOC analysts may detect:

```text
One Source IP
      ↓
Many Destination Ports
```

Example:

```text
192.168.1.50 → Port 20
192.168.1.50 → Port 21
192.168.1.50 → Port 22
192.168.1.50 → Port 23
```

This pattern may be suspicious.

---

## 4. SYN Flood

A SYN flood is a type of Denial of Service activity.

The attacker sends many SYN packets.

```text
Attacker → SYN
Attacker → SYN
Attacker → SYN
Attacker → SYN
Attacker → SYN
```

The server sends SYN-ACK packets.

However, the final ACK may never arrive.

```text
SYN
 ↓
SYN-ACK
 ↓
No ACK
```

The server may keep many half-open connections.

This can consume resources.

SOC analysts may investigate:

```text
Large Number of SYN Packets
Few ACK Packets
Many Half-Open Connections
Traffic Spikes
```

---

## 5. Unusual Destination Ports

Suppose SIEM logs show:

```text
Internal PC
     ↓
External IP:4444
```

The analyst should investigate the connection.

Important questions include:

```text
Which process created the connection?

Which user was logged in?

Is the destination IP malicious?

How long did the connection last?

How much data was transferred?
```

---

## 6. 5-Tuple Analysis

SOC tools frequently use the 5-tuple.

Example:

```text
Source IP:
192.168.1.10

Source Port:
52000

Destination IP:
45.x.x.x

Destination Port:
443

Protocol:
TCP
```

This information helps identify a network connection.

The 5-tuple is:

```text
Source IP
Source Port
Destination IP
Destination Port
Protocol
```

You will see these fields in:

```text
Firewall Logs
NetFlow
Packet Captures
SIEM
IDS/IPS Alerts
```

---

## 7. TCP Flags in Packet Analysis

TCP flags can provide useful information.

Example:

```text
SYN
```

May indicate a connection attempt.

```text
SYN-ACK
```

May indicate an accepting TCP service.

```text
RST
```

May indicate a rejected or closed connection.

```text
FIN
```

May indicate normal connection termination.

A large number of unusual TCP flags may require investigation.

---

## 8. UDP Traffic Analysis

UDP traffic should also be monitored.

Common UDP traffic includes:

```text
DNS
DHCP
NTP
```

Suppose a device sends thousands of UDP packets to an unknown external IP.

This may require investigation.

Possible questions:

```text
Which application generated the traffic?

What destination port is used?

How much data is transferred?

Is the destination trusted?

Is the traffic normal for the device?
```

---

# Useful Commands

## Linux

Show listening ports:

```bash
ss -tuln
```

Explanation:

```text
-t → TCP

-u → UDP

-l → Listening

-n → Show numerical ports
```

Show TCP connections:

```bash
ss -tn
```

Show UDP sockets:

```bash
ss -un
```

Show listening TCP ports:

```bash
ss -ltn
```

Show listening UDP ports:

```bash
ss -lun
```

Show processes using sockets:

```bash
ss -tulpn
```

Another command:

```bash
netstat -tuln
```

`netstat` may need to be installed on some Linux systems.

---

## Windows

Show network connections:

```cmd
netstat -ano
```

Show listening ports:

```cmd
netstat -an
```

Show executable information:

```cmd
netstat -ab
```

Administrator permissions may be required.

Find a specific port:

```cmd
netstat -ano | findstr :443
```

Example:

```text
TCP
192.168.1.10:52000
142.x.x.x:443
ESTABLISHED
```

---

## Nmap

Scan common ports:

```bash
nmap 192.168.1.10
```

Scan a specific port:

```bash
nmap -p 22 192.168.1.10
```

Scan multiple ports:

```bash
nmap -p 22,80,443 192.168.1.10
```

Scan a port range:

```bash
nmap -p 1-1000 192.168.1.10
```

TCP SYN scan:

```bash
sudo nmap -sS 192.168.1.10
```

UDP scan:

```bash
sudo nmap -sU 192.168.1.10
```

Only scan systems that you own or have permission to test.

---

# Practical Lab

## Lab 1 - View Listening Ports

Run:

```bash
ss -tuln
```

Observe:

```text
TCP Ports
UDP Ports
Listening Addresses
```

Example:

```text
0.0.0.0:22
```

This may mean a service is listening on port `22` on all IPv4 interfaces.

Example:

```text
127.0.0.1:5000
```

This means the service is listening only on the local machine.

---

## Lab 2 - Start a Netcat Listener

On one VM:

```bash
nc -lvnp 1234
```

This creates a listener on:

```text
Port 1234
```

On another VM:

```bash
nc SERVER_IP 1234
```

Example:

```bash
nc 192.168.1.10 1234
```

Type a message.

```text
Hello
```

The message should appear on the listening machine.

Observe the connection using:

```bash
ss -tn
```

Try to identify:

```text
Source IP
Source Port
Destination IP
Destination Port
```

---

## Lab 3 - Capture TCP Handshake

Start Wireshark.

Apply the filter:

```text
tcp
```

Open a website.

Look for:

```text
SYN
SYN-ACK
ACK
```

Try the Wireshark filter:

```text
tcp.flags.syn == 1
```

Observe the TCP handshake.

---

## Lab 4 - Observe DNS UDP Traffic

In Wireshark, use:

```text
dns
```

Run:

```bash
nslookup example.com
```

Observe:

```text
DNS Query
DNS Response
```

Check:

```text
Source Port
Destination Port
Protocol
```

The destination port will commonly be:

```text
53
```

---

## Lab 5 - Scan Your Own VM

Find the VM IP:

```bash
ip addr
```

From another VM, run:

```bash
nmap VM_IP
```

Example:

```bash
nmap 192.168.1.10
```

Observe:

```text
Open Ports
Closed Ports
Detected Services
```

Now start a listener:

```bash
nc -lvnp 1234
```

Scan again:

```bash
nmap -p 1234 192.168.1.10
```

The port may now appear as:

```text
open
```

Stop Netcat.

Scan again.

The port may appear as:

```text
closed
```

This lab helps understand:

```text
Listening Port
Open Port
Closed Port
TCP Connection
Socket
```

---

# What I Learned

After completing this section, I learned:

* The Transport Layer is Layer 4 of the OSI model.
* TCP and UDP are major Transport Layer protocols.
* TCP stands for Transmission Control Protocol.
* TCP is connection-oriented.
* TCP provides reliable and ordered data delivery.
* TCP uses a three-way handshake to establish a connection.
* The TCP three-way handshake uses SYN, SYN-ACK, and ACK.
* TCP flags control TCP connections.
* SYN starts a TCP connection.
* ACK acknowledges received data.
* FIN is used for normal connection termination.
* RST immediately resets a connection.
* TCP commonly uses FIN, ACK, FIN, and ACK to terminate a connection.
* TCP uses sequence numbers to track data.
* TCP uses acknowledgment numbers to identify the next expected data.
* TCP can retransmit lost data.
* UDP stands for User Datagram Protocol.
* UDP is connectionless.
* UDP does not use a three-way handshake.
* UDP does not guarantee delivery or packet order.
* UDP has less overhead than TCP.
* TCP is useful when reliable delivery is important.
* UDP is useful when low delay is important.
* Port numbers identify applications and network services.
* Port numbers range from 0 to 65535.
* Ports 0 to 1023 are well-known ports.
* Ports 1024 to 49151 are registered ports.
* Ports 49152 to 65535 are dynamic or ephemeral ports.
* Source ports commonly identify client connections.
* Destination ports commonly identify server services.
* A socket is a network communication endpoint.
* A socket commonly contains an IP address, port number, and protocol.
* A 4-tuple contains source IP, source port, destination IP, and destination port.
* A 5-tuple also includes the protocol.
* A listening port means an application is waiting for incoming communication.
* An open TCP port commonly responds with SYN-ACK.
* A closed TCP port may respond with RST.
* A filtered port may be blocked by a firewall.
* Port scanning can be identified by one source contacting many destination ports.
* SYN floods may create many half-open TCP connections.
* TCP flags are useful during packet and SOC analysis.
* The 5-tuple is commonly seen in firewall, NetFlow, IDS/IPS, and SIEM logs.
* Commands such as `ss`, `netstat`, `nmap`, and Wireshark help analyze Transport Layer communication.
