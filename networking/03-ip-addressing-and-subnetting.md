# IP Addressing and Subnetting

## Table of Contents

- [IP Addressing and Subnetting](#ip-addressing-and-subnetting)
  - [Table of Contents](#table-of-contents)
- [Introduction](#introduction)
- [What is an IP Address?](#what-is-an-ip-address)
- [IPv4 Addressing](#ipv4-addressing)
- [Understanding Octets](#understanding-octets)
- [Binary and IPv4 Addresses](#binary-and-ipv4-addresses)
- [Network ID and Host ID](#network-id-and-host-id)
- [Subnet Mask](#subnet-mask)
- [CIDR Notation](#cidr-notation)
- [Public and Private IP Addresses](#public-and-private-ip-addresses)
  - [Public IP Address](#public-ip-address)
  - [Private IP Address](#private-ip-address)
    - [10.0.0.0/8](#100008)
    - [172.16.0.0/12](#172160012)
    - [192.168.0.0/16](#1921680016)
- [Special IPv4 Addresses and Ranges](#special-ipv4-addresses-and-ranges)
  - [Loopback Address](#loopback-address)
  - [APIPA / IPv4 Link-Local Address](#apipa--ipv4-link-local-address)
  - [0.0.0.0](#0000)
  - [Limited Broadcast Address](#limited-broadcast-address)
  - [Documentation Address Ranges](#documentation-address-ranges)
- [Network and Broadcast Addresses](#network-and-broadcast-addresses)
  - [Important Misconception About 0 and 255](#important-misconception-about-0-and-255)
- [What is Subnetting?](#what-is-subnetting)
- [Why is Subnetting Used?](#why-is-subnetting-used)
  - [Better Address Management](#better-address-management)
  - [Reduced Broadcast Domains](#reduced-broadcast-domains)
  - [Improved Security](#improved-security)
  - [Easier Network Management](#easier-network-management)
- [Calculating the Number of Hosts](#calculating-the-number-of-hosts)
  - [Example: /26](#example-26)
  - [Important Exceptions](#important-exceptions)
- [Common CIDR Prefixes](#common-cidr-prefixes)
- [Finding the Network and Broadcast Address](#finding-the-network-and-broadcast-address)
- [Subnetting Examples](#subnetting-examples)
  - [Example 1 - /24 Network](#example-1---24-network)
  - [Example 2 - /25 Network](#example-2---25-network)
  - [Example 3 - /27 Network](#example-3---27-network)
  - [Example 4 - Dividing a /24 Network](#example-4---dividing-a-24-network)
    - [Subnet 1](#subnet-1)
    - [Subnet 2](#subnet-2)
    - [Subnet 3](#subnet-3)
    - [Subnet 4](#subnet-4)
- [IPv6](#ipv6)
  - [IPv6 Address Compression](#ipv6-address-compression)
  - [IPv4 vs IPv6](#ipv4-vs-ipv6)
- [Useful Networking Commands](#useful-networking-commands)
  - [Windows](#windows)
  - [Linux](#linux)
- [Cybersecurity Relevance](#cybersecurity-relevance)
  - [Identifying Internal Network Ranges](#identifying-internal-network-ranges)
  - [Detecting Network Scanning](#detecting-network-scanning)
  - [Detecting External Communication](#detecting-external-communication)
  - [Subnet-Based Security Rules](#subnet-based-security-rules)
  - [SIEM Example](#siem-example)
- [What I Learned](#what-i-learned)

---

# Introduction

Every device communicating through an IP network requires a logical address.

This logical address is called an **IP address**.

For example:

```text
192.168.1.10
```

IP addresses allow systems to identify the source and destination of network packets.

A simple communication example is:

```text
Source IP                        Destination IP

192.168.1.10 ------------------> 192.168.1.20
```

The source IP identifies the system sending the packet.

The destination IP identifies the system the packet is intended to reach.

IP addressing is one of the most important concepts in networking and cybersecurity.

---

# What is an IP Address?

IP stands for:

```text
Internet Protocol
```

An **IP address** is a logical address assigned to a network interface participating in an IP network.

IP addresses are used to identify interfaces and help deliver packets across networks.

Consider the following network:

```text
PC1
IP: 192.168.1.10
        |
        |
      Switch
        |
        |
PC2
IP: 192.168.1.20
```

PC1 can send an IP packet toward PC2 using PC2's IP address as the destination address.

An IP address is different from a MAC address.

```text
IP Address  → Logical Address

MAC Address → Link-Layer Address
```

IP addresses are used for packet delivery and routing across IP networks.

MAC addresses are used for frame delivery on technologies such as Ethernet within a local network segment.

The two major versions of IP are:

```text
IPv4

IPv6
```

---

# IPv4 Addressing

IPv4 stands for:

```text
Internet Protocol Version 4
```

An IPv4 address is **32 bits long**.

Example:

```text
192.168.1.10
```

The 32 bits are divided into four groups.

Each group contains 8 bits.

```text
8 bits + 8 bits + 8 bits + 8 bits = 32 bits
```

Each 8-bit group is called an **octet**.

Therefore:

```text
IPv4 Address = 4 Octets = 32 Bits
```

Example:

```text
192 . 168 . 1 . 10

 |     |    |    |
Octet Octet Octet Octet
```

IPv4 addresses are commonly written using **dotted decimal notation**.

Example:

```text
192.168.1.10
```

The dots separate the four octets.

---

# Understanding Octets

An **octet** is a group of 8 bits.

Example:

```text
11000000
```

Since one octet contains 8 bits, it can represent 256 possible values.

The minimum decimal value is:

```text
00000000 = 0
```

The maximum decimal value is:

```text
11111111 = 255
```

Therefore, each IPv4 octet can contain a value from:

```text
0 to 255
```

Example valid IPv4 address:

```text
192.168.1.10
```

Example invalid IPv4 address:

```text
192.300.1.10
```

The value `300` is invalid because an octet cannot exceed `255`.

---

# Binary and IPv4 Addresses

Computers process IPv4 addresses in binary.

Humans commonly write them in decimal format.

The binary place values of an octet are:

```text
128  64  32  16  8  4  2  1
```

Each position represents a power of two.

```text
2⁷  2⁶  2⁵  2⁴  2³  2²  2¹  2⁰
```

Consider:

```text
11000000
```

The binary calculation is:

```text
1 × 128 = 128
1 × 64  = 64
0 × 32  = 0
0 × 16  = 0
0 × 8   = 0
0 × 4   = 0
0 × 2   = 0
0 × 1   = 0
```

Therefore:

```text
128 + 64 = 192
```

So:

```text
11000000 = 192
```

Consider the IPv4 address:

```text
192.168.1.10
```

Its binary representation is:

```text
192 = 11000000

168 = 10101000

1   = 00000001

10  = 00001010
```

Therefore:

```text
192.168.1.10

=

11000000.10101000.00000001.00001010
```

Understanding binary is important for subnetting because subnet masks operate on individual bits.

---

# Network ID and Host ID

An IPv4 address contains two logical portions:

```text
Network Portion

Host Portion
```

The **network portion** identifies the IP network.

The **host portion** identifies an interface within that network.

Consider:

```text
192.168.1.10/24
```

With a `/24` prefix:

```text
192.168.1 | 10
-----------|---
 Network   | Host
```

The network is:

```text
192.168.1.0/24
```

The host portion for this address is represented by the final 8 bits.

Consider another device:

```text
192.168.1.20/24
```

Both addresses belong to:

```text
192.168.1.0/24
```

Example:

```text
192.168.1.10/24
192.168.1.20/24
192.168.1.50/24
```

All three belong to the same `/24` subnet.

Now consider:

```text
192.168.2.10/24
```

This address belongs to:

```text
192.168.2.0/24
```

Therefore:

```text
192.168.1.10/24

and

192.168.2.10/24
```

are located in different IP subnets.

Communication between different IP networks normally requires Layer 3 forwarding, usually through a router or Layer 3 switch.

---

# Subnet Mask

A **subnet mask** identifies which bits of an IPv4 address represent the network prefix.

Example:

```text
IP Address:  192.168.1.10

Subnet Mask: 255.255.255.0
```

The subnet mask in binary is:

```text
11111111.11111111.11111111.00000000
```

The `1` bits represent the network prefix.

The `0` bits represent host bits.

```text
11111111.11111111.11111111 | 00000000
          Network Bits       | Host Bits
```

Therefore:

```text
255.255.255.0 = /24
```

The IP address:

```text
192.168.1.10
```

with subnet mask:

```text
255.255.255.0
```

belongs to the network:

```text
192.168.1.0/24
```

---

# CIDR Notation

CIDR stands for:

```text
Classless Inter-Domain Routing
```

CIDR notation represents the number of network prefix bits.

Example:

```text
192.168.1.10/24
```

The `/24` means:

```text
24 bits are part of the network prefix
```

Binary subnet mask:

```text
11111111.11111111.11111111.00000000
```

Count the `1` bits:

```text
8 + 8 + 8 = 24
```

Therefore:

```text
/24
```

Another example:

```text
10.0.0.1/8
```

The subnet mask is:

```text
255.0.0.0
```

Binary:

```text
11111111.00000000.00000000.00000000
```

There are 8 network prefix bits.

Therefore:

```text
/8
```

Another example:

```text
172.16.1.10/16
```

Subnet mask:

```text
255.255.0.0
```

Binary:

```text
11111111.11111111.00000000.00000000
```

Therefore:

```text
/16
```

---

# Public and Private IP Addresses

IPv4 addresses can be divided into different address categories.

Two commonly discussed categories are:

```text
Public IP Addresses

Private IP Addresses
```

---

## Public IP Address

A public IP address is globally routable on the public Internet when allocated and advertised appropriately.

Public IP addresses are commonly assigned through Internet service providers and organizations under the global Internet addressing system.

Example network communication:

```text
Home Network
     |
   Router
     |
Public IP
     |
  Internet
```

Public IP addresses allow communication across the Internet.

---

## Private IP Address

Private IPv4 addresses are intended for use inside private networks.

Private addresses are not globally routed across the public Internet.

The private IPv4 ranges are defined by RFC 1918.

### 10.0.0.0/8

```text
10.0.0.0 - 10.255.255.255
```

CIDR:

```text
10.0.0.0/8
```

---

### 172.16.0.0/12

```text
172.16.0.0 - 172.31.255.255
```

CIDR:

```text
172.16.0.0/12
```

An important point is that the entire `172.0.0.0/8` range is not private.

For example:

```text
172.10.1.1
```

is not inside the RFC 1918 private range.

The private range starts at:

```text
172.16.0.0
```

and ends at:

```text
172.31.255.255
```

---

### 192.168.0.0/16

```text
192.168.0.0 - 192.168.255.255
```

CIDR:

```text
192.168.0.0/16
```

These addresses are commonly used in home and small office networks.

Example:

```text
192.168.1.1
192.168.1.10
192.168.0.100
```

Summary:

| Private Range                 | CIDR           |
| ----------------------------- | -------------- |
| 10.0.0.0 - 10.255.255.255     | 10.0.0.0/8     |
| 172.16.0.0 - 172.31.255.255   | 172.16.0.0/12  |
| 192.168.0.0 - 192.168.255.255 | 192.168.0.0/16 |

Private IP addresses commonly access the Internet through **Network Address Translation (NAT)**.

Example:

```text
192.168.1.10
192.168.1.20
192.168.1.30
        |
        |
      Router
        |
        | NAT
        |
     Public IP
        |
     Internet
```

NAT will be discussed in the routing and NAT documentation.

---

# Special IPv4 Addresses and Ranges

Some IPv4 addresses and ranges have special purposes.

---

## Loopback Address

The IPv4 loopback range is:

```text
127.0.0.0/8
```

The most commonly used loopback address is:

```text
127.0.0.1
```

It is commonly associated with:

```text
localhost
```

The loopback interface allows a system to communicate with itself through the local IP stack.

Example:

```bash
ping 127.0.0.1
```

It can be used when testing local network software and services.

---

## APIPA / IPv4 Link-Local Address

The IPv4 link-local range is:

```text
169.254.0.0/16
```

Addresses from this range may be automatically configured when a device is set to obtain an IPv4 address automatically but cannot obtain a DHCP lease.

Example:

```text
169.254.10.20
```

If a system unexpectedly receives a `169.254.x.x` address, a possible issue is:

```text
DHCP Server Unreachable
```

or:

```text
DHCP Configuration Problem
```

IPv4 link-local addresses are intended for communication on the local link and are not normally routed.

---

## 0.0.0.0

The address:

```text
0.0.0.0
```

represents an unspecified IPv4 address in many contexts.

Its exact meaning depends on where it is used.

For example, a server may listen on:

```text
0.0.0.0:80
```

This commonly means the service is listening on all suitable local IPv4 interfaces.

In routing:

```text
0.0.0.0/0
```

represents the IPv4 default route.

Example:

```text
0.0.0.0/0 via 192.168.1.1
```

This means:

```text
If no more specific route exists,
send the packet to 192.168.1.1
```

---

## Limited Broadcast Address

The IPv4 limited broadcast address is:

```text
255.255.255.255
```

It represents a broadcast to the local network segment.

Routers normally do not forward limited broadcasts.

---

## Documentation Address Ranges

Some IP ranges are reserved for documentation and examples.

Examples include:

```text
192.0.2.0/24

198.51.100.0/24

203.0.113.0/24
```

These ranges are useful when writing technical documentation because they should not represent normal public Internet destinations.

---

# Network and Broadcast Addresses

Every traditional IPv4 subnet has a **network address**.

Subnets that support broadcast also have a **directed broadcast address**.

Consider:

```text
192.168.1.0/24
```

The address range is:

```text
192.168.1.0 - 192.168.1.255
```

The network address is:

```text
192.168.1.0
```

The broadcast address is:

```text
192.168.1.255
```

The commonly usable host range is:

```text
192.168.1.1 - 192.168.1.254
```

Therefore:

```text
Network Address   = 192.168.1.0

First Host        = 192.168.1.1

Last Host         = 192.168.1.254

Broadcast Address = 192.168.1.255
```

---

## Important Misconception About 0 and 255

A common misconception is:

```text
0 and 255 are always reserved.
```

This is incorrect.

The network and broadcast addresses depend on the **subnet prefix**.

Consider:

```text
192.168.1.0/24
```

In this subnet:

```text
192.168.1.0   = Network Address

192.168.1.255 = Broadcast Address
```

However, consider:

```text
192.168.0.0/23
```

This subnet covers:

```text
192.168.0.0 - 192.168.1.255
```

The network address is:

```text
192.168.0.0
```

The broadcast address is:

```text
192.168.1.255
```

An address such as:

```text
192.168.1.0
```

is inside the `/23` subnet and is not the network address of that `/23`.

Therefore:

```text
Network and broadcast addresses are determined by the subnet mask.
```

They are not determined simply by whether the final octet is `0` or `255`.

---

# What is Subnetting?

**Subnetting** is the process of dividing a larger IP network into smaller networks called **subnets**.

Consider:

```text
192.168.1.0/24
```

The network contains 256 total IPv4 addresses.

The network can be divided into smaller subnets.

For example:

```text
192.168.1.0/25

192.168.1.128/25
```

The original `/24` network has been divided into two `/25` networks.

Diagram:

```text
Original Network

192.168.1.0/24
        |
        |
        +----------------------+
        |                      |
        v                      v

192.168.1.0/25        192.168.1.128/25
```

Each subnet is a separate IP network.

Routers or Layer 3 switches can route traffic between subnets when configured to do so.

---

# Why is Subnetting Used?

Subnetting provides several advantages.

## Better Address Management

IP addresses can be allocated based on network requirements.

Example:

```text
HR Department      → 50 Devices

Finance Department → 25 Devices

IT Department      → 100 Devices
```

Different subnet sizes can be designed based on the number of required addresses.

---

## Reduced Broadcast Domains

IPv4 broadcast traffic is limited to a broadcast domain.

Dividing a network into smaller routed subnets can reduce the number of systems receiving local broadcast traffic.

---

## Improved Security

Different departments or systems can be placed in different subnets.

Example:

```text
User Network
192.168.10.0/24

Server Network
192.168.20.0/24

Security Network
192.168.30.0/24
```

Traffic between these networks can be controlled using:

* Firewalls
* Router ACLs
* Security policies

---

## Easier Network Management

Smaller logical networks can be easier to organize and troubleshoot.

Example:

```text
VLAN 10 → HR

VLAN 20 → Finance

VLAN 30 → IT
```

Each VLAN can be associated with a different IP subnet.

---

# Calculating the Number of Hosts

IPv4 contains:

```text
32 bits
```

CIDR notation tells us how many bits belong to the network prefix.

The remaining bits are host bits.

Formula:

```text
Host Bits = 32 - Prefix Length
```

Example:

```text
/24
```

Calculation:

```text
32 - 24 = 8 Host Bits
```

The total number of addresses is:

```text
2^Host Bits
```

Therefore:

```text
2^8 = 256 Total Addresses
```

For a traditional broadcast-capable IPv4 subnet, the commonly used host calculation is:

```text
Usable Hosts = 2^Host Bits - 2
```

The two excluded addresses are commonly:

```text
Network Address

Broadcast Address
```

Therefore:

```text
256 - 2 = 254 Usable Hosts
```

---

## Example: /26

Host bits:

```text
32 - 26 = 6
```

Total addresses:

```text
2^6 = 64
```

Commonly usable hosts:

```text
64 - 2 = 62
```

Therefore:

```text
/26 = 64 Total Addresses

/26 = 62 Commonly Usable Host Addresses
```

---

## Important Exceptions

The `2^h - 2` formula is a useful beginner rule for traditional IPv4 subnets, but it has exceptions.

For example:

```text
/31
```

can be used on point-to-point links under modern networking standards.

A `/31` does not use the traditional network-and-broadcast host model in the same way.

Also:

```text
/32
```

represents a single IPv4 address.

Therefore, subnetting calculations should always consider the network design and protocol use case.

---

# Common CIDR Prefixes

| CIDR | Subnet Mask     | Total Addresses | Commonly Usable Hosts |
| ---- | --------------- | --------------: | --------------------: |
| /8   | 255.0.0.0       |      16,777,216 |            16,777,214 |
| /16  | 255.255.0.0     |          65,536 |                65,534 |
| /24  | 255.255.255.0   |             256 |                   254 |
| /25  | 255.255.255.128 |             128 |                   126 |
| /26  | 255.255.255.192 |              64 |                    62 |
| /27  | 255.255.255.224 |              32 |                    30 |
| /28  | 255.255.255.240 |              16 |                    14 |
| /29  | 255.255.255.248 |               8 |                     6 |
| /30  | 255.255.255.252 |               4 |                     2 |
| /31  | 255.255.255.254 |               2 |    Point-to-point use |
| /32  | 255.255.255.255 |               1 |        Single address |

A useful pattern is:

```text
/24 → 256 addresses

/25 → 128 addresses

/26 → 64 addresses

/27 → 32 addresses

/28 → 16 addresses

/29 → 8 addresses

/30 → 4 addresses
```

Every time the prefix increases by one bit, the subnet address count is divided by two.

---

# Finding the Network and Broadcast Address

One method for finding subnet ranges is the **block size method**.

Consider:

```text
192.168.1.70/26
```

The `/26` subnet mask is:

```text
255.255.255.192
```

The interesting octet is:

```text
192
```

Calculate the block size:

```text
256 - 192 = 64
```

Therefore, the subnet ranges increase by 64.

```text
0

64

128

192
```

The subnets are:

```text
192.168.1.0/26

192.168.1.64/26

192.168.1.128/26

192.168.1.192/26
```

The IP address is:

```text
192.168.1.70
```

`70` falls between:

```text
64 and 127
```

Therefore:

```text
Network Address = 192.168.1.64
```

The next subnet starts at:

```text
192.168.1.128
```

Therefore, the current subnet's broadcast address is one address before the next subnet:

```text
192.168.1.127
```

The host range is:

```text
192.168.1.65 - 192.168.1.126
```

Final answer:

```text
IP Address:        192.168.1.70/26

Network Address:   192.168.1.64

First Host:        192.168.1.65

Last Host:         192.168.1.126

Broadcast Address: 192.168.1.127
```

---

# Subnetting Examples

## Example 1 - /24 Network

Given:

```text
192.168.10.25/24
```

Subnet mask:

```text
255.255.255.0
```

Network address:

```text
192.168.10.0
```

Broadcast address:

```text
192.168.10.255
```

Host range:

```text
192.168.10.1 - 192.168.10.254
```

Commonly usable hosts:

```text
254
```

---

## Example 2 - /25 Network

Given:

```text
192.168.1.150/25
```

Subnet mask:

```text
255.255.255.128
```

Block size:

```text
256 - 128 = 128
```

Subnet ranges:

```text
0 - 127

128 - 255
```

The IP address `150` belongs to:

```text
128 - 255
```

Therefore:

```text
Network Address:   192.168.1.128

First Host:        192.168.1.129

Last Host:         192.168.1.254

Broadcast Address: 192.168.1.255
```

---

## Example 3 - /27 Network

Given:

```text
192.168.1.100/27
```

Subnet mask:

```text
255.255.255.224
```

Block size:

```text
256 - 224 = 32
```

Subnet ranges begin at:

```text
0

32

64

96

128

160

192

224
```

The IP address `100` belongs to the subnet beginning at:

```text
96
```

The next subnet begins at:

```text
128
```

Therefore:

```text
Network Address:   192.168.1.96

First Host:        192.168.1.97

Last Host:         192.168.1.126

Broadcast Address: 192.168.1.127
```

---

## Example 4 - Dividing a /24 Network

Suppose we have:

```text
192.168.1.0/24
```

We want four equal subnets.

To create four subnets:

```text
2^2 = 4
```

We borrow two host bits.

Original prefix:

```text
/24
```

New prefix:

```text
/26
```

Each `/26` contains:

```text
64 addresses
```

The four subnets are:

### Subnet 1

```text
Network:   192.168.1.0/26

Hosts:     192.168.1.1 - 192.168.1.62

Broadcast: 192.168.1.63
```

### Subnet 2

```text
Network:   192.168.1.64/26

Hosts:     192.168.1.65 - 192.168.1.126

Broadcast: 192.168.1.127
```

### Subnet 3

```text
Network:   192.168.1.128/26

Hosts:     192.168.1.129 - 192.168.1.190

Broadcast: 192.168.1.191
```

### Subnet 4

```text
Network:   192.168.1.192/26

Hosts:     192.168.1.193 - 192.168.1.254

Broadcast: 192.168.1.255
```

Diagram:

```text
192.168.1.0/24
        |
        +---- 192.168.1.0/26
        |
        +---- 192.168.1.64/26
        |
        +---- 192.168.1.128/26
        |
        +---- 192.168.1.192/26
```

---

# IPv6

IPv6 stands for:

```text
Internet Protocol Version 6
```

IPv6 was developed partly because the available IPv4 address space is limited.

IPv4 uses:

```text
32 bits
```

IPv6 uses:

```text
128 bits
```

An IPv6 address is written using hexadecimal values.

Example:

```text
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

IPv6 addresses contain eight groups of hexadecimal values.

Each group contains 16 bits.

```text
16 × 8 = 128 bits
```

---

## IPv6 Address Compression

IPv6 addresses can be shortened.

Consider:

```text
2001:0db8:0000:0000:0000:0000:0000:0001
```

Leading zeros inside each group can be removed:

```text
2001:db8:0:0:0:0:0:1
```

A single continuous sequence of zero groups can be replaced with:

```text
::
```

Therefore:

```text
2001:db8::1
```

The `::` compression can normally be used only once in an IPv6 address because the missing groups must be unambiguous.

---

## IPv4 vs IPv6

| IPv4                  | IPv6                                   |
| --------------------- | -------------------------------------- |
| 32-bit address        | 128-bit address                        |
| Dotted decimal        | Hexadecimal with colons                |
| Example: 192.168.1.10 | Example: 2001:db8::1                   |
| Limited address space | Extremely large address space          |
| Broadcast exists      | No IPv6 broadcast                      |
| NAT commonly used     | End-to-end addressing is more feasible |

IPv6 uses multicast and other mechanisms instead of IPv4-style broadcast.

---

# Useful Networking Commands

## Windows

View network configuration:

```powershell
ipconfig
```

Detailed configuration:

```powershell
ipconfig /all
```

Test connectivity:

```powershell
ping 192.168.1.1
```

View routing table:

```powershell
route print
```

Trace the network route:

```powershell
tracert google.com
```

---

## Linux

View IP addresses:

```bash
ip addr
```

Short form:

```bash
ip a
```

View routing table:

```bash
ip route
```

Test connectivity:

```bash
ping 192.168.1.1
```

Trace a route:

```bash
traceroute google.com
```

View neighboring IPv4 and IPv6 link-layer mappings:

```bash
ip neigh
```

---

# Cybersecurity Relevance

IP addressing and subnetting are essential for cybersecurity and SOC analysis.

Security analysts frequently investigate:

```text
Source IP Address

Destination IP Address

Internal IP Address

External IP Address

Subnet

Network Range
```

Consider the following log:

```text
Source IP:      192.168.1.50

Destination IP: 203.0.113.25

Destination Port: 443
```

The source address:

```text
192.168.1.50
```

belongs to the RFC 1918 private range:

```text
192.168.0.0/16
```

Therefore, it is likely representing an internal system in the logged environment.

The destination:

```text
203.0.113.25
```

is from a documentation range and is used here only as an example external-style destination.

In a real investigation, an analyst may investigate an actual public destination using authorized threat intelligence and organizational logs.

---

## Identifying Internal Network Ranges

Suppose an organization uses:

```text
10.10.0.0/16
```

The range covers addresses from:

```text
10.10.0.0
```

to:

```text
10.10.255.255
```

An analyst can use this information to distinguish the organization's internal address space from external addresses in logs.

---

## Detecting Network Scanning

Suppose logs show:

```text
192.168.1.50 → 192.168.1.1

192.168.1.50 → 192.168.1.2

192.168.1.50 → 192.168.1.3

192.168.1.50 → 192.168.1.4

192.168.1.50 → 192.168.1.5
```

A single system communicating with many addresses in a short period may indicate:

```text
Network Scanning
```

However, legitimate network management and security tools can produce similar patterns.

The analyst must investigate context before classifying the activity as malicious.

---

## Detecting External Communication

Consider:

```text
10.0.0.25 ---> External IP
```

If the internal system repeatedly communicates with an unusual external address, the activity may require investigation.

Possible explanations include:

* Legitimate cloud service
* Software updates
* Misconfigured application
* Malware
* Command and Control communication

Analysts should investigate:

```text
Destination IP

Destination Port

DNS Queries

Connection Frequency

Data Volume

Related Process

Threat Intelligence
```

---

## Subnet-Based Security Rules

Firewalls and access control systems often use CIDR notation.

Example:

```text
Allow: 192.168.10.0/24

Deny:  192.168.20.0/24
```

A firewall rule may allow one subnet to access a service while blocking another.

Example:

```text
HR Network
192.168.10.0/24
        |
        | Allowed
        v
Application Server
```

```text
Guest Network
192.168.50.0/24
        |
        | Blocked
        X
Application Server
```

Understanding CIDR is essential when analyzing:

* Firewall rules
* SIEM logs
* IDS alerts
* VPN configurations
* Cloud security groups
* Network ACLs

---

## SIEM Example

A SIEM query may filter an entire subnet.

Example concept:

```text
source_ip = 192.168.1.0/24
```

This represents systems within the subnet:

```text
192.168.1.0 - 192.168.1.255
```

A security analyst may search for outbound connections originating from this network.

Example investigation:

```text
Internal Subnet
      |
      v
192.168.1.0/24
      |
      v
Unusual External Destination
```

Subnet knowledge allows analysts to understand which systems belong to specific network environments.

---

# What I Learned

From studying IP addressing, I learned that IPv4 addresses are 32 bits long and contain four octets.

Each octet contains 8 bits and can represent decimal values from 0 to 255.

An IPv4 address contains network and host portions, and the subnet mask or CIDR prefix determines which bits belong to the network prefix.

I learned that private IPv4 addresses are located in three major RFC 1918 ranges:

```text
10.0.0.0/8

172.16.0.0/12

192.168.0.0/16
```

I also learned that network and broadcast addresses depend on the subnet mask.

The values `0` and `255` are not automatically reserved simply because they appear in the last octet.

Subnetting divides a larger IP network into smaller logical networks.

The number of host bits can be calculated using:

```text
Host Bits = 32 - Prefix Length
```

For traditional IPv4 subnets, the common host calculation is:

```text
2^Host Bits - 2
```

although `/31` and `/32` have special use cases.

I learned how to calculate network addresses, broadcast addresses, host ranges, and subnet sizes using CIDR notation and the block size method.

IPv6 uses 128-bit addresses and provides a significantly larger address space than IPv4.

From a cybersecurity perspective, IP addressing and subnetting are essential for analyzing network logs, understanding internal and external communication, identifying scanning patterns, and investigating suspicious network activity.
