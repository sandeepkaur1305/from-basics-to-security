# Ethernet, MAC Addresses, and ARP

## Table of Contents

- [Ethernet, MAC Addresses, and ARP](#ethernet-mac-addresses-and-arp)
  - [Table of Contents](#table-of-contents)
- [Introduction](#introduction)
- [What is Ethernet?](#what-is-ethernet)
- [Ethernet Communication](#ethernet-communication)
- [Ethernet Frames](#ethernet-frames)
- [Ethernet Frame Structure](#ethernet-frame-structure)
  - [Destination MAC Address](#destination-mac-address)
  - [Source MAC Address](#source-mac-address)
  - [EtherType](#ethertype)
  - [Payload](#payload)
  - [Frame Check Sequence](#frame-check-sequence)
- [What is a MAC Address?](#what-is-a-mac-address)
- [MAC Address Structure](#mac-address-structure)
- [Types of MAC Addresses](#types-of-mac-addresses)
  - [Unicast MAC Address](#unicast-mac-address)
  - [Broadcast MAC Address](#broadcast-mac-address)
  - [Multicast MAC Address](#multicast-mac-address)
- [MAC Address vs IP Address](#mac-address-vs-ip-address)
- [How a Switch Works](#how-a-switch-works)
- [MAC Address Table](#mac-address-table)
- [MAC Address Learning](#mac-address-learning)
- [Frame Forwarding](#frame-forwarding)
- [Unknown Unicast Flooding](#unknown-unicast-flooding)
- [What is ARP?](#what-is-arp)
- [Why is ARP Required?](#why-is-arp-required)
- [How ARP Works](#how-arp-works)
  - [Step 1 - Check ARP Cache](#step-1---check-arp-cache)
  - [Step 2 - Send ARP Request](#step-2---send-arp-request)
  - [Step 3 - Target Identifies Its IP](#step-3---target-identifies-its-ip)
  - [Step 4 - ARP Reply](#step-4---arp-reply)
  - [Step 5 - Store the Mapping](#step-5---store-the-mapping)
- [ARP Request](#arp-request)
- [ARP Reply](#arp-reply)
- [ARP Cache](#arp-cache)
- [ARP and the Default Gateway](#arp-and-the-default-gateway)
- [ARP Packet Encapsulation](#arp-packet-encapsulation)
- [Gratuitous ARP](#gratuitous-arp)
- [ARP Spoofing and ARP Poisoning](#arp-spoofing-and-arp-poisoning)
- [Man-in-the-Middle Attack](#man-in-the-middle-attack)
- [Detecting Suspicious ARP Activity](#detecting-suspicious-arp-activity)
  - [Multiple IP Addresses Associated with One MAC](#multiple-ip-addresses-associated-with-one-mac)
  - [High Volume of ARP Replies](#high-volume-of-arp-replies)
- [Wireshark ARP Analysis](#wireshark-arp-analysis)
  - [ARP Request Analysis](#arp-request-analysis)
  - [ARP Reply Analysis](#arp-reply-analysis)
  - [Useful Wireshark Filters](#useful-wireshark-filters)
- [Useful Commands](#useful-commands)
  - [Windows](#windows)
  - [Linux](#linux)
- [Cybersecurity Relevance](#cybersecurity-relevance)
  - [Example Investigation](#example-investigation)
- [What I Learned](#what-i-learned)

---

# Introduction

Ethernet is one of the most commonly used technologies for communication in Local Area Networks.

When devices communicate through an Ethernet network, data is transmitted using **Ethernet frames**.

Ethernet communication uses **MAC addresses** to identify network interfaces on a local link.

Consider the following network:

```text
PC1 -------- Switch -------- PC2
```

Suppose PC1 wants to send data to PC2.

PC1 may know PC2's IP address:

```text
192.168.1.20
```

However, Ethernet needs a destination MAC address to create and send a frame on the local network.

This creates an important question:

```text
How does PC1 find the MAC address associated with 192.168.1.20?
```

For IPv4, the **Address Resolution Protocol (ARP)** is used to resolve an IPv4 address to a MAC address on the local network.

The communication process can be simplified as:

```text
IPv4 Address
     |
     v
    ARP
     |
     v
MAC Address
     |
     v
Ethernet Frame
```

Understanding Ethernet, MAC addresses, and ARP is essential for understanding local network communication.

---

# What is Ethernet?

**Ethernet** is a family of networking technologies commonly used in Local Area Networks.

Ethernet defines how devices communicate over a local network link.

Ethernet is standardized under:

```text
IEEE 802.3
```

Ethernet mainly operates at:

```text
OSI Layer 1 - Physical Layer

OSI Layer 2 - Data Link Layer
```

At Layer 1, Ethernet defines physical communication characteristics.

Examples include:

* Cables
* Connectors
* Electrical or optical signaling
* Data rates

At Layer 2, Ethernet defines:

* Ethernet frames
* MAC addressing
* Local frame delivery
* Error detection

A simple Ethernet network may look like:

```text
PC1 ----\
         \
PC2 ----- Switch ----- Server
         /
PC3 ----/
```

The switch forwards Ethernet frames between devices.

---

# Ethernet Communication

Ethernet communication occurs using **frames**.

Suppose PC1 wants to communicate with PC2.

```text
PC1                         PC2

192.168.1.10                192.168.1.20

MAC: AA:AA:AA:AA:AA:AA     MAC: BB:BB:BB:BB:BB:BB
```

PC1 creates an Ethernet frame.

The frame contains:

```text
Source MAC Address:
AA:AA:AA:AA:AA:AA

Destination MAC Address:
BB:BB:BB:BB:BB:BB
```

The frame is sent to the switch.

```text
PC1
 |
 | Ethernet Frame
 v
Switch
 |
 | Ethernet Frame
 v
PC2
```

The switch examines the destination MAC address and forwards the frame.

The destination IP address is processed by the Network Layer.

The destination MAC address is used for delivery across the current Ethernet link.

---

# Ethernet Frames

An **Ethernet frame** is the Layer 2 Protocol Data Unit used by Ethernet.

The frame contains network data and Ethernet control information.

A simplified Ethernet frame can be represented as:

```text
+------------------+
| Destination MAC  |
+------------------+
| Source MAC       |
+------------------+
| EtherType        |
+------------------+
| Payload          |
+------------------+
| FCS              |
+------------------+
```

The Ethernet payload may contain an IPv4 packet, IPv6 packet, or another supported protocol.

Example:

```text
Ethernet Frame
      |
      +-- Ethernet Header
      |
      +-- IPv4 Packet
      |
      +-- Frame Check Sequence
```

---

# Ethernet Frame Structure

A common Ethernet II frame contains the following fields:

| Field                | Size          |
| -------------------- | ------------- |
| Destination MAC      | 6 bytes       |
| Source MAC           | 6 bytes       |
| EtherType            | 2 bytes       |
| Payload              | 46-1500 bytes |
| Frame Check Sequence | 4 bytes       |

The commonly referenced Ethernet frame size from the Destination MAC field through the FCS is:

```text
Minimum: 64 bytes

Maximum: 1518 bytes
```

This value does not include some physical-layer transmission fields such as the preamble and Start Frame Delimiter.

VLAN tagging can also increase the frame size.

---

## Destination MAC Address

The destination MAC address identifies the intended Layer 2 destination.

Example:

```text
BB:BB:BB:BB:BB:BB
```

A switch examines this address when making forwarding decisions.

---

## Source MAC Address

The source MAC address identifies the interface that transmitted the Ethernet frame onto the current link.

Example:

```text
AA:AA:AA:AA:AA:AA
```

Switches use the source MAC address to learn which MAC addresses are reachable through specific switch ports.

---

## EtherType

The EtherType field identifies the protocol carried inside the Ethernet frame.

Common EtherType values include:

| EtherType | Protocol |
| --------- | -------- |
| 0x0800    | IPv4     |
| 0x0806    | ARP      |
| 0x86DD    | IPv6     |

Example:

```text
EtherType: 0x0800
```

indicates that the Ethernet payload contains an IPv4 packet.

---

## Payload

The payload contains the higher-layer data.

For example:

```text
Ethernet
   |
   v
IPv4
   |
   v
TCP
   |
   v
HTTP
```

The Ethernet payload contains the IPv4 packet.

The IPv4 packet contains the TCP segment.

The TCP segment contains application data.

---

## Frame Check Sequence

The **Frame Check Sequence (FCS)** is used for error detection.

Ethernet commonly uses a Cyclic Redundancy Check value as part of the FCS process.

The sender calculates a value based on the frame.

The receiver performs its own calculation.

If the calculated result indicates corruption, the frame is treated as damaged.

Ethernet itself does not provide TCP-style retransmission.

Higher-layer protocols or applications may handle recovery depending on the communication protocol.

---

# What is a MAC Address?

MAC stands for:

```text
Media Access Control
```

A **MAC address** is a Layer 2 address associated with a network interface.

Example:

```text
00:1A:2B:3C:4D:5E
```

A traditional Ethernet MAC address is:

```text
48 bits
```

or:

```text
6 bytes
```

Since one byte contains 8 bits:

```text
6 × 8 = 48 bits
```

MAC addresses are commonly written using hexadecimal values.

Examples:

```text
00:1A:2B:3C:4D:5E

AA:BB:CC:DD:EE:FF
```

Windows may display MAC addresses using hyphens:

```text
00-1A-2B-3C-4D-5E
```

---

# MAC Address Structure

A MAC address contains six octets.

Example:

```text
00:1A:2B:3C:4D:5E
```

It can be represented as:

```text
00 : 1A : 2B : 3C : 4D : 5E

 |    |    |    |    |    |
Byte Byte Byte Byte Byte Byte
```

MAC addresses contain information about whether an address is universally or locally administered and whether it represents an individual or group address.

For many introductory networking explanations, MAC addresses are also discussed using:

```text
OUI

Device-specific portion
```

The **Organizationally Unique Identifier (OUI)** is traditionally associated with the first 24 bits of a universally administered MAC address.

Example:

```text
00:1A:2B : 3C:4D:5E

   OUI   : Device Portion
```

The OUI can be associated with an organization or vendor.

However, modern operating systems can use MAC randomization or locally administered MAC addresses.

Therefore, a MAC address does not always reliably identify the physical hardware manufacturer.

---

# Types of MAC Addresses

MAC addresses can be used for different delivery types.

## Unicast MAC Address

A unicast MAC address identifies a single network interface.

Example:

```text
00:1A:2B:3C:4D:5E
```

Communication:

```text
PC1 ----------> PC2
```

One sender communicates with one destination interface.

---

## Broadcast MAC Address

The Ethernet broadcast MAC address is:

```text
FF:FF:FF:FF:FF:FF
```

A broadcast frame is delivered across the local Layer 2 broadcast domain.

Example:

```text
             ---> PC2
PC1 ---> Switch ---> PC3
             ---> PC4
```

ARP Requests commonly use the Ethernet broadcast destination MAC address.

---

## Multicast MAC Address

A multicast MAC address represents a group of interfaces interested in specific multicast traffic.

Example communication:

```text
           ---> PC2
Server ---->
           ---> PC3

PC4 does not need to process the multicast traffic as a group member.
```

Multicast is commonly used for group-based network communication.

---

# MAC Address vs IP Address

MAC addresses and IP addresses perform different functions.

| MAC Address                        | IP Address                              |
| ---------------------------------- | --------------------------------------- |
| Layer 2 address                    | Layer 3 address                         |
| Used for link-local frame delivery | Used for logical addressing and routing |
| Commonly 48 bits in Ethernet       | IPv4 is 32 bits                         |
| Used by switches                   | Used by routers                         |
| Example: AA:BB:CC:DD:EE:FF         | Example: 192.168.1.10                   |

Consider:

```text
PC1
IP:  192.168.1.10
MAC: AA:AA:AA:AA:AA:AA
```

The IP address identifies the interface logically within IP networking.

The MAC address is used for Ethernet frame delivery on the current local link.

A simple way to understand the difference is:

```text
IP Address  → Where the IP packet needs to go

MAC Address → Where the Ethernet frame goes on the current link
```

The destination IP address normally remains associated with the final IP destination while routers forward the packet.

The Layer 2 source and destination MAC addresses change as the packet moves across different Ethernet links.

---

# How a Switch Works

A switch connects devices in a local network.

Example:

```text
PC1 -------- Fa0/1
                |
PC2 -------- Fa0/2
                |
             Switch
                |
PC3 -------- Fa0/3
```

A Layer 2 switch primarily uses MAC addresses to forward Ethernet frames.

The switch maintains a table called the:

```text
MAC Address Table
```

It may also be called:

```text
CAM Table
```

CAM stands for:

```text
Content Addressable Memory
```

A simplified MAC address table may look like:

| MAC Address       | Port  |
| ----------------- | ----- |
| AA:AA:AA:AA:AA:AA | Fa0/1 |
| BB:BB:BB:BB:BB:BB | Fa0/2 |
| CC:CC:CC:CC:CC:CC | Fa0/3 |

The switch uses this table to decide where frames should be forwarded.

---

# MAC Address Table

Suppose the network contains:

```text
PC1 MAC: AA:AA:AA:AA:AA:AA

PC2 MAC: BB:BB:BB:BB:BB:BB
```

PC1 is connected to:

```text
Fa0/1
```

PC2 is connected to:

```text
Fa0/2
```

The switch may learn:

```text
AA:AA:AA:AA:AA:AA → Fa0/1

BB:BB:BB:BB:BB:BB → Fa0/2
```

The MAC address table allows the switch to forward known unicast frames efficiently.

---

# MAC Address Learning

Switches learn MAC addresses by examining the **source MAC address** of received Ethernet frames.

Consider:

```text
PC1
MAC: AA:AA:AA:AA:AA:AA
```

PC1 sends a frame through:

```text
Fa0/1
```

The switch receives:

```text
Source MAC:
AA:AA:AA:AA:AA:AA
```

The switch learns:

```text
AA:AA:AA:AA:AA:AA → Fa0/1
```

This process is called:

```text
MAC Address Learning
```

Important point:

```text
Switches learn from the SOURCE MAC address.
```

The destination MAC address is primarily used for the forwarding decision.

---

# Frame Forwarding

Suppose the switch has learned:

```text
AA:AA:AA:AA:AA:AA → Fa0/1

BB:BB:BB:BB:BB:BB → Fa0/2
```

PC1 sends a frame to PC2.

The frame contains:

```text
Source MAC:
AA:AA:AA:AA:AA:AA

Destination MAC:
BB:BB:BB:BB:BB:BB
```

The switch checks the destination MAC address.

It finds:

```text
BB:BB:BB:BB:BB:BB → Fa0/2
```

The switch forwards the frame through:

```text
Fa0/2
```

Diagram:

```text
PC1
 |
 | Frame
 v
Switch
 |
 | MAC Table Lookup
 v
Fa0/2
 |
 v
PC2
```

The switch does not need to send this known unicast frame through every other port.

---

# Unknown Unicast Flooding

What happens if the switch does not know the destination MAC address?

Consider:

```text
Destination MAC:
DD:DD:DD:DD:DD:DD
```

The switch checks its MAC table.

The address is not found.

This is called an:

```text
Unknown Unicast
```

The switch floods the frame through appropriate ports in the same VLAN, except the port where the frame was received.

Example:

```text
             ---> PC2
PC1 ---> Switch ---> PC3
             ---> PC4
```

If the destination responds, the switch can learn its source MAC address.

Example:

```text
DD:DD:DD:DD:DD:DD → Fa0/4
```

Future frames can then be forwarded directly toward the learned port.

---

# What is ARP?

ARP stands for:

```text
Address Resolution Protocol
```

ARP is used in IPv4 networks to resolve an IPv4 address to a Layer 2 address such as an Ethernet MAC address on the local network.

Example:

```text
IPv4 Address:
192.168.1.20

        |
        v

       ARP

        |
        v

MAC Address:
BB:BB:BB:BB:BB:BB
```

Suppose PC1 knows:

```text
PC2 IP = 192.168.1.20
```

But PC1 does not know:

```text
PC2 MAC = ?
```

PC1 uses ARP to discover the MAC address.

---

# Why is ARP Required?

IP operates at Layer 3.

Ethernet operates at Layer 2.

Suppose PC1 wants to send an IPv4 packet to PC2 on the same local network.

```text
PC1

IP: 192.168.1.10

        |
        |
        v

PC2

IP: 192.168.1.20
```

The IPv4 packet contains:

```text
Source IP:
192.168.1.10

Destination IP:
192.168.1.20
```

However, to transmit the packet through Ethernet, PC1 needs to create an Ethernet frame.

The Ethernet frame requires:

```text
Source MAC

Destination MAC
```

PC1 knows its own MAC address.

But it may not know PC2's MAC address.

ARP solves this problem.

```text
IP Address
    |
    v
ARP Resolution
    |
    v
MAC Address
```

---

# How ARP Works

Consider the following network:

```text
PC1

IP:  192.168.1.10
MAC: AA:AA:AA:AA:AA:AA

        |
        |
      Switch
        |
        |

PC2

IP:  192.168.1.20
MAC: BB:BB:BB:BB:BB:BB
```

PC1 wants to communicate with:

```text
192.168.1.20
```

The ARP process occurs in several steps.

---

## Step 1 - Check ARP Cache

PC1 first checks its ARP or neighbor cache.

Example:

```text
192.168.1.1  → CC:CC:CC:CC:CC:CC
```

PC1 searches for:

```text
192.168.1.20
```

If the mapping is not available, PC1 performs ARP resolution.

---

## Step 2 - Send ARP Request

PC1 sends an ARP Request.

Conceptually:

```text
Who has 192.168.1.20?

Tell 192.168.1.10
```

The Ethernet destination MAC address is:

```text
FF:FF:FF:FF:FF:FF
```

This is the Ethernet broadcast MAC address.

The switch forwards the broadcast through the VLAN or broadcast domain.

```text
             ---> PC2
PC1 ---> Switch ---> PC3
             ---> PC4
```

All devices in the broadcast domain can receive the broadcast frame.

---

## Step 3 - Target Identifies Its IP

PC2 receives the ARP Request.

PC2 checks the target IPv4 address.

```text
Target IP:
192.168.1.20
```

PC2 compares it with its own address.

```text
PC2 IP:
192.168.1.20
```

The address matches.

PC2 prepares an ARP Reply.

Other devices normally ignore the request because the target IPv4 address does not match their own address.

---

## Step 4 - ARP Reply

PC2 sends an ARP Reply to PC1.

Conceptually:

```text
192.168.1.20 is at BB:BB:BB:BB:BB:BB
```

The reply can be sent directly to PC1.

```text
PC2 ----------> PC1
```

The ARP Reply provides the MAC address associated with the target IPv4 address.

---

## Step 5 - Store the Mapping

PC1 stores the mapping in its ARP or neighbor cache.

```text
192.168.1.20 → BB:BB:BB:BB:BB:BB
```

PC1 can now create an Ethernet frame.

```text
Source MAC:
AA:AA:AA:AA:AA:AA

Destination MAC:
BB:BB:BB:BB:BB:BB
```

The IPv4 packet can now be transmitted inside the Ethernet frame.

---

# ARP Request

An ARP Request asks:

```text
Who has this IPv4 address?
```

Example:

```text
Who has 192.168.1.20?

Tell 192.168.1.10
```

The Ethernet destination MAC address is:

```text
FF:FF:FF:FF:FF:FF
```

Therefore:

```text
ARP Request → Commonly Broadcast
```

A simplified ARP Request contains information such as:

```text
Sender MAC Address

Sender IPv4 Address

Target MAC Address

Target IPv4 Address
```

Example:

```text
Sender MAC: AA:AA:AA:AA:AA:AA

Sender IP:  192.168.1.10

Target MAC: 00:00:00:00:00:00

Target IP:  192.168.1.20
```

The target MAC address is unknown during the request.

---

# ARP Reply

An ARP Reply answers the ARP Request.

Example:

```text
192.168.1.20 is at BB:BB:BB:BB:BB:BB
```

The reply provides the requested MAC address.

Example:

```text
Sender MAC: BB:BB:BB:BB:BB:BB

Sender IP:  192.168.1.20

Target MAC: AA:AA:AA:AA:AA:AA

Target IP:  192.168.1.10
```

ARP Replies are commonly sent as unicast frames to the requesting device.

Therefore:

```text
ARP Request → Broadcast

ARP Reply   → Commonly Unicast
```

---

# ARP Cache

Devices temporarily store IP-to-MAC mappings.

This information is stored in the:

```text
ARP Cache
```

or more generally in a neighbor table.

Example:

| IP Address   | MAC Address       |
| ------------ | ----------------- |
| 192.168.1.1  | CC:CC:CC:CC:CC:CC |
| 192.168.1.20 | BB:BB:BB:BB:BB:BB |
| 192.168.1.30 | DD:DD:DD:DD:DD:DD |

The cache improves network efficiency.

Without caching, a system might need to perform ARP resolution before every communication attempt.

ARP entries can expire after a period of time.

The exact timeout and neighbor state behavior depend on the operating system.

---

# ARP and the Default Gateway

ARP is not only used when communicating with another local host.

It is also used to find the MAC address of the default gateway.

Consider:

```text
PC1

IP:      192.168.1.10
Gateway: 192.168.1.1
```

PC1 wants to communicate with:

```text
8.8.8.8
```

PC1 uses its subnet information to determine that `8.8.8.8` is outside the local network.

Therefore, PC1 sends the packet toward the default gateway.

The IPv4 packet contains:

```text
Source IP:
192.168.1.10

Destination IP:
8.8.8.8
```

However, the Ethernet frame on the local network contains:

```text
Source MAC:
PC1 MAC

Destination MAC:
Router MAC
```

PC1 may use ARP to resolve:

```text
192.168.1.1
```

to the router's MAC address.

Example:

```text
192.168.1.1 → CC:CC:CC:CC:CC:CC
```

The final communication looks like:

```text
IP Destination:
8.8.8.8

MAC Destination:
Default Gateway MAC
```

This is an extremely important networking concept.

```text
Remote Destination

Destination IP  = Remote Host

Destination MAC = Local Next Hop
```

PC1 does **not** use ARP to discover the MAC address of `8.8.8.8`.

ARP works on the local link.

PC1 resolves the MAC address of its local next hop, usually the default gateway.

---

# ARP Packet Encapsulation

An important concept is that ARP is not encapsulated inside UDP.

ARP is also not encapsulated inside an IPv4 packet.

ARP is carried directly inside an Ethernet frame.

Example:

```text
Ethernet
   |
   v
ARP
```

The EtherType for ARP is:

```text
0x0806
```

Compare this with IPv4 communication:

```text
Ethernet
   |
   v
IPv4
   |
   v
TCP / UDP
```

For an ARP frame:

```text
[ Ethernet Header | ARP Message ]
```

For a TCP/IPv4 frame:

```text
[ Ethernet Header | IPv4 Header | TCP Header | Data ]
```

Therefore:

```text
ARP does not require TCP.

ARP does not require UDP.

ARP is not carried inside IPv4.
```

ARP communicates directly over the local Layer 2 network.

---

# Gratuitous ARP

A **Gratuitous ARP** is an ARP message sent without first receiving a normal ARP Request for that mapping.

A device may announce information about its own IPv4-to-MAC mapping.

Conceptually:

```text
192.168.1.20 is associated with BB:BB:BB:BB:BB:BB
```

Gratuitous ARP can be used for:

* Updating ARP caches
* Detecting duplicate IPv4 addresses
* High-availability systems
* Failover mechanisms
* Announcing MAC address changes

Example:

```text
Old Server
192.168.1.100 → AA:AA:AA:AA:AA:AA
```

After failover:

```text
New Server
192.168.1.100 → BB:BB:BB:BB:BB:BB
```

A Gratuitous ARP may help devices update their cached mapping.

Gratuitous ARP is legitimate network behavior.

However, unusual or unexpected ARP announcements may require investigation.

---

# ARP Spoofing and ARP Poisoning

ARP was designed for local network address resolution and does not provide strong built-in authentication of ARP claims.

This creates a security risk.

An attacker on the local network may send false ARP information.

This technique is commonly called:

```text
ARP Spoofing
```

or:

```text
ARP Poisoning
```

Consider:

```text
Router

IP:  192.168.1.1

MAC: RR:RR:RR:RR:RR:RR
```

A victim has the correct ARP mapping:

```text
192.168.1.1 → RR:RR:RR:RR:RR:RR
```

An attacker may attempt to send false ARP information claiming:

```text
192.168.1.1 → AA:AA:AA:AA:AA:AA
```

where:

```text
AA:AA:AA:AA:AA:AA
```

is the attacker's MAC address.

The victim's ARP cache may become poisoned.

Before:

```text
192.168.1.1 → Router MAC
```

After successful poisoning:

```text
192.168.1.1 → Attacker MAC
```

Traffic intended for the router may then be sent toward the attacker's network interface.

---

# Man-in-the-Middle Attack

ARP spoofing can be used as part of a **Man-in-the-Middle (MITM)** attack on a local network.

Normal communication:

```text
Victim ----------------> Router
```

After successful ARP manipulation:

```text
Victim ----> Attacker ----> Router
```

The attacker may attempt to position their system between the victim and the gateway.

The attacker could potentially:

* Observe traffic
* Forward traffic
* Modify unprotected traffic
* Disrupt communication

However, encrypted protocols such as HTTPS and SSH provide protection for application data when certificate or host-key validation and cryptographic protections are correctly used.

An attacker seeing encrypted traffic does not automatically mean they can read the application data.

For example:

```text
Victim ---> Attacker ---> HTTPS Server
```

The attacker may observe connection metadata, but properly validated TLS is designed to protect the encrypted application content from passive interception and many active attacks.

---

# Detecting Suspicious ARP Activity

Security analysts can investigate ARP activity for unusual behavior.

One possible indicator is an unexpected IPv4-to-MAC mapping change.

Example:

```text
192.168.1.1 → AA:AA:AA:AA:AA:AA
```

Later:

```text
192.168.1.1 → BB:BB:BB:BB:BB:BB
```

If:

```text
192.168.1.1
```

is the default gateway, the unexpected change may require investigation.

Possible explanations include:

* Router replacement
* High-availability failover
* Network reconfiguration
* Virtualization changes
* MAC randomization in another context
* ARP spoofing

Analysts should investigate context before classifying the event as malicious.

---

## Multiple IP Addresses Associated with One MAC

Example:

```text
192.168.1.1  → AA:AA:AA:AA:AA:AA

192.168.1.10 → AA:AA:AA:AA:AA:AA

192.168.1.20 → AA:AA:AA:AA:AA:AA
```

This pattern may require investigation.

However, legitimate systems such as routers, proxy ARP configurations, virtualized environments, or high-availability systems can produce unusual mappings.

Context is important.

---

## High Volume of ARP Replies

A device sending a large number of unsolicited ARP replies may require investigation.

Example:

```text
ARP Reply
ARP Reply
ARP Reply
ARP Reply
ARP Reply
```

Possible explanations include:

* Network failover
* Misconfiguration
* Network management activity
* ARP spoofing

The analyst should examine:

```text
Source MAC Address

Claimed IPv4 Address

Frequency

Affected Hosts

Switch Port

Device Identity
```

---

# Wireshark ARP Analysis

Wireshark can be used to inspect ARP traffic.

A basic display filter is:

```text
arp
```

This displays ARP packets.

An ARP Request may appear conceptually as:

```text
Who has 192.168.1.20?

Tell 192.168.1.10
```

An ARP Reply may appear as:

```text
192.168.1.20 is at BB:BB:BB:BB:BB:BB
```

---

## ARP Request Analysis

Important fields include:

```text
Opcode

Sender MAC Address

Sender IPv4 Address

Target MAC Address

Target IPv4 Address
```

Example:

```text
Opcode: Request

Sender MAC: AA:AA:AA:AA:AA:AA

Sender IP: 192.168.1.10

Target IP: 192.168.1.20
```

Ethernet destination:

```text
FF:FF:FF:FF:FF:FF
```

This indicates broadcast delivery.

---

## ARP Reply Analysis

Example:

```text
Opcode: Reply

Sender MAC: BB:BB:BB:BB:BB:BB

Sender IP: 192.168.1.20

Target MAC: AA:AA:AA:AA:AA:AA

Target IP: 192.168.1.10
```

The reply provides the requested IPv4-to-MAC mapping.

---

## Useful Wireshark Filters

Display all ARP traffic:

```text
arp
```

Display ARP requests:

```text
arp.opcode == 1
```

Display ARP replies:

```text
arp.opcode == 2
```

Search for an IPv4 address inside ARP traffic:

```text
arp.src.proto_ipv4 == 192.168.1.1
```

or:

```text
arp.dst.proto_ipv4 == 192.168.1.1
```

Display Ethernet frames from a specific MAC address:

```text
eth.src == aa:bb:cc:dd:ee:ff
```

Display frames sent to a specific MAC address:

```text
eth.dst == aa:bb:cc:dd:ee:ff
```

These filters are useful when investigating local network activity.

---

# Useful Commands

## Windows

View the ARP cache:

```powershell
arp -a
```

Example:

```text
Internet Address      Physical Address

192.168.1.1           aa-bb-cc-dd-ee-ff

192.168.1.20          11-22-33-44-55-66
```

View network configuration:

```powershell
ipconfig /all
```

The output can include the physical address of network interfaces.

---

## Linux

View the neighbor table:

```bash
ip neigh
```

Example:

```text
192.168.1.1 dev eth0 lladdr aa:bb:cc:dd:ee:ff REACHABLE
```

The older command is:

```bash
arp -a
```

View network interfaces:

```bash
ip addr
```

View link-layer information:

```bash
ip link
```

Example:

```text
link/ether aa:bb:cc:dd:ee:ff
```

Capture ARP traffic using tcpdump:

```bash
sudo tcpdump -i eth0 arp
```

The interface name may be different.

Examples include:

```text
eth0

ens33

enp0s3

wlan0
```

Available interfaces can be checked using:

```bash
ip link
```

---

# Cybersecurity Relevance

Ethernet, MAC addresses, and ARP are important for local network security analysis.

Security analysts may investigate:

* MAC address changes
* ARP Requests
* ARP Replies
* Duplicate IPv4 addresses
* Unusual ARP volume
* Gateway MAC address changes
* Unknown devices
* Local network attacks

Consider:

```text
Gateway IP:
192.168.1.1
```

Normal mapping:

```text
192.168.1.1 → AA:AA:AA:AA:AA:AA
```

Later:

```text
192.168.1.1 → BB:BB:BB:BB:BB:BB
```

A SOC analyst may investigate:

```text
When did the MAC address change?

Which device owns the new MAC?

Which switch port is the device connected to?

Was there a network failover?

Was a router replaced?

Are multiple hosts seeing the same mapping?

Is there unusual ARP traffic?
```

Switch logs, packet captures, endpoint data, and network monitoring tools can provide additional context.

---

## Example Investigation

Suppose a packet capture shows:

```text
192.168.1.50 is at AA:AA:AA:AA:AA:AA
```

A few seconds later:

```text
192.168.1.50 is at BB:BB:BB:BB:BB:BB
```

The analyst should not immediately assume an attack.

Possible explanations include:

```text
DHCP or address conflict

Virtual machine migration

High-availability failover

Network configuration change

ARP spoofing
```

The analyst should correlate the event with other data sources.

Examples:

```text
DHCP Logs

Switch Logs

Endpoint Logs

Authentication Logs

Packet Captures

Asset Inventory
```

This process helps determine whether the ARP activity is legitimate or suspicious.

---

# What I Learned

From studying Ethernet, I learned that Ethernet is a common technology used for communication in Local Area Networks.

Ethernet transmits data using frames.

A simplified Ethernet frame contains:

```text
Destination MAC

Source MAC

EtherType

Payload

Frame Check Sequence
```

MAC addresses are Layer 2 addresses commonly used for Ethernet frame delivery on a local link.

I learned that switches examine Ethernet frames and maintain MAC address tables.

Switches learn MAC addresses from the:

```text
Source MAC Address
```

and use the:

```text
Destination MAC Address
```

to make forwarding decisions.

If a destination MAC address is unknown, a switch may flood the frame through appropriate ports in the same VLAN.

I also learned that ARP resolves IPv4 addresses to MAC addresses on the local network.

The ARP process can be summarized as:

```text
Check ARP Cache
       ↓
Send ARP Request
       ↓
Receive ARP Reply
       ↓
Store IPv4-to-MAC Mapping
       ↓
Create Ethernet Frame
```

An ARP Request is commonly broadcast using:

```text
FF:FF:FF:FF:FF:FF
```

An ARP Reply is commonly sent as a unicast response.

ARP is not encapsulated inside TCP, UDP, or IPv4.

It is carried directly inside an Ethernet frame using EtherType:

```text
0x0806
```

When communicating with a remote network, a device does not ARP for the remote destination.

Instead, it resolves the MAC address of the local next hop, usually the default gateway.

```text
Destination IP  = Remote Host

Destination MAC = Local Next Hop
```

From a cybersecurity perspective, unusual ARP activity, unexpected MAC address changes, and suspicious ARP replies can indicate network problems or potential ARP spoofing.

Packet analysis tools can be used to investigate ARP traffic and understand local network communication.
