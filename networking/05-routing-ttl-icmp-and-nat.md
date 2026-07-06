# 05 - Routing, TTL, ICMP and NAT

## Table of Contents

* [What is Routing?](#what-is-routing)
* [Router](#router)
* [Routing Table](#routing-table)
* [Default Gateway](#default-gateway)
* [Next Hop](#next-hop)
* [How a Router Forwards Packets](#how-a-router-forwards-packets)
* [TTL - Time To Live](#ttl---time-to-live)
* [ICMP](#icmp)
* [Ping](#ping)
* [Traceroute and Tracert](#traceroute-and-tracert)
* [NAT - Network Address Translation](#nat---network-address-translation)
* [Why NAT is Used](#why-nat-is-used)
* [Types of NAT](#types-of-nat)
* [Static NAT](#static-nat)
* [Dynamic NAT](#dynamic-nat)
* [PAT - Port Address Translation](#pat---port-address-translation)
* [Private IP to Public IP Communication](#private-ip-to-public-ip-communication)
* [NAT Table](#nat-table)
* [NAT vs ARP](#nat-vs-arp)
* [Cybersecurity and SOC Relevance](#cybersecurity-and-soc-relevance)
* [Useful Commands](#useful-commands)
* [What I Learned](#what-i-learned)

---

# What is Routing?

**Routing is the process of finding a path for a packet from one network to another network.**

Routing mainly happens at the:

> OSI Layer 3 - Network Layer

Routers use **IP addresses** to make routing decisions.

Example:

```text
PC A
192.168.1.10
      |
      |
   Router
      |
      |
PC B
10.0.0.10
```

PC A and PC B are on different networks.

```text
192.168.1.0/24
10.0.0.0/24
```

PC A cannot directly communicate with PC B.

The packet must pass through a **router**.

```text
PC A → Router → PC B
```

This process is called **routing**.

---

# Router

A **router is a networking device that connects different networks together**.

Example:

```text
Network A
192.168.1.0/24
        |
        |
      Router
        |
        |
Network B
10.0.0.0/24
```

The router has interfaces connected to different networks.

Example:

```text
Router Interface 1
192.168.1.1

Router Interface 2
10.0.0.1
```

The router can communicate with both networks.

A router mainly works using:

```text
IP Addresses
Routing Tables
Routing Protocols
```

A router checks the **destination IP address** of a packet and decides where the packet should go.

---

# Routing Table

A **routing table is a table containing information about available network paths**.

A router uses the routing table to decide where to send packets.

Example routing table:

| Destination Network | Gateway  | Interface |
| ------------------- | -------- | --------- |
| 192.168.1.0/24      | Direct   | eth0      |
| 10.0.0.0/24         | Direct   | eth1      |
| 172.16.0.0/16       | 10.0.0.2 | eth1      |

Example:

```text
Destination IP: 172.16.1.10
```

The router checks its routing table.

```text
172.16.0.0/16 → Send to 10.0.0.2
```

The router forwards the packet to:

```text
10.0.0.2
```

The routing table acts like a **map for the router**.

Simple analogy:

```text
Google Maps → Finds road path
Routing Table → Finds network path
```

---

## Routing Table in Linux

Use:

```bash
ip route
```

Example output:

```text
default via 192.168.1.1 dev eth0

192.168.1.0/24 dev eth0
```

Explanation:

```text
default via 192.168.1.1
```

means:

> If the destination network is unknown, send the packet to 192.168.1.1.

```text
192.168.1.0/24 dev eth0
```

means:

> The 192.168.1.0/24 network is directly connected through eth0.

---

## Routing Table in Windows

Use:

```cmd
route print
```

or:

```cmd
netstat -r
```

These commands display the system routing table.

---

# Default Gateway

A **default gateway is the router used by a device to communicate with other networks**.

Example:

```text
PC IP:          192.168.1.10
Subnet Mask:    255.255.255.0
Default Gateway: 192.168.1.1
```

Suppose the PC wants to communicate with:

```text
192.168.1.20
```

The destination is on the same network.

The PC communicates directly.

```text
PC → PC
```

Now suppose the destination is:

```text
8.8.8.8
```

The destination is outside the local network.

The PC sends the packet to the **default gateway**.

```text
PC → Default Gateway → Internet
```

Simple rule:

```text
Same Network
    ↓
Send Directly

Different Network
    ↓
Send to Default Gateway
```

The default gateway is usually the IP address of the router.

Example:

```text
Router IP: 192.168.1.1
```

Therefore:

```text
Default Gateway: 192.168.1.1
```

---

# Next Hop

The **next hop is the next router or device where a packet is forwarded**.

Example:

```text
PC → Router A → Router B → Router C → Server
```

For Router A:

```text
Next Hop = Router B
```

For Router B:

```text
Next Hop = Router C
```

Each router does not necessarily need to know the complete path.

It only needs to know:

> Where should I send this packet next?

Example:

```text
Destination Network: 10.0.0.0/24
Next Hop: 192.168.1.2
```

The router sends the packet to:

```text
192.168.1.2
```

---

# How a Router Forwards Packets

Suppose:

```text
PC A
192.168.1.10

Router
192.168.1.1
10.0.0.1

PC B
10.0.0.10
```

PC A wants to send data to PC B.

Destination IP:

```text
10.0.0.10
```

## Step 1 - PC Checks Destination Network

PC A checks:

```text
My Network:
192.168.1.0/24

Destination Network:
10.0.0.0/24
```

The destination is on a different network.

Therefore, PC A sends the packet to the **default gateway**.

```text
192.168.1.1
```

---

## Step 2 - PC Finds Router MAC Address

PC A uses **ARP**.

```text
Who has 192.168.1.1?
```

The router replies:

```text
192.168.1.1 is at AA:BB:CC:DD:EE:FF
```

PC A creates an Ethernet frame.

```text
Source MAC:
PC A MAC

Destination MAC:
Router MAC
```

The IP packet contains:

```text
Source IP:
192.168.1.10

Destination IP:
10.0.0.10
```

Important:

> The destination IP is still PC B.

The destination MAC is the router because the router is the next device.

---

## Step 3 - Router Receives the Packet

The router removes the Ethernet frame and checks the destination IP.

```text
Destination IP:
10.0.0.10
```

The router checks its routing table.

```text
10.0.0.0/24 → eth1
```

The router knows that the destination network is connected to `eth1`.

---

## Step 4 - Router Finds Destination MAC

The router uses ARP.

```text
Who has 10.0.0.10?
```

PC B replies with its MAC address.

The router creates a new Ethernet frame.

```text
Source MAC:
Router eth1 MAC

Destination MAC:
PC B MAC
```

The IP addresses remain:

```text
Source IP:
192.168.1.10

Destination IP:
10.0.0.10
```

The packet is delivered to PC B.

---

## Important Concept

During routing:

```text
MAC Address → Changes at every router
IP Address → Usually remains the same
```

Example:

```text
PC A → Router A
```

MAC addresses:

```text
PC A MAC → Router A MAC
```

Then:

```text
Router A → Router B
```

MAC addresses:

```text
Router A MAC → Router B MAC
```

IP addresses still remain:

```text
Source IP: PC A
Destination IP: Server
```

Exception:

> NAT can modify IP addresses.

---

# TTL - Time To Live

**TTL stands for Time To Live.**

TTL is a field inside the **IP header**.

Its purpose is to prevent packets from travelling forever inside a network.

Example:

```text
TTL = 64
```

Every router decreases the TTL by `1`.

Example:

```text
PC
TTL = 64

↓ Router 1

TTL = 63

↓ Router 2

TTL = 62

↓ Router 3

TTL = 61
```

If TTL becomes:

```text
TTL = 0
```

the router drops the packet.

The router may send an ICMP message:

```text
ICMP Time Exceeded
```

---

## Why is TTL Needed?

Imagine a routing loop.

```text
Router A → Router B
    ↑           ↓
    ← Router C ←
```

The packet could continuously move between routers.

```text
A → B → C → A → B → C → A
```

Without TTL, the packet could travel forever.

TTL solves this problem.

Example:

```text
TTL = 3
```

Packet path:

```text
Router A
TTL = 2

Router B
TTL = 1

Router C
TTL = 0
```

The packet is dropped.

Therefore:

> TTL prevents infinite routing loops.

---

## Common Default TTL Values

Different operating systems may use different default TTL values.

Common examples:

| Operating System | Common TTL |
| ---------------- | ---------- |
| Linux            | 64         |
| Windows          | 128        |
| Network Devices  | 255        |

These values are not always guaranteed.

Security analysts can sometimes use TTL as one clue during **OS fingerprinting**.

Example:

```text
TTL=128
```

The system may be Windows.

```text
TTL=64
```

The system may be Linux.

However:

> TTL alone cannot confirm the operating system.

---

# ICMP

**ICMP stands for Internet Control Message Protocol.**

ICMP is used for:

```text
Network Diagnostics
Error Reporting
Network Status Messages
```

ICMP works with the IP protocol.

Common ICMP messages include:

```text
Echo Request
Echo Reply
Destination Unreachable
Time Exceeded
```

ICMP is commonly used by:

```text
ping
traceroute
tracert
```

---

## ICMP Echo Request

When you run:

```bash
ping 8.8.8.8
```

your system sends:

```text
ICMP Echo Request
```

Simple meaning:

> Are you reachable?

---

## ICMP Echo Reply

If the destination responds, it sends:

```text
ICMP Echo Reply
```

Simple meaning:

> Yes, I am reachable.

Communication:

```text
PC → ICMP Echo Request → Server

PC ← ICMP Echo Reply ← Server
```

---

## ICMP Destination Unreachable

A router or host may send:

```text
ICMP Destination Unreachable
```

This means the destination cannot be reached.

Possible reasons:

```text
Network Unreachable
Host Unreachable
Port Unreachable
Communication Administratively Prohibited
```

Example:

```text
Destination Host Unreachable
```

This may indicate:

```text
Routing problem
Host is offline
ARP failure
Network configuration problem
```

---

## ICMP Time Exceeded

When TTL reaches `0`, the router sends:

```text
ICMP Time Exceeded
```

This message is very important for:

```text
traceroute
tracert
```

---

# Ping

`ping` is a network diagnostic command used to test whether a device is reachable.

Example:

```bash
ping 8.8.8.8
```

Ping commonly uses:

```text
ICMP Echo Request
ICMP Echo Reply
```

Communication:

```text
Your PC
   |
   | ICMP Echo Request
   ↓
Server
   |
   | ICMP Echo Reply
   ↓
Your PC
```

Example output:

```text
64 bytes from 8.8.8.8: icmp_seq=1 ttl=117 time=20 ms
```

Explanation:

```text
64 bytes
```

Size of the received data.

```text
icmp_seq=1
```

ICMP packet sequence number.

```text
ttl=117
```

Remaining TTL value.

```text
time=20 ms
```

Round Trip Time.

---

## Round Trip Time - RTT

RTT is the time required for a packet to travel:

```text
Your PC → Destination → Your PC
```

Example:

```text
time=20 ms
```

This means the complete round trip took approximately:

```text
20 milliseconds
```

Lower RTT generally means lower network delay.

---

## Ping Does Not Always Mean a Host is Down

Suppose:

```bash
ping example.com
```

does not receive a response.

This does not always mean the server is offline.

A firewall may block ICMP.

Example:

```text
PC → ICMP Echo Request → Firewall X
```

The server may still allow:

```text
HTTP
HTTPS
SSH
```

Therefore:

> No ping response does not always mean the host is down.

---

# Traceroute and Tracert

`traceroute` and `tracert` are used to discover the path packets take to reach a destination.

Linux:

```bash
traceroute google.com
```

Windows:

```cmd
tracert google.com
```

Example path:

```text
PC
 ↓
Router 1
 ↓
Router 2
 ↓
Router 3
 ↓
Google Server
```

Example output:

```text
1   192.168.1.1
2   10.10.0.1
3   172.16.5.1
4   8.8.8.8
```

Each router is called a **hop**.

---

## How Traceroute Uses TTL

Traceroute sends packets with increasing TTL values.

First packet:

```text
TTL = 1
```

Router 1 receives it.

```text
TTL becomes 0
```

Router 1 drops the packet and sends:

```text
ICMP Time Exceeded
```

Traceroute learns the IP address of Router 1.

Next packet:

```text
TTL = 2
```

Path:

```text
Router 1
TTL = 1

Router 2
TTL = 0
```

Router 2 sends:

```text
ICMP Time Exceeded
```

Traceroute learns Router 2.

The process continues.

```text
TTL 1 → Router 1
TTL 2 → Router 2
TTL 3 → Router 3
TTL 4 → Destination
```

This is how traceroute discovers the network path.

---

## Ping vs Traceroute

| Ping               | Traceroute                 |
| ------------------ | -------------------------- |
| Tests reachability | Finds network path         |
| Measures RTT       | Shows routers/hops         |
| Uses ICMP commonly | Uses TTL                   |
| Shows packet loss  | Helps locate path problems |

Simple explanation:

```text
Ping:
Can I reach the server?

Traceroute:
Which routers are between me and the server?
```

---

# NAT - Network Address Translation

**NAT stands for Network Address Translation.**

NAT translates one IP address into another IP address.

NAT commonly converts:

```text
Private IP Address
        ↓
Public IP Address
```

Example:

```text
Private IP:
192.168.1.10

Public IP:
49.x.x.x
```

Private IP addresses cannot normally be directly routed on the public Internet.

Therefore, a router performs NAT.

```text
PC
192.168.1.10
      |
      |
   NAT Router
      |
      |
49.x.x.x
      |
      |
Internet
```

---

# Why NAT is Used

NAT is mainly used because IPv4 addresses are limited.

An IPv4 address contains:

```text
32 bits
```

The total theoretical number of IPv4 addresses is:

```text
2^32
```

which is approximately:

```text
4.3 billion addresses
```

There are billions of devices connected to networks.

Giving every device a unique public IPv4 address is difficult.

NAT allows multiple private devices to use public IP addresses more efficiently.

Example home network:

```text
Laptop
192.168.1.10

Phone
192.168.1.11

TV
192.168.1.12

PC
192.168.1.13
```

All devices may access the Internet through one public IP.

```text
49.x.x.x
```

Diagram:

```text
192.168.1.10 ─┐
192.168.1.11 ─┤
192.168.1.12 ─┼── Router/NAT ── 49.x.x.x ── Internet
192.168.1.13 ─┘
```

---

# Types of NAT

The main NAT types are:

```text
Static NAT
Dynamic NAT
PAT
```

---

# Static NAT

**Static NAT creates a permanent one-to-one mapping between a private IP and a public IP.**

Example:

```text
192.168.1.10
      ↓
49.10.10.10
```

Mapping:

| Private IP   | Public IP   |
| ------------ | ----------- |
| 192.168.1.10 | 49.10.10.10 |

The mapping does not normally change.

```text
192.168.1.10 ↔ 49.10.10.10
```

Static NAT is useful when an internal server needs a consistent public IP.

Example:

```text
Web Server
Mail Server
Application Server
```

Simple analogy:

> One private IP gets one fixed public IP.

---

# Dynamic NAT

**Dynamic NAT maps private IP addresses to public IP addresses from a pool of available public IPs.**

Example public IP pool:

```text
49.10.10.10
49.10.10.11
49.10.10.12
```

Private devices:

```text
192.168.1.10
192.168.1.11
192.168.1.12
```

Possible mappings:

```text
192.168.1.10 → 49.10.10.11

192.168.1.11 → 49.10.10.10

192.168.1.12 → 49.10.10.12
```

The public IP is selected from the available pool.

The mapping may change.

Simple analogy:

> A private device temporarily gets an available public IP.

---

# PAT - Port Address Translation

**PAT stands for Port Address Translation.**

PAT allows multiple private devices to share a single public IP address.

PAT uses **port numbers** to identify different connections.

PAT is also called:

```text
NAT Overload
```

Example:

```text
PC A
192.168.1.10:5000

PC B
192.168.1.11:6000

PC C
192.168.1.12:7000
```

The router may translate them to:

```text
49.10.10.10:10001
49.10.10.10:10002
49.10.10.10:10003
```

Mapping:

| Private Address   | Public Address    |
| ----------------- | ----------------- |
| 192.168.1.10:5000 | 49.10.10.10:10001 |
| 192.168.1.11:6000 | 49.10.10.10:10002 |
| 192.168.1.12:7000 | 49.10.10.10:10003 |

Notice:

```text
Public IP is the same.
```

The port numbers are different.

This allows the router to identify each connection.

---

## PAT Example

Suppose two computers open a website.

PC A:

```text
192.168.1.10:5000
```

PC B:

```text
192.168.1.11:5000
```

Both connect to:

```text
Web Server
142.x.x.x:443
```

The NAT router creates mappings.

```text
192.168.1.10:5000
        ↓
49.x.x.x:40001
```

and:

```text
192.168.1.11:5000
        ↓
49.x.x.x:40002
```

The web server sees:

```text
49.x.x.x:40001

49.x.x.x:40002
```

When responses return, the router checks the port number.

```text
40001 → 192.168.1.10

40002 → 192.168.1.11
```

The router sends the response to the correct device.

---

## Static NAT vs Dynamic NAT vs PAT

| NAT Type    | Mapping                                  |
| ----------- | ---------------------------------------- |
| Static NAT  | One private IP to one fixed public IP    |
| Dynamic NAT | Private IP to public IP from a pool      |
| PAT         | Multiple private IPs share one public IP |

Example:

```text
Static NAT
1 Private IP → 1 Public IP
```

```text
Dynamic NAT
Private IP → Available Public IP
```

```text
PAT
Many Private IPs → 1 Public IP using Ports
```

PAT is commonly used in home networks.

---

# Private IP to Public IP Communication

Suppose:

```text
PC IP:
192.168.1.10

Router Public IP:
49.10.10.10

Web Server:
142.250.x.x
```

The PC opens a website.

## Step 1 - PC Creates Packet

```text
Source IP:
192.168.1.10

Destination IP:
142.250.x.x
```

The PC sends the packet to its default gateway.

---

## Step 2 - Router Performs NAT

The router receives:

```text
Source IP:
192.168.1.10
```

The router changes the source IP.

Before NAT:

```text
Source IP:
192.168.1.10
```

After NAT:

```text
Source IP:
49.10.10.10
```

The packet is sent to the Internet.

---

## Step 3 - Server Sends Response

The server sees:

```text
Source:
49.10.10.10
```

The server sends the response to:

```text
49.10.10.10
```

---

## Step 4 - Router Checks NAT Table

The router checks its NAT table.

Example:

```text
49.10.10.10:40001
        ↓
192.168.1.10:5000
```

The router translates the destination.

Before NAT:

```text
Destination:
49.10.10.10:40001
```

After NAT:

```text
Destination:
192.168.1.10:5000
```

The router sends the response to the PC.

Complete flow:

```text
PC
192.168.1.10
      |
      ↓
NAT Router
49.10.10.10
      |
      ↓
Internet
      |
      ↓
Web Server
```

---

# NAT Table

A **NAT table stores NAT translation information**.

Example:

| Inside Local      | Inside Global     |
| ----------------- | ----------------- |
| 192.168.1.10:5000 | 49.10.10.10:40001 |
| 192.168.1.11:6000 | 49.10.10.10:40002 |
| 192.168.1.12:7000 | 49.10.10.10:40003 |

The router uses this table to track connections.

Example incoming response:

```text
Destination:
49.10.10.10:40002
```

The router checks:

```text
40002 → 192.168.1.11:6000
```

The router forwards the packet to:

```text
192.168.1.11
```

Without the NAT table, the router would not know which internal device should receive the response.

---

# NAT vs ARP

NAT and ARP are completely different concepts.

| NAT                          | ARP                       |
| ---------------------------- | ------------------------- |
| Translates IP addresses      | Finds MAC addresses       |
| Used between networks        | Used inside local network |
| Router commonly performs NAT | Devices use ARP           |
| Private IP to public IP      | IP address to MAC address |

ARP example:

```text
192.168.1.1
      ↓
AA:BB:CC:DD:EE:FF
```

Meaning:

```text
IP Address → MAC Address
```

NAT example:

```text
192.168.1.10
      ↓
49.10.10.10
```

Meaning:

```text
Private IP → Public IP
```

Simple rule:

```text
ARP
IP → MAC
```

```text
NAT
IP → Different IP
```

---

# Cybersecurity and SOC Relevance

Routing, ICMP, TTL, and NAT are important concepts for cybersecurity and SOC analysts.

## 1. Understanding Network Traffic

A SOC analyst may see logs containing:

```text
Source IP
Destination IP
Source Port
Destination Port
Protocol
```

Example:

```text
Source IP: 192.168.1.10
Destination IP: 45.x.x.x
Destination Port: 443
```

The analyst should understand how the packet leaves the internal network through NAT.

---

## 2. Private IP vs Public IP Investigation

Suppose a firewall log shows:

```text
Internal IP:
192.168.1.50

Public IP:
49.x.x.x
```

The analyst may need to understand the NAT translation.

Example:

```text
192.168.1.50
      ↓ NAT
49.x.x.x
```

This is important during incident investigation.

---

## 3. NAT Logs

In large networks, multiple systems may share a public IP.

Example:

```text
192.168.1.10
192.168.1.11
192.168.1.12
```

All appear on the Internet as:

```text
49.x.x.x
```

To identify the actual internal system, analysts may need:

```text
NAT Logs
Timestamp
Source Port
Public Port
```

Example:

```text
49.x.x.x:40002
```

NAT logs may show:

```text
49.x.x.x:40002
        ↓
192.168.1.11:6000
```

Therefore, timestamps and port numbers can be very important during investigations.

---

## 4. ICMP Reconnaissance

Attackers may use ICMP to discover active hosts.

Example:

```bash
ping 192.168.1.10
```

An attacker may scan multiple systems.

```text
192.168.1.1
192.168.1.2
192.168.1.3
192.168.1.4
```

This may help identify live hosts.

This process is sometimes called:

```text
Host Discovery
```

Security tools may detect unusual ICMP activity.

---

## 5. ICMP Flood

An attacker may send a large number of ICMP packets.

Example:

```text
Attacker
   |
   | ICMP
   | ICMP
   | ICMP
   | ICMP
   ↓
Victim
```

This may cause resource consumption.

This type of activity may be related to a:

```text
Denial of Service - DoS
```

SOC analysts may investigate:

```text
High ICMP packet count
Repeated Echo Requests
Unusual source IPs
Traffic spikes
```

---

## 6. TTL Analysis

TTL can provide information about packet paths.

Example:

```text
TTL suddenly changes from 118 to 54
```

This may indicate:

```text
Different operating system
Different network path
Proxy or VPN
Packet manipulation
```

TTL should not be used alone as proof.

It is only one clue during traffic analysis.

---

## 7. Traceroute for Network Troubleshooting

A security or network analyst can use traceroute to identify where communication is failing.

Example:

```text
PC → Router 1 → Router 2 → X
```

If packets stop after Router 2, the problem may exist:

```text
At Router 2
After Router 2
On the next network path
```

Traceroute helps investigate:

```text
Routing Problems
Network Delays
Unexpected Paths
Connectivity Issues
```

---

# Useful Commands

## Linux

Show routing table:

```bash
ip route
```

Alternative:

```bash
route -n
```

Test connectivity:

```bash
ping 8.8.8.8
```

Ping a domain:

```bash
ping google.com
```

Send a limited number of ping packets:

```bash
ping -c 4 8.8.8.8
```

Trace network path:

```bash
traceroute google.com
```

Show ARP or neighbour table:

```bash
ip neigh
```

Alternative:

```bash
arp -a
```

---

## Windows

Show routing table:

```cmd
route print
```

Test connectivity:

```cmd
ping 8.8.8.8
```

Trace network path:

```cmd
tracert google.com
```

Show ARP table:

```cmd
arp -a
```

Show IP configuration:

```cmd
ipconfig
```

Detailed IP configuration:

```cmd
ipconfig /all
```

---

# Practical Example

Run:

```bash
ip route
```

Identify:

```text
Default Gateway
Network Address
Network Interface
```

Example:

```text
default via 192.168.1.1 dev eth0
```

The default gateway is:

```text
192.168.1.1
```

Now run:

```bash
ping -c 4 8.8.8.8
```

Observe:

```text
TTL
RTT
Packet Loss
```

Then run:

```bash
traceroute 8.8.8.8
```

Observe the routers between your system and the destination.

Finally, run:

```bash
ip neigh
```

Observe:

```text
IP Addresses
MAC Addresses
Network Interface
Neighbour State
```

Try to connect these concepts:

```text
Routing
   ↓
Default Gateway
   ↓
Next Hop
   ↓
TTL decreases
   ↓
ICMP messages
   ↓
NAT translation
   ↓
Internet
```

---

# What I Learned

After completing this section, I learned:

* Routing is the process of forwarding packets between different networks.
* Routers work mainly at Layer 3 of the OSI model.
* Routers use destination IP addresses to make forwarding decisions.
* A routing table stores available network paths.
* The default gateway is used to communicate with external networks.
* The next hop is the next router or device in a packet's path.
* MAC addresses change when packets move between network segments.
* Source and destination IP addresses usually remain the same during routing.
* NAT can modify IP addresses.
* TTL stands for Time To Live.
* Every router decreases the TTL value by one.
* A packet is dropped when its TTL reaches zero.
* TTL prevents packets from travelling forever because of routing loops.
* ICMP is used for network diagnostics and error reporting.
* Ping commonly uses ICMP Echo Request and Echo Reply messages.
* A failed ping does not always mean a system is offline.
* Traceroute discovers the routers between a source and destination.
* Traceroute uses increasing TTL values.
* NAT translates IP addresses.
* NAT commonly translates private IP addresses into public IP addresses.
* Static NAT creates a fixed one-to-one IP mapping.
* Dynamic NAT uses a pool of public IP addresses.
* PAT allows multiple private devices to share one public IP using port numbers.
* NAT tables help routers track translated connections.
* NAT and ARP are different concepts.
* ARP maps an IP address to a MAC address.
* NAT translates one IP address into another IP address.
* NAT logs, timestamps, and port numbers are important during SOC investigations.
* ICMP traffic can be used for network troubleshooting as well as reconnaissance.
* TTL values can provide clues during network traffic analysis.
