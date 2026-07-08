# 07 - DNS and DHCP

## Table of Contents

* [Application Layer Network Services](#application-layer-network-services)
* [What is DNS?](#what-is-dns)
* [Why DNS is Needed](#why-dns-is-needed)
* [Domain Name Structure](#domain-name-structure)
* [DNS Hierarchy](#dns-hierarchy)
* [DNS Resolution Process](#dns-resolution-process)
* [Recursive and Iterative DNS Queries](#recursive-and-iterative-dns-queries)
* [DNS Resolver](#dns-resolver)
* [DNS Cache](#dns-cache)
* [DNS Records](#dns-records)
* [A Record](#a-record)
* [AAAA Record](#aaaa-record)
* [CNAME Record](#cname-record)
* [MX Record](#mx-record)
* [NS Record](#ns-record)
* [TXT Record](#txt-record)
* [PTR Record](#ptr-record)
* [SOA Record](#soa-record)
* [DNS Ports and Protocols](#dns-ports-and-protocols)
* [What is DHCP?](#what-is-dhcp)
* [Why DHCP is Needed](#why-dhcp-is-needed)
* [DHCP DORA Process](#dhcp-dora-process)
* [DHCP Lease](#dhcp-lease)
* [DHCP Scope and Pool](#dhcp-scope-and-pool)
* [DHCP Reservation](#dhcp-reservation)
* [DHCP Relay](#dhcp-relay)
* [APIPA](#apipa)
* [DNS vs DHCP](#dns-vs-dhcp)
* [Cybersecurity and SOC Relevance](#cybersecurity-and-soc-relevance)
* [Useful Commands](#useful-commands)
* [Practical Labs](#practical-labs)
* [What I Learned](#what-i-learned)

---

# Application Layer Network Services

DNS and DHCP are important network services.

They help devices communicate and automatically configure network settings.

Simple overview:

```text
DNS
Domain Name → IP Address
```

```text
DHCP
Automatically Gives Network Configuration
```

Example:

```text
google.com
     ↓
DNS
     ↓
IP Address
```

DHCP example:

```text
Laptop Connects to Network
          ↓
DHCP Server
          ↓
IP Address
Subnet Mask
Default Gateway
DNS Server
```

Without DNS, users would need to remember IP addresses.

Without DHCP, administrators would need to manually configure every device.

---

# What is DNS?

**DNS stands for Domain Name System.**

DNS converts domain names into IP addresses.

Example:

```text
google.com
      ↓
DNS
      ↓
142.x.x.x
```

Computers communicate using IP addresses.

Humans prefer domain names.

Example:

```text
google.com
github.com
youtube.com
```

Instead of remembering:

```text
142.x.x.x
140.x.x.x
```

DNS performs the translation.

Simple definition:

> DNS is the naming system of the Internet that translates domain names into IP addresses.

---

# Why DNS is Needed

Suppose you want to open:

```text
example.com
```

Your browser needs the IP address of the server.

The browser cannot directly send packets to:

```text
example.com
```

At the Network Layer, the destination must be an IP address.

Example:

```text
Destination IP:
93.184.x.x
```

DNS finds this IP address.

Process:

```text
User Types Domain
        ↓
Browser
        ↓
DNS Query
        ↓
DNS Server
        ↓
IP Address Returned
        ↓
Browser Connects to Server
```

Simple analogy:

```text
Contact Name → Phone Number
```

Example:

```text
Rahul
   ↓
Contacts
   ↓
987xxxxxxx
```

DNS works similarly:

```text
example.com
     ↓
DNS
     ↓
IP Address
```

---

# Domain Name Structure

A domain name contains multiple parts.

Example:

```text
www.example.com
```

The structure is:

```text
www      example      com
 ↓          ↓          ↓
Host       Domain      TLD
```

---

## Top-Level Domain - TLD

The last part of a domain name is called the:

```text
Top-Level Domain
```

Examples:

```text
.com
.org
.net
.edu
.gov
.in
```

Example:

```text
example.com
        ↓
       .com
```

`.com` is the TLD.

Country-code TLD examples:

```text
.in → India
.uk → United Kingdom
.jp → Japan
```

---

## Second-Level Domain

Example:

```text
example.com
```

The second-level domain is:

```text
example
```

Another example:

```text
google.com
```

The second-level domain is:

```text
google
```

---

## Subdomain

A subdomain is placed before the main domain.

Example:

```text
mail.example.com
```

Here:

```text
mail
```

is the subdomain.

Other examples:

```text
blog.example.com

shop.example.com

api.example.com
```

Subdomains may point to different services or servers.

---

## Fully Qualified Domain Name - FQDN

FQDN stands for:

```text
Fully Qualified Domain Name
```

An FQDN represents the complete domain name of a host.

Example:

```text
www.example.com
```

Another example:

```text
mail.company.com
```

The complete DNS hierarchy technically ends with a root dot.

Example:

```text
www.example.com.
```

The final dot represents the DNS root.

It is usually hidden.

---

# DNS Hierarchy

DNS uses a hierarchical structure.

The hierarchy is:

```text
Root DNS Servers
        ↓
TLD DNS Servers
        ↓
Authoritative DNS Servers
```

Example for:

```text
www.example.com
```

The process may involve:

```text
Root Server
     ↓
.com TLD Server
     ↓
example.com Authoritative Server
```

---

## Root DNS Servers

Root DNS servers are at the top of the DNS hierarchy.

They do not normally provide the final IP address of every website.

Instead, they direct DNS queries to the correct TLD server.

Example query:

```text
Where is www.example.com?
```

Root server response:

```text
Ask the .com TLD server.
```

Simple role:

```text
Root Server
     ↓
Find Correct TLD Server
```

---

## TLD DNS Servers

TLD servers manage information for top-level domains.

Examples:

```text
.com
.org
.net
.in
```

Suppose the domain is:

```text
example.com
```

The `.com` TLD server may respond:

```text
Ask the authoritative DNS server for example.com.
```

Simple role:

```text
TLD Server
     ↓
Find Authoritative DNS Server
```

---

## Authoritative DNS Server

The authoritative DNS server contains DNS records for a domain.

Example:

```text
example.com
```

It may contain:

```text
A Record
MX Record
TXT Record
NS Record
```

The authoritative server can provide the final DNS answer.

Example:

```text
www.example.com
        ↓
93.184.x.x
```

Simple role:

```text
Authoritative DNS Server
          ↓
Provides DNS Record
```

---

# DNS Resolution Process

Suppose you type:

```text
www.example.com
```

into a browser.

The DNS resolution process begins.

---

## Step 1 - Browser Checks Cache

The browser may check its DNS cache.

```text
Do I already know the IP address?
```

If the answer exists:

```text
Use Cached IP
```

No DNS query may be required.

---

## Step 2 - Operating System Checks Cache

If the browser does not know the answer, the operating system may check its DNS cache.

Example:

```text
OS DNS Cache
```

If the record exists, the IP address is returned.

---

## Step 3 - Query DNS Resolver

If the answer is not cached, the system sends a DNS query to a DNS resolver.

Example DNS resolver:

```text
Router DNS
ISP DNS
Public DNS Resolver
```

Examples of public DNS resolvers include:

```text
8.8.8.8

1.1.1.1
```

The client asks:

```text
What is the IP address of www.example.com?
```

---

## Step 4 - Resolver Checks Cache

The DNS resolver checks its own cache.

If the answer exists:

```text
Return Cached Answer
```

If not, the resolver continues the DNS lookup.

---

## Step 5 - Resolver Queries Root Server

The resolver asks:

```text
Where is www.example.com?
```

The root server responds:

```text
Ask the .com TLD server.
```

---

## Step 6 - Resolver Queries TLD Server

The resolver asks the `.com` TLD server.

```text
Where is example.com?
```

The TLD server responds:

```text
Ask the authoritative DNS server for example.com.
```

---

## Step 7 - Resolver Queries Authoritative Server

The resolver asks:

```text
What is the IP address of www.example.com?
```

The authoritative server responds:

```text
www.example.com → 93.184.x.x
```

---

## Step 8 - Resolver Returns the IP

The DNS resolver sends the IP address to the client.

```text
Client ← IP Address ← DNS Resolver
```

The result may also be cached.

---

## Step 9 - Browser Connects to Server

The browser now knows the destination IP.

```text
www.example.com
        ↓
93.184.x.x
```

The browser can create a network connection.

Example:

```text
Client → TCP Connection → Web Server
```

Complete process:

```text
Domain Name
     ↓
DNS Resolver
     ↓
Root Server
     ↓
TLD Server
     ↓
Authoritative Server
     ↓
IP Address
     ↓
Browser Connects to Server
```

---

# Recursive and Iterative DNS Queries

DNS queries can involve recursive and iterative behaviour.

---

## Recursive Query

In a recursive query, the client asks the DNS resolver for the final answer.

Example:

```text
Client → DNS Resolver

What is the IP of example.com?
```

The client expects:

```text
Final IP Address
```

The resolver performs the required lookup.

Simple meaning:

> Find the answer for me.

Example:

```text
Client
   |
   | Recursive Query
   ↓
DNS Resolver
```

---

## Iterative Query

In an iterative query, a DNS server may return the best information it has.

Example:

```text
Resolver → Root Server
```

Root server:

```text
I do not have the final IP.

Ask the .com server.
```

The resolver then asks the TLD server.

Simple meaning:

> I do not have the final answer, but ask this server next.

---

## Recursive vs Iterative

| Recursive Query                    | Iterative Query                |
| ---------------------------------- | ------------------------------ |
| Client expects final answer        | Server may provide a referral  |
| Resolver performs lookup           | Resolver follows DNS hierarchy |
| Common between client and resolver | Common during DNS resolution   |

Simple flow:

```text
Client → Resolver
Recursive
```

```text
Resolver → Root/TLD/Authoritative
Iterative
```

---

# DNS Resolver

A DNS resolver receives DNS queries from clients.

Example:

```text
Laptop
   |
   | DNS Query
   ↓
DNS Resolver
```

The resolver may:

```text
Check Cache
Query DNS Servers
Return DNS Answers
```

A resolver is sometimes called a:

```text
Recursive Resolver
```

The resolver reduces the work required by client devices.

The client does not normally contact every DNS server itself.

Instead:

```text
Client
   ↓
DNS Resolver
   ↓
DNS Infrastructure
```

---

# DNS Cache

DNS caching temporarily stores DNS results.

Example:

```text
google.com → IP Address
```

After the first lookup, the result may be stored.

The next query may use the cached result.

Without cache:

```text
Client
 ↓
Resolver
 ↓
Root
 ↓
TLD
 ↓
Authoritative
```

With cache:

```text
Client
 ↓
Cached Answer
```

Benefits of DNS caching:

```text
Faster DNS Resolution
Less DNS Traffic
Reduced DNS Server Load
```

---

## TTL in DNS

DNS records also have a value called:

```text
TTL
```

Here TTL means:

```text
Time To Live
```

It defines how long a DNS record may remain cached.

Example:

```text
TTL = 3600 seconds
```

This means the DNS record may be cached for:

```text
1 hour
```

Important:

> DNS TTL and IP packet TTL are different concepts.

IP TTL:

```text
Decreases at Every Router
```

DNS TTL:

```text
Controls DNS Cache Duration
```

---

# DNS Records

DNS servers store information using DNS records.

Common DNS records include:

| Record | Purpose                |
| ------ | ---------------------- |
| A      | Domain to IPv4 address |
| AAAA   | Domain to IPv6 address |
| CNAME  | Domain alias           |
| MX     | Mail server            |
| NS     | Name server            |
| TXT    | Text information       |
| PTR    | Reverse DNS lookup     |
| SOA    | DNS zone information   |

---

# A Record

An A record maps a domain name to an IPv4 address.

Example:

```text
example.com
     ↓
93.184.x.x
```

Record:

```text
example.com → IPv4 Address
```

Simple rule:

```text
A Record
Domain → IPv4
```

Example query:

```bash
nslookup -type=A example.com
```

---

# AAAA Record

An AAAA record maps a domain name to an IPv6 address.

Example:

```text
example.com
     ↓
2001:db8::1
```

Simple rule:

```text
AAAA Record
Domain → IPv6
```

Why four As?

IPv6 addresses are larger than IPv4 addresses.

The name `AAAA` is used for IPv6 DNS records.

---

# CNAME Record

CNAME stands for:

```text
Canonical Name
```

A CNAME record creates an alias for another domain name.

Example:

```text
www.example.com
        ↓
example.com
```

Another example:

```text
blog.example.com
        ↓
hosting-provider.example
```

Simple rule:

```text
CNAME
Domain Alias → Another Domain
```

A CNAME normally points to another domain name, not directly to an IP address.

---

# MX Record

MX stands for:

```text
Mail Exchange
```

MX records identify mail servers for a domain.

Example:

```text
example.com
     ↓
mail.example.com
```

Simple rule:

```text
MX Record
Domain → Mail Server
```

When someone sends an email to:

```text
user@example.com
```

DNS can be used to find the mail server for:

```text
example.com
```

Example command:

```bash
nslookup -type=MX example.com
```

---

# NS Record

NS stands for:

```text
Name Server
```

NS records identify the authoritative DNS servers for a domain.

Example:

```text
example.com
     ↓
ns1.example.com
ns2.example.com
```

Simple rule:

```text
NS Record
Domain → Authoritative DNS Server
```

---

# TXT Record

TXT records store text information in DNS.

Example uses include:

```text
Domain Verification
Email Security
Service Verification
```

TXT records are commonly used with:

```text
SPF
DKIM
DMARC
```

These technologies are important for email security.

Example:

```text
example.com
     ↓
TXT Record
```

A security analyst may inspect TXT records during domain investigation.

---

# PTR Record

PTR stands for:

```text
Pointer Record
```

PTR records are used for reverse DNS lookups.

Normal DNS lookup:

```text
Domain Name
     ↓
IP Address
```

Reverse DNS lookup:

```text
IP Address
     ↓
Domain Name
```

Example:

```text
8.8.8.8
   ↓
DNS Name
```

Simple rule:

```text
PTR Record
IP → Domain
```

Example command:

```bash
nslookup 8.8.8.8
```

---

# SOA Record

SOA stands for:

```text
Start of Authority
```

The SOA record contains important information about a DNS zone.

It may contain:

```text
Primary Name Server
Administrator Information
Serial Number
Refresh Timer
Retry Timer
Expire Timer
```

Simple explanation:

> The SOA record contains administrative information about a DNS zone.

---

# DNS Ports and Protocols

DNS commonly uses:

```text
Port 53
```

DNS can use:

```text
UDP Port 53
TCP Port 53
```

---

## DNS over UDP

Most normal DNS queries commonly use UDP.

Example:

```text
Client → UDP 53 → DNS Server
```

UDP is useful because DNS queries are often small and fast.

There is no TCP three-way handshake.

---

## DNS over TCP

DNS may use TCP.

Examples include:

```text
Large DNS Responses
Zone Transfers
Specific DNS Operations
```

Communication:

```text
Client → TCP 53 → DNS Server
```

Important:

> DNS does not use only UDP. DNS can use both UDP and TCP.

---

# What is DHCP?

**DHCP stands for Dynamic Host Configuration Protocol.**

DHCP automatically provides network configuration to devices.

A DHCP server may provide:

```text
IP Address
Subnet Mask
Default Gateway
DNS Server
Lease Time
```

Example:

```text
Laptop Connects to Wi-Fi
          ↓
DHCP
          ↓
IP Address: 192.168.1.10
Subnet Mask: 255.255.255.0
Gateway: 192.168.1.1
DNS: 8.8.8.8
```

Without DHCP, these values may need to be configured manually.

---

# Why DHCP is Needed

Imagine an office with:

```text
1000 Computers
```

Without DHCP, an administrator may need to configure every computer manually.

Example:

```text
PC 1 → 192.168.1.10
PC 2 → 192.168.1.11
PC 3 → 192.168.1.12
...
```

This is difficult to manage.

Possible problems include:

```text
Duplicate IP Addresses
Wrong Subnet Masks
Wrong Gateways
Configuration Errors
```

DHCP automates the process.

```text
Device Connects
      ↓
DHCP Server
      ↓
Automatic Configuration
```

---

# DHCP DORA Process

DHCP commonly uses a four-step process called:

```text
DORA
```

DORA stands for:

```text
Discover
Offer
Request
Acknowledgment
```

The process is:

```text
DHCP Discover
      ↓
DHCP Offer
      ↓
DHCP Request
      ↓
DHCP ACK
```

Remember:

```text
D O R A
```

---

## Step 1 - DHCP Discover

A new device connects to the network.

The device may not have an IP address.

Example:

```text
Client IP:
0.0.0.0
```

The client does not know the DHCP server IP.

Therefore, it sends a broadcast message.

```text
DHCP Discover
```

Simple meaning:

> Is there any DHCP server available?

Communication:

```text
Client → Broadcast

DHCP DISCOVER
```

The client commonly uses:

```text
UDP Source Port 68
UDP Destination Port 67
```

---

## Step 2 - DHCP Offer

The DHCP server receives the Discover message.

The server offers an IP configuration.

Example:

```text
IP Address:
192.168.1.10

Subnet Mask:
255.255.255.0

Gateway:
192.168.1.1

DNS:
8.8.8.8
```

The server sends:

```text
DHCP OFFER
```

Simple meaning:

> You can use this IP address.

---

## Step 3 - DHCP Request

The client receives the offer.

The client sends:

```text
DHCP REQUEST
```

Simple meaning:

> I want to use the offered IP address.

Example:

```text
Client Requests:
192.168.1.10
```

The request may also help inform other DHCP servers which offer was selected.

---

## Step 4 - DHCP Acknowledgment

The DHCP server sends:

```text
DHCP ACK
```

ACK means:

```text
Acknowledgment
```

Simple meaning:

> The IP address is assigned to you.

The client configures its network interface.

Example:

```text
IP:
192.168.1.10

Subnet Mask:
255.255.255.0

Gateway:
192.168.1.1

DNS:
8.8.8.8
```

Complete DORA process:

```text
Client                     DHCP Server

   | ---- DISCOVER ------> |
   |                       |
   | <----- OFFER -------- |
   |                       |
   | ----- REQUEST ------> |
   |                       |
   | <------ ACK --------- |
```

Remember:

```text
Discover
Offer
Request
ACK
```

---

# DHCP Ports

DHCP uses UDP.

Common ports:

```text
UDP 67 → DHCP Server

UDP 68 → DHCP Client
```

Example:

```text
Client Port 68
      ↓
Server Port 67
```

Simple memory trick:

```text
Server = 67

Client = 68
```

---

# DHCP Lease

A DHCP-assigned IP address is usually temporary.

The client receives the IP for a specific period.

This period is called a:

```text
DHCP Lease
```

Example:

```text
IP Address:
192.168.1.10

Lease Time:
24 Hours
```

The client can use the IP address during the lease period.

Before the lease expires, the client may attempt to renew it.

---

## DHCP Lease Renewal

Suppose:

```text
Lease Time = 24 Hours
```

The client does not normally wait until the final second.

It may attempt to renew the lease earlier.

Example:

```text
Client → DHCP Server

Can I continue using 192.168.1.10?
```

If approved:

```text
DHCP ACK
```

The lease is renewed.

---

# DHCP Scope and Pool

A DHCP scope defines the IP addresses that a DHCP server can assign.

Example network:

```text
192.168.1.0/24
```

DHCP pool:

```text
192.168.1.100
        to
192.168.1.200
```

The DHCP server can assign addresses from this range.

Example:

```text
Laptop → 192.168.1.100

Phone → 192.168.1.101

PC → 192.168.1.102
```

Simple definition:

> A DHCP pool is a range of IP addresses available for automatic assignment.

---

## DHCP Exclusion

Some IP addresses may be excluded from the DHCP pool.

Example:

```text
192.168.1.1 → Router

192.168.1.2 → Server

192.168.1.3 → Printer
```

These addresses should not be automatically assigned to clients.

DHCP exclusions prevent the DHCP server from assigning specific IP addresses.

---

# DHCP Reservation

A DHCP reservation gives a specific IP address to a specific device.

The reservation may use the device's MAC address.

Example:

```text
MAC Address:
AA:BB:CC:DD:EE:FF

Reserved IP:
192.168.1.50
```

Whenever this device requests an IP:

```text
DHCP Server
     ↓
192.168.1.50
```

Simple explanation:

> DHCP reservation gives the same IP address to a specific device.

Useful for:

```text
Printers
Servers
Network Devices
Cameras
```

---

## Static IP vs DHCP Reservation

Static IP:

```text
IP Configured Manually on Device
```

DHCP Reservation:

```text
IP Assigned by DHCP Server
But Same IP is Reserved
```

Example:

```text
Static IP

Device Configuration
      ↓
192.168.1.50
```

```text
DHCP Reservation

DHCP Server
      ↓
MAC Address → 192.168.1.50
```

---

# DHCP Relay

DHCP Discover messages commonly use broadcasts.

Routers normally do not forward broadcasts between networks.

Example:

```text
Network A
192.168.1.0/24

       Router

Network B
10.0.0.0/24
```

Suppose the DHCP server is in Network B.

```text
DHCP Server:
10.0.0.10
```

A client in Network A sends:

```text
DHCP Discover Broadcast
```

The router may not normally forward the broadcast.

A DHCP relay can help.

```text
Client
   ↓
DHCP Discover
   ↓
Router / DHCP Relay
   ↓
DHCP Server
```

The DHCP relay forwards DHCP messages between networks.

On Cisco devices, a common command is:

```text
ip helper-address
```

Example:

```text
ip helper-address 10.0.0.10
```

Simple definition:

> DHCP relay allows clients to communicate with a DHCP server on another network.

---

# APIPA

APIPA stands for:

```text
Automatic Private IP Addressing
```

Windows may automatically assign an APIPA address when a DHCP server cannot be reached.

APIPA range:

```text
169.254.0.0/16
```

Example:

```text
169.254.10.20
```

If you see:

```text
169.254.x.x
```

it may indicate a DHCP problem.

Possible reasons:

```text
DHCP Server Down
Network Cable Problem
Wi-Fi Problem
VLAN Problem
DHCP Relay Problem
Firewall Problem
```

Simple meaning:

```text
169.254.x.x
      ↓
Check DHCP Connectivity
```

APIPA devices can communicate locally in some situations.

However, they normally cannot access other routed networks using normal network configuration.

---

# DNS vs DHCP

DNS and DHCP perform different functions.

| DNS                   | DHCP                          |
| --------------------- | ----------------------------- |
| Resolves domain names | Assigns network configuration |
| Domain to IP          | Provides IP address           |
| Uses port 53          | Uses ports 67 and 68          |
| Uses UDP and TCP      | Uses UDP                      |
| Helps locate servers  | Helps configure devices       |

Simple rule:

```text
DNS
Name → IP
```

```text
DHCP
Device → Network Configuration
```

Example:

```text
Laptop Connects to Network
          ↓
DHCP
          ↓
Gets IP Address
```

Then:

```text
User Opens google.com
          ↓
DNS
          ↓
Gets Server IP
```

DHCP happens during network configuration.

DNS happens when resolving domain names.

---

# Cybersecurity and SOC Relevance

DNS and DHCP are extremely important for SOC analysts.

---

## 1. DNS Logs

DNS logs may contain:

```text
Source IP
Queried Domain
Record Type
DNS Response
Timestamp
```

Example:

```text
Source IP:
192.168.1.50

Query:
example.com

Type:
A
```

A SOC analyst may investigate unusual domain queries.

---

## 2. Suspicious DNS Queries

Suppose a device repeatedly queries:

```text
ajshd82hshd.example.com

js82js91ks.example.com

qwe892jsks.example.com
```

The subdomains appear random.

This may require investigation.

Possible causes include:

```text
Malware
DNS Tunneling
Command and Control
Tracking
Legitimate Automated Services
```

Important:

> Random-looking domains alone do not prove malware.

Analysts should investigate additional evidence.

---

## 3. DNS Tunneling

DNS tunneling can use DNS queries to transfer data.

Normal DNS query:

```text
www.example.com
```

Suspicious-looking query:

```text
SGVsbG9Xb3JsZA.example.com
```

Encoded data may be placed inside subdomains.

Communication may look like:

```text
Compromised Host
       ↓
DNS Queries
       ↓
Attacker-Controlled DNS Server
```

Possible indicators:

```text
Very Long Domain Names
Large Number of DNS Queries
Random Subdomains
High TXT Query Volume
Repeated Queries to One Domain
```

SOC analysts may use SIEM tools to identify unusual DNS patterns.

---

## 4. Domain Generation Algorithms - DGA

Some malware may generate many domain names.

Example:

```text
xjs82ks.com

qwe91js.net

mz82hsa.org
```

This behaviour may be associated with a:

```text
Domain Generation Algorithm
```

The malware may attempt to find an attacker-controlled domain.

Example:

```text
Malware
   ↓
Domain 1
Domain 2
Domain 3
Domain 4
Domain 5
```

Most domains may fail.

One domain may successfully connect.

Possible SOC indicators:

```text
Many NXDOMAIN Responses
Random Domain Names
High DNS Query Volume
Repeated Failed DNS Queries
```

---

## 5. NXDOMAIN

NXDOMAIN means:

```text
Non-Existent Domain
```

It indicates that the requested DNS domain does not exist.

Example:

```text
Query:
randomxyz12345.com
```

DNS response:

```text
NXDOMAIN
```

A small number of NXDOMAIN responses is normal.

A large number from one host may require investigation.

Possible causes:

```text
Typing Errors
Application Problems
Malware
DGA Activity
Misconfiguration
```

---

## 6. DNS Spoofing and Cache Poisoning

DNS spoofing attempts to provide a false DNS answer.

Normal:

```text
bank.example
      ↓
Correct IP
```

Malicious response:

```text
bank.example
      ↓
Attacker IP
```

The victim may connect to the wrong server.

DNS cache poisoning attempts to place false DNS information inside a DNS cache.

Example:

```text
DNS Cache

bank.example → Attacker IP
```

Users may be redirected to malicious infrastructure.

Security controls may include:

```text
DNSSEC
Secure DNS Configuration
DNS Monitoring
DNS Filtering
```

---

## 7. Rogue DHCP Server

A rogue DHCP server is an unauthorized DHCP server on a network.

Example:

```text
Legitimate DHCP Server
192.168.1.1
```

Attacker connects another DHCP server.

```text
Rogue DHCP Server
192.168.1.50
```

The rogue server may provide:

```text
Malicious Default Gateway
Malicious DNS Server
Incorrect Network Configuration
```

Example:

```text
Victim
  ↓
DHCP
  ↓
Default Gateway = Attacker
```

This may allow traffic interception.

Network security technologies such as DHCP snooping can help protect against rogue DHCP servers.

---

## 8. DHCP Starvation

An attacker may send many DHCP requests using different fake MAC addresses.

Example:

```text
Fake MAC 1 → DHCP Request

Fake MAC 2 → DHCP Request

Fake MAC 3 → DHCP Request

Fake MAC 4 → DHCP Request
```

The DHCP server assigns addresses.

Eventually:

```text
DHCP Pool Exhausted
```

Legitimate devices may fail to receive IP addresses.

This is called:

```text
DHCP Starvation
```

Possible indicators:

```text
Large Number of DHCP Requests
Many New MAC Addresses
Rapid IP Allocation
DHCP Pool Exhaustion
```

---

## 9. DHCP Logs During Investigation

DHCP logs can help identify which device used an IP address at a specific time.

Example:

```text
IP Address:
192.168.1.50

MAC Address:
AA:BB:CC:DD:EE:FF

Lease Time:
10:00 AM - 6:00 PM
```

Suppose a firewall alert shows:

```text
192.168.1.50
```

The SOC analyst can check DHCP logs.

```text
192.168.1.50
      ↓
MAC Address
      ↓
Device
```

Important investigation fields include:

```text
IP Address
MAC Address
Hostname
Lease Start Time
Lease End Time
```

Timestamps are extremely important.

The same IP address may be assigned to another device later.

---

## 10. DNS and SIEM

DNS logs can be sent to a SIEM such as [Splunk](https://www.splunk.com/?utm_source=chatgpt.com).

Example DNS event:

```text
src_ip=192.168.1.50

query=random-domain.example

record_type=A

response=NXDOMAIN
```

A SOC analyst may search for:

```text
High NXDOMAIN Count
Rare Domains
Newly Observed Domains
Long Subdomains
High DNS Query Volume
Unusual TXT Queries
```

Example investigation idea:

```text
One Host
   ↓
1000 DNS Queries
   ↓
Random Domains
   ↓
Many NXDOMAIN Responses
```

This pattern may require investigation.

---

# Useful Commands

## Linux

Show DNS configuration:

```bash
cat /etc/resolv.conf
```

Example:

```text
nameserver 8.8.8.8
```

Perform DNS lookup:

```bash
nslookup example.com
```

Using `dig`:

```bash
dig example.com
```

Query A record:

```bash
dig A example.com
```

Query AAAA record:

```bash
dig AAAA example.com
```

Query MX record:

```bash
dig MX example.com
```

Query NS record:

```bash
dig NS example.com
```

Query TXT record:

```bash
dig TXT example.com
```

Reverse DNS lookup:

```bash
dig -x 8.8.8.8
```

Show IP configuration:

```bash
ip addr
```

Show routing information:

```bash
ip route
```

Request a DHCP lease using some Linux DHCP client configurations:

```bash
sudo dhclient
```

Release DHCP lease:

```bash
sudo dhclient -r
```

The exact DHCP client command may depend on the Linux distribution and network manager.

---

## Windows

DNS lookup:

```cmd
nslookup example.com
```

Show network configuration:

```cmd
ipconfig
```

Show detailed configuration:

```cmd
ipconfig /all
```

Display DNS cache:

```cmd
ipconfig /displaydns
```

Flush DNS cache:

```cmd
ipconfig /flushdns
```

Release DHCP configuration:

```cmd
ipconfig /release
```

Renew DHCP configuration:

```cmd
ipconfig /renew
```

Example troubleshooting process:

```text
ipconfig /all
      ↓
Check IP Address
      ↓
Check Default Gateway
      ↓
Check DNS Server
      ↓
Check DHCP Enabled
```

---

# Practical Labs

## Lab 1 - DNS Lookup

Run:

```bash
nslookup example.com
```

Observe:

```text
DNS Server
Resolved IP Address
```

Then run:

```bash
nslookup google.com
```

Compare the results.

Questions:

```text
Which DNS server answered?

What IP address was returned?

Was IPv4 or IPv6 returned?
```

---

## Lab 2 - Inspect DNS Records

Install `dig` if required.

On Ubuntu or Debian-based systems:

```bash
sudo apt install dnsutils
```

Run:

```bash
dig example.com
```

Observe:

```text
QUESTION SECTION

ANSWER SECTION
```

Now run:

```bash
dig MX google.com
```

Then:

```bash
dig NS google.com
```

Then:

```bash
dig TXT google.com
```

Try to identify:

```text
A Records
MX Records
NS Records
TXT Records
TTL Values
```

---

## Lab 3 - Observe DNS Traffic in Wireshark

Open Wireshark.

Use the display filter:

```text
dns
```

Run:

```bash
nslookup example.com
```

Observe the DNS query.

Check:

```text
Source IP
Destination IP
Source Port
Destination Port
Protocol
Query Name
Query Type
```

Look for:

```text
Standard Query
```

and:

```text
Standard Query Response
```

Try to find:

```text
A Record
```

or:

```text
AAAA Record
```

---

## Lab 4 - Observe DNS Cache

On Windows:

```cmd
ipconfig /displaydns
```

Look for cached domain names.

Then run:

```cmd
ipconfig /flushdns
```

The DNS cache is cleared.

Open a website.

Run:

```cmd
ipconfig /displaydns
```

again.

Observe newly cached DNS records.

This helps understand:

```text
DNS Query
      ↓
DNS Response
      ↓
DNS Cache
```

---

## Lab 5 - Inspect DHCP Configuration

On Windows:

```cmd
ipconfig /all
```

Find:

```text
DHCP Enabled
DHCP Server
IPv4 Address
Subnet Mask
Default Gateway
DNS Servers
Lease Obtained
Lease Expires
```

Try to connect the information.

Example:

```text
DHCP Server
      ↓
Assigned IPv4 Address
      ↓
Lease Obtained
      ↓
Lease Expires
```

---

## Lab 6 - DHCP Release and Renew

On a Windows lab VM, run:

```cmd
ipconfig /release
```

Observe the IP configuration.

Then run:

```cmd
ipconfig /renew
```

Observe the new configuration.

Run:

```cmd
ipconfig /all
```

Check:

```text
IP Address
DHCP Server
Lease Time
```

Use a lab VM so you do not unnecessarily interrupt an important active connection.

---

## Lab 7 - Capture DHCP DORA

Open Wireshark.

Use the filter:

```text
dhcp
```

On some Wireshark versions, the protocol may appear as:

```text
bootp
```

Release and renew the DHCP configuration on a lab machine.

Look for:

```text
DHCP Discover

DHCP Offer

DHCP Request

DHCP ACK
```

Try to identify the DORA process.

```text
D
Discover

O
Offer

R
Request

A
ACK
```

Check the UDP ports:

```text
67
68
```

---

## Lab 8 - DNS Investigation Practice

Open Wireshark and capture DNS traffic.

Visit several websites.

Apply:

```text
dns
```

Observe the queried domains.

Try to answer:

```text
Which device generated the query?

Which DNS server received the query?

What domain was queried?

What record type was requested?

What IP address was returned?
```

Think like a SOC analyst.

Example:

```text
Source IP
      ↓
DNS Query
      ↓
Domain
      ↓
DNS Response
      ↓
Resolved IP
```

---

# What I Learned

After completing this section, I learned:

* DNS stands for Domain Name System.
* DNS translates domain names into IP addresses.
* DNS allows users to use readable domain names instead of remembering IP addresses.
* Domain names have a hierarchical structure.
* TLD stands for Top-Level Domain.
* Subdomains appear before the main domain name.
* FQDN stands for Fully Qualified Domain Name.
* DNS hierarchy includes root servers, TLD servers, and authoritative DNS servers.
* Root DNS servers direct queries toward the correct TLD servers.
* TLD servers help locate authoritative DNS servers.
* Authoritative DNS servers contain DNS records for domains.
* DNS resolvers perform DNS lookups for clients.
* Recursive queries expect a final answer.
* Iterative DNS queries may return referrals to other DNS servers.
* DNS caching improves DNS resolution speed.
* DNS TTL controls how long a DNS record may remain cached.
* DNS TTL is different from IP packet TTL.
* A records map domains to IPv4 addresses.
* AAAA records map domains to IPv6 addresses.
* CNAME records create domain aliases.
* MX records identify mail servers.
* NS records identify authoritative name servers.
* TXT records store text information and are commonly used in email security.
* PTR records are used for reverse DNS lookups.
* SOA records contain DNS zone information.
* DNS commonly uses port 53.
* DNS can use both UDP and TCP.
* DHCP stands for Dynamic Host Configuration Protocol.
* DHCP automatically provides network configuration.
* DHCP can provide an IP address, subnet mask, default gateway, and DNS server.
* DHCP commonly uses the DORA process.
* DORA stands for Discover, Offer, Request, and Acknowledgment.
* DHCP uses UDP ports 67 and 68.
* UDP port 67 is commonly used by DHCP servers.
* UDP port 68 is commonly used by DHCP clients.
* DHCP leases assign IP addresses for a period of time.
* DHCP pools contain addresses available for automatic assignment.
* DHCP reservations provide specific devices with consistent IP addresses.
* DHCP relay allows DHCP communication across routed networks.
* APIPA addresses use the 169.254.0.0/16 range.
* An APIPA address may indicate a DHCP connectivity problem.
* DNS logs are important for SOC investigations.
* High NXDOMAIN counts may require investigation.
* Random or long subdomains may be indicators of DNS tunneling or automated activity.
* DGA malware may generate large numbers of domain names.
* DNS spoofing can redirect users to incorrect IP addresses.
* Rogue DHCP servers may provide malicious network configuration.
* DHCP starvation attempts to exhaust the DHCP address pool.
* DHCP logs can help map an IP address to a device at a specific time.
* DNS and DHCP logs can be analyzed using SIEM platforms.
