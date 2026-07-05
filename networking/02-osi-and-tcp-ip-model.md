# OSI and TCP/IP Models

## Table of Contents

* [Introduction](#introduction)
* [Why Do We Need Networking Models?](#why-do-we-need-networking-models)
* [OSI Model](#osi-model)
* [Layer 7 - Application Layer](#layer-7---application-layer)
* [Layer 6 - Presentation Layer](#layer-6---presentation-layer)
* [Layer 5 - Session Layer](#layer-5---session-layer)
* [Layer 4 - Transport Layer](#layer-4---transport-layer)
* [Layer 3 - Network Layer](#layer-3---network-layer)
* [Layer 2 - Data Link Layer](#layer-2---data-link-layer)
* [Layer 1 - Physical Layer](#layer-1---physical-layer)
* [Protocol Data Units](#protocol-data-units)
* [Encapsulation](#encapsulation)
* [Decapsulation](#decapsulation)
* [Complete Packet Flow Example](#complete-packet-flow-example)
* [TCP/IP Model](#tcpip-model)
* [OSI vs TCP/IP Model](#osi-vs-tcpip-model)
* [How the Models Work Together](#how-the-models-work-together)
* [Cybersecurity Relevance](#cybersecurity-relevance)
* [What I Learned](#what-i-learned)

---

# Introduction

Networking models provide a structured way to understand how devices communicate over a network.

When a computer sends data to another device, many different operations take place.

For example:

```text
Application creates data
        ↓
TCP manages communication
        ↓
IP determines destination
        ↓
Ethernet handles local delivery
        ↓
Bits are transmitted
```

Instead of studying the complete communication process as one large system, networking models divide it into multiple layers.

Each layer performs a specific task.

The two major networking models are:

* OSI Model
* TCP/IP Model

---

# Why Do We Need Networking Models?

Networking involves many different technologies, protocols, and devices.

Networking models help divide these responsibilities into layers.

This provides several advantages.

## Standardization

Different manufacturers can build networking devices and software that communicate using common standards.

For example, a laptop from one manufacturer can communicate with a router from another manufacturer.

---

## Easier Troubleshooting

Network problems can be analyzed layer by layer.

For example:

```text
No cable connection        → Layer 1 problem
MAC/VLAN issue             → Layer 2 problem
IP or routing issue        → Layer 3 problem
TCP connection issue       → Layer 4 problem
HTTP or DNS issue          → Layer 7 problem
```

This makes troubleshooting easier.

---

## Modular Design

Each layer performs a specific function.

Changes in one layer do not always require major changes in other layers.

For example, an application can use Ethernet or Wi-Fi without needing to completely change how HTTP works.

---

## Easier Learning

Networking models divide complex communication into smaller concepts.

Instead of understanding the entire network at once, each layer can be studied separately.

---

# OSI Model

OSI stands for:

```text
Open Systems Interconnection
```

The OSI model is a conceptual networking model developed to describe how network communication works.

It contains **seven layers**.

```text
+---------------------------+
| Layer 7 - Application     |
+---------------------------+
| Layer 6 - Presentation    |
+---------------------------+
| Layer 5 - Session         |
+---------------------------+
| Layer 4 - Transport       |
+---------------------------+
| Layer 3 - Network         |
+---------------------------+
| Layer 2 - Data Link       |
+---------------------------+
| Layer 1 - Physical        |
+---------------------------+
```

The layers are numbered from bottom to top.

```text
Layer 7 → Closest to applications

Layer 1 → Closest to physical network hardware
```

A common way to remember the OSI layers is:

```text
All People Seem To Need Data Processing
```

```text
Application
Presentation
Session
Transport
Network
Data Link
Physical
```

From Layer 1 to Layer 7:

```text
Please Do Not Throw Sausage Pizza Away
```

```text
Physical
Data Link
Network
Transport
Session
Presentation
Application
```

---

# Layer 7 - Application Layer

The **Application Layer** is the layer closest to the user and network applications.

It provides network services to applications.

Applications do not directly send electrical signals or Ethernet frames.

Instead, they use Application Layer protocols.

Examples include:

* Web browsers
* Email clients
* File transfer applications
* DNS clients

Common Application Layer protocols include:

| Protocol | Purpose                  |
| -------- | ------------------------ |
| HTTP     | Web communication        |
| HTTPS    | Secure web communication |
| DNS      | Domain name resolution   |
| DHCP     | Network configuration    |
| FTP      | File transfer            |
| SMTP     | Sending email            |
| POP3     | Receiving email          |
| IMAP     | Email synchronization    |
| SSH      | Secure remote access     |
| Telnet   | Remote terminal access   |

Example:

When a user enters:

```text
https://example.com
```

in a browser, the browser uses web protocols to communicate with the web server.

The Application Layer creates the data that needs to be transmitted.

Example:

```http
GET / HTTP/1.1
Host: example.com
```

### Main Responsibilities

* Providing network services to applications
* Web communication
* Email communication
* File transfer
* Name resolution
* Remote access

### PDU

```text
Data
```

---

# Layer 6 - Presentation Layer

The **Presentation Layer** is responsible for the format and representation of data.

Different systems may store or represent information differently.

The Presentation Layer helps ensure that data can be understood by the receiving system.

### Main Responsibilities

* Data formatting
* Data translation
* Encryption
* Decryption
* Compression
* Decompression
* Character encoding

Example:

```text
Application Data
      ↓
Encryption
      ↓
Encrypted Data
```

The receiving system performs:

```text
Encrypted Data
      ↓
Decryption
      ↓
Application Data
```

Examples of data formats and encodings include:

```text
JPEG
PNG
MP3
MP4
ASCII
UTF-8
```

Encryption technologies such as TLS are often conceptually associated with Presentation Layer functions in OSI explanations.

### Example

Suppose a user sends the message:

```text
Hello
```

The data may be:

```text
Encoded
Compressed
Encrypted
```

before transmission.

The receiving system reverses these operations.

### PDU

```text
Data
```

---

# Layer 5 - Session Layer

The **Session Layer** is responsible for establishing, managing, and terminating communication sessions between applications.

A session represents an active communication between two systems.

Example:

```text
Computer A <------ Session ------> Server
```

The Session Layer manages the communication session.

### Main Responsibilities

* Establishing sessions
* Maintaining sessions
* Terminating sessions
* Session synchronization
* Dialog control

Example:

A user connects to a remote system.

```text
Session Established
        ↓
Communication
        ↓
Session Maintained
        ↓
Session Terminated
```

If communication is interrupted, synchronization mechanisms can help applications manage the session state.

### PDU

```text
Data
```

---

# Layer 4 - Transport Layer

The **Transport Layer** is responsible for end-to-end communication between applications.

The two major Transport Layer protocols are:

```text
TCP
UDP
```

---

## TCP

TCP stands for:

```text
Transmission Control Protocol
```

TCP is a **connection-oriented and reliable protocol**.

Before transmitting application data, TCP can establish a connection using the **three-way handshake**.

```text
Client                         Server

SYN -------------------------->

     <--------------------- SYN-ACK

ACK -------------------------->
```

TCP provides:

* Reliable delivery
* Sequence numbers
* Acknowledgments
* Retransmission
* Flow control
* Connection management

Suppose data is divided into segments:

```text
Segment 1
Segment 2
Segment 3
```

TCP uses sequence information to help the receiver process data in the correct byte-stream order.

If required data is lost, TCP can retransmit it.

Example:

```text
Segment 1 → Received
Segment 2 → Lost
Segment 3 → Received

TCP detects missing data

Segment 2 → Retransmitted
```

---

## UDP

UDP stands for:

```text
User Datagram Protocol
```

UDP is a **connectionless protocol**.

It does not establish a connection using a three-way handshake.

UDP does not provide TCP-style guaranteed delivery, ordering, or retransmission.

Because UDP has less overhead, it is useful for applications where speed and low latency are important.

Examples include:

* DNS queries
* Voice communication
* Video streaming
* Online gaming

### Transport Layer Responsibilities

* End-to-end communication
* Segmentation
* Port numbers
* Reliability with TCP
* Flow control with TCP
* Connection management

### PDU

For TCP:

```text
Segment
```

For UDP:

```text
Datagram
```

---

# Layer 3 - Network Layer

The **Network Layer** is responsible for logical addressing and routing packets between networks.

The most important protocol at this layer is:

```text
IP - Internet Protocol
```

IP addresses identify devices logically on an IP network.

Example:

```text
192.168.1.10
```

Routers mainly operate at Layer 3.

Example:

```text
192.168.1.0/24
       |
       |
     Router
       |
       |
10.0.0.0/24
```

The router connects the two networks.

When a packet arrives, the router examines the destination IP address.

The router then checks its routing table.

Example:

```text
Destination Network      Next Hop

192.168.1.0/24           Local
10.0.0.0/24              Local
8.8.8.0/24               Router A
```

The router selects a route and forwards the packet.

### Main Responsibilities

* Logical addressing
* IP addressing
* Routing
* Packet forwarding
* Path selection

### Common Protocols

* IPv4
* IPv6
* ICMP

Routing protocols also support network path exchange and route selection, including:

* OSPF
* BGP
* RIP

### Devices

```text
Router
Layer 3 Switch
```

### PDU

```text
Packet
```

---

# Layer 2 - Data Link Layer

The **Data Link Layer** is responsible for local network communication over a specific link.

It uses **MAC addresses** for Ethernet delivery.

Example MAC address:

```text
00:1A:2B:3C:4D:5E
```

Switches mainly operate at Layer 2.

Consider:

```text
PC1 -------- Switch -------- PC2
```

PC1 wants to send data to PC2.

The Ethernet frame contains:

```text
Source MAC Address
Destination MAC Address
```

The switch examines the destination MAC address.

It checks its MAC address table.

Example:

```text
MAC Address               Port

AA:AA:AA:AA:AA:AA         Fa0/1
BB:BB:BB:BB:BB:BB         Fa0/2
```

The switch then forwards the frame toward the correct port when the destination is known.

### Main Responsibilities

* MAC addressing
* Local link delivery
* Frame creation
* Error detection
* Media access
* VLAN-based logical segmentation

### Technologies and Protocols

* Ethernet
* Wi-Fi link-layer technologies
* VLANs
* Spanning Tree Protocol

ARP is closely associated with local network communication because it resolves IPv4 addresses to MAC addresses.

### Devices

```text
Switch
Bridge
Network Interface Card
```

### PDU

```text
Frame
```

---

# Layer 1 - Physical Layer

The **Physical Layer** is responsible for transmitting raw bits through a physical or wireless communication medium.

Data is represented as:

```text
0 and 1
```

Example:

```text
1011010010101101
```

The Physical Layer defines how these bits are transmitted.

Transmission may use:

* Electrical signals
* Light signals
* Radio signals

Examples of physical media include:

```text
Ethernet copper cable
Fiber optic cable
Coaxial cable
Wireless radio
```

Physical Layer components include:

* Cables
* Connectors
* Repeaters
* Hubs
* Physical network interfaces

### Main Responsibilities

* Bit transmission
* Signal transmission
* Physical connections
* Data rates
* Cable specifications
* Connector specifications

### Devices

```text
Hub
Repeater
Cables
```

### PDU

```text
Bits
```

---

# Protocol Data Units

A **Protocol Data Unit (PDU)** is the name given to data at a specific networking layer.

| OSI Layer    | PDU                |
| ------------ | ------------------ |
| Application  | Data               |
| Presentation | Data               |
| Session      | Data               |
| Transport    | Segment / Datagram |
| Network      | Packet             |
| Data Link    | Frame              |
| Physical     | Bits               |

The data changes as it moves through the networking layers.

```text
Data
 ↓
Segment
 ↓
Packet
 ↓
Frame
 ↓
Bits
```

At the receiving system:

```text
Bits
 ↓
Frame
 ↓
Packet
 ↓
Segment
 ↓
Data
```

---

# Encapsulation

**Encapsulation** is the process of adding protocol information to data as it moves down the networking layers.

Suppose a user accesses a website.

The browser creates an HTTP request.

```http
GET / HTTP/1.1
Host: example.com
```

This is application data.

---

## Step 1 - Application Data

```text
[ HTTP Data ]
```

The application creates the data.

---

## Step 2 - Transport Layer

TCP adds a TCP header.

```text
[ TCP Header | HTTP Data ]
```

The TCP header can contain information such as:

* Source port
* Destination port
* Sequence number
* Acknowledgment number
* TCP flags

The result is a TCP segment.

---

## Step 3 - Network Layer

IP adds an IP header.

```text
[ IP Header | TCP Header | HTTP Data ]
```

The IP header contains information such as:

* Source IP address
* Destination IP address
* TTL
* Protocol information

The result is an IP packet.

---

## Step 4 - Data Link Layer

Ethernet adds an Ethernet header and trailer.

```text
[ Ethernet Header | IP Header | TCP Header | HTTP Data | Ethernet Trailer ]
```

The Ethernet header contains:

* Source MAC address
* Destination MAC address

The trailer includes information used for error detection.

The result is an Ethernet frame.

---

## Step 5 - Physical Layer

The frame is transmitted as bits.

```text
101010110101010101011101
```

These bits are transmitted using electrical, light, or radio signals.

The complete process is:

```text
HTTP Data
    ↓
TCP Header Added
    ↓
TCP Segment
    ↓
IP Header Added
    ↓
IP Packet
    ↓
Ethernet Header and Trailer Added
    ↓
Ethernet Frame
    ↓
Bits
```

This process is called **encapsulation**.

---

# Decapsulation

**Decapsulation** is the reverse of encapsulation.

It occurs on the receiving device.

The receiving device gets the bits.

```text
Bits
 ↓
Frame
 ↓
Packet
 ↓
Segment
 ↓
Application Data
```

---

## Step 1 - Physical Layer

The device receives signals and processes them as bits.

```text
101010101101
```

---

## Step 2 - Data Link Layer

The Ethernet frame is processed.

```text
[ Ethernet Header | IP Packet | Ethernet Trailer ]
```

The Data Link Layer examines information such as the destination MAC address and performs frame validation.

The Ethernet information is removed before the packet is passed upward.

---

## Step 3 - Network Layer

The IP packet is processed.

```text
[ IP Header | TCP Segment ]
```

The destination IP address and other IP header fields are examined.

The IP header is processed and the Transport Layer payload is passed upward.

---

## Step 4 - Transport Layer

TCP processes the segment.

```text
[ TCP Header | HTTP Data ]
```

TCP uses port and connection information to deliver the data to the correct application process.

---

## Step 5 - Application Layer

The application receives the data.

```text
HTTP Data
```

The web server processes the HTTP request.

This process is called **decapsulation**.

---

# Complete Packet Flow Example

Suppose a computer accesses a web server.

```text
Client IP: 192.168.1.10

Server IP: 203.0.113.10
```

The user enters:

```text
http://example.com
```

For simplicity, assume the client already knows the server IP address and an HTTP connection is being used.

---

## Application Layer

The browser creates an HTTP request.

```http
GET / HTTP/1.1
Host: example.com
```

---

## Transport Layer

TCP processes the application data.

Example ports:

```text
Source Port:      50000
Destination Port: 80
```

The source port identifies the client-side communication endpoint.

Port 80 identifies the HTTP service.

```text
[ TCP Header | HTTP Data ]
```

---

## Network Layer

IP adds logical addressing.

```text
Source IP:      192.168.1.10
Destination IP: 203.0.113.10
```

The packet becomes:

```text
[ IP Header | TCP Header | HTTP Data ]
```

---

## Data Link Layer

The client determines that the destination server is outside its local network.

Therefore, the Ethernet frame is sent toward the default gateway.

Example:

```text
Source MAC:      Client MAC
Destination MAC: Router MAC
```

The frame becomes:

```text
[ Ethernet Header | IP Header | TCP Header | HTTP Data | Ethernet Trailer ]
```

An important concept is:

```text
Destination IP  = Web Server

Destination MAC = Next local network hop
```

The destination IP normally remains the remote server's IP while the packet is routed.

The Layer 2 frame is recreated for each link along the route.

---

## Physical Layer

The frame is transmitted as bits.

```text
1011010101011010101
```

The router receives the frame.

The router removes the incoming Layer 2 framing and examines the destination IP address.

It checks its routing table and selects the next hop.

The router then creates a new Layer 2 frame for the next link.

This process continues until the packet reaches the destination network.

---

# TCP/IP Model

The **TCP/IP model** is the practical networking model used to describe communication on the Internet.

TCP/IP commonly uses four layers.

```text
+---------------------------+
| Application Layer         |
+---------------------------+
| Transport Layer           |
+---------------------------+
| Internet Layer            |
+---------------------------+
| Network Access Layer      |
+---------------------------+
```

---

# TCP/IP Application Layer

The TCP/IP Application Layer combines the functions associated with the OSI:

```text
Application Layer
Presentation Layer
Session Layer
```

Common protocols include:

* HTTP
* HTTPS
* DNS
* DHCP
* FTP
* SMTP
* POP3
* IMAP
* SSH

### Responsibilities

* Application network services
* Data representation
* Application communication
* Session-related functionality

---

# TCP/IP Transport Layer

The Transport Layer provides communication between application processes.

Protocols include:

```text
TCP
UDP
```

Responsibilities include:

* Port numbers
* End-to-end communication
* Segmentation
* Reliability with TCP
* Connection management with TCP

---

# TCP/IP Internet Layer

The Internet Layer handles logical addressing and packet routing.

Protocols include:

```text
IPv4
IPv6
ICMP
```

Routers operate primarily at this layer.

Responsibilities include:

* IP addressing
* Routing
* Packet forwarding

---

# TCP/IP Network Access Layer

The Network Access Layer handles local network communication and physical transmission.

It combines OSI Layer 1 and Layer 2 functions.

Technologies include:

* Ethernet
* Wi-Fi

Responsibilities include:

* Framing
* MAC addressing
* Media access
* Physical transmission

---

# OSI vs TCP/IP Model

| OSI Model    | TCP/IP Model   |
| ------------ | -------------- |
| Application  | Application    |
| Presentation | Application    |
| Session      | Application    |
| Transport    | Transport      |
| Network      | Internet       |
| Data Link    | Network Access |
| Physical     | Network Access |

Diagram:

```text
OSI Model                  TCP/IP Model

Application  ┐
Presentation ├───────────> Application
Session      ┘

Transport    ────────────> Transport

Network      ────────────> Internet

Data Link    ┐
Physical     ┴───────────> Network Access
```

---

## Major Differences

| OSI                                 | TCP/IP                                  |
| ----------------------------------- | --------------------------------------- |
| 7 layers                            | Commonly represented with 4 layers      |
| Conceptual reference model          | Practical Internet protocol model       |
| Developed by ISO                    | Based on the Internet protocol suite    |
| Separates Session and Presentation  | Includes these functions in Application |
| Physical and Data Link are separate | Combined as Network Access              |

The OSI model is commonly used for:

```text
Learning
Troubleshooting
Network discussion
Security analysis
```

The TCP/IP model is used to describe the protocols and architecture that power modern IP networks and the Internet.

---

# How the Models Work Together

The OSI and TCP/IP models describe similar networking concepts using different layer structures.

Consider accessing a website.

```text
Browser
   ↓
HTTP
   ↓
TCP
   ↓
IP
   ↓
Ethernet
   ↓
Physical Network
```

Using the OSI model:

```text
HTTP       → Application Layer

TCP        → Transport Layer

IP         → Network Layer

Ethernet   → Data Link Layer

Signals    → Physical Layer
```

Using the TCP/IP model:

```text
HTTP       → Application Layer

TCP        → Transport Layer

IP         → Internet Layer

Ethernet   → Network Access Layer
```

Both models help explain the same communication process.

---

# Cybersecurity Relevance

Understanding networking models is extremely important in cybersecurity.

Security analysts often investigate network activity layer by layer.

For example:

```text
Layer 7 → Suspicious HTTP request

Layer 4 → Unusual TCP connection

Layer 3 → Malicious destination IP

Layer 2 → ARP spoofing

Layer 1 → Physical network issue
```

---

## Layer 7 Analysis

At the Application Layer, analysts may investigate:

* HTTP requests
* DNS queries
* Email traffic
* File transfers
* Authentication activity

Example:

```http
POST /upload HTTP/1.1
```

An unusual POST request containing a large amount of data may require investigation.

---

## Layer 4 Analysis

At the Transport Layer, analysts analyze:

* Source ports
* Destination ports
* TCP flags
* Connection attempts
* Connection frequency

Example:

```text
Source:      192.168.1.20:55000
Destination: 10.0.0.5:22
Protocol:    TCP
```

Repeated connection attempts may indicate scanning or brute-force activity.

---

## Layer 3 Analysis

At the Network Layer, analysts examine:

* Source IP addresses
* Destination IP addresses
* ICMP activity
* Routing behavior

Example:

```text
192.168.1.15 ---> Unknown External IP
```

Repeated communication with a suspicious external IP address may indicate malware or command-and-control activity.

---

## Layer 2 Analysis

At the Data Link Layer, analysts may investigate:

* MAC addresses
* ARP traffic
* VLAN activity
* Unusual MAC address changes

Example:

```text
192.168.1.1 → AA:AA:AA:AA:AA:AA

Later

192.168.1.1 → BB:BB:BB:BB:BB:BB
```

An unexpected MAC address change for the gateway could indicate ARP spoofing or a network configuration change.

---

## Packet Analysis

Tools such as Wireshark and tcpdump allow security analysts to inspect network traffic.

A packet may contain multiple protocol layers.

Example:

```text
Ethernet
   ↓
IPv4
   ↓
TCP
   ↓
HTTP
```

In Wireshark, these protocols can be examined separately.

Example display filters include:

```text
ip
```

```text
tcp
```

```text
udp
```

```text
dns
```

```text
http
```

Understanding the OSI and TCP/IP models helps analysts understand where each protocol operates and what information should be investigated.

---

# What I Learned

From studying the OSI and TCP/IP models, I learned that network communication is divided into layers.

The OSI model contains seven layers:

```text
Application
Presentation
Session
Transport
Network
Data Link
Physical
```

Each layer performs a specific function.

The Transport Layer manages communication between applications using protocols such as TCP and UDP.

The Network Layer uses IP addresses and routing to deliver packets between networks.

The Data Link Layer handles local network communication using frames and MAC addresses.

The Physical Layer transmits bits through wired or wireless communication media.

I also learned about encapsulation, where protocol headers are added as data moves down the networking layers.

```text
Data
↓
Segment
↓
Packet
↓
Frame
↓
Bits
```

The receiving device performs the reverse process, called decapsulation.

The TCP/IP model provides a practical view of Internet communication using the Application, Transport, Internet, and Network Access layers.

From a cybersecurity perspective, networking models help security analysts investigate network activity at different layers and understand how protocols are encapsulated inside packets.
