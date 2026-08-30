# 🌐 DevOps Day 10 — Linux & DevOps Networking

> **Goal:** Understand networking from fundamentals to Linux troubleshooting and connect these concepts with AWS/DevOps.

---

# 1. What is Networking?

**Networking** is the process of connecting two or more devices so they can communicate and exchange data or resources.

### Simple example

```text
Laptop
   │
   │ Wi-Fi
   ▼
Wi-Fi Router
   │
   ▼
Internet
   │
   ▼
Web Server
```

When you open a website:

```text
Your Browser
     ↓
DNS
     ↓
Server IP Address
     ↓
Internet
     ↓
Web Server
     ↓
HTTP/HTTPS Response
     ↓
Your Browser
```

Networking is extremely important in **DevOps, Cloud, AWS, Kubernetes and Linux administration**.

---

# 2. Important Networking Components

| Component | Meaning | Example |
|---|---|---|
| Host | Any device connected to a network | Laptop, server |
| NIC | Network Interface Card | Ethernet/Wi-Fi |
| IP Address | Logical network address | `192.168.1.10` |
| MAC Address | Hardware/interface address | `00:1A:2B:3C:4D:5E` |
| Switch | Connects devices in a LAN | Office switch |
| Router | Connects different networks | Home router |
| Gateway | Exit point to another network | `192.168.1.1` |
| DNS | Converts names to IP addresses | `google.com → IP` |
| DHCP | Automatically assigns network configuration | Router → Laptop |
| Firewall | Allows/blocks traffic | AWS SG, Linux firewall |
| Port | Identifies a service | SSH `22` |

---

# 3. Host

A **host** is any device connected to a network that can communicate using network protocols.

Examples:

```text
Laptop
Desktop
Mobile
EC2 Instance
Web Server
Database Server
```

---

# 4. NIC — Network Interface Card

NIC stands for **Network Interface Card**.

It allows a device to connect to a network.

Examples:

```text
Ethernet NIC
Wi-Fi NIC
Virtual NIC
```

On Linux:

```bash
ip link
```

or:

```bash
ip addr
```

A typical interface may look like:

```text
eth0
ens5
enp0s3
```

### Important

A cloud VM also has a **virtual network interface**.

For example, an AWS EC2 instance uses an **Elastic Network Interface (ENI)**.

---

# 5. IP Address

An **IP address** identifies a network interface on an IP network.

Example:

```text
192.168.1.10
```

There are two major IP versions:

```text
IPv4 → 32 bits
IPv6 → 128 bits
```

---

# 6. IPv4

IPv4 uses **32 bits**.

Example:

```text
192.168.1.10
```

IPv4 contains four octets:

```text
192 . 168 . 1 . 10
 │     │    │    │
0-255  0-255 0-255 0-255
```

Each octet contains 8 bits.

Therefore:

```text
8 × 4 = 32 bits
```

Total possible IPv4 addresses:

```text
2³² = 4,294,967,296
```

---

# 7. IPv6

IPv6 uses **128 bits**.

Example:

```text
2001:db8::10
```

Why IPv6?

The number of available IPv4 addresses is limited, while IPv6 provides an enormous address space.

```text
IPv4 → 32 bits
IPv6 → 128 bits
```

---

# 8. Private IPv4 Address Ranges

The three standard private IPv4 ranges are:

```text
10.0.0.0/8
```

Range:

```text
10.0.0.0 – 10.255.255.255
```

Second:

```text
172.16.0.0/12
```

Range:

```text
172.16.0.0 – 172.31.255.255
```

Third:

```text
192.168.0.0/16
```

Range:

```text
192.168.0.0 – 192.168.255.255
```

These addresses are normally used inside private networks.

---

# 9. Public vs Private IP

### Private IP

Used inside private networks.

Example:

```text
192.168.1.10
```

### Public IP

Used for communication over the public Internet.

Simplified example:

```text
Laptop
192.168.1.10
     │
     ▼
Router/NAT
     │
Public IP
     │
     ▼
Internet
```

---

# 10. MAC Address

MAC stands for **Media Access Control** address.

Example:

```text
00:1A:2B:3C:4D:5E
```

A MAC address is associated with a network interface.

MAC addresses are primarily used at the **Data Link Layer (Layer 2)**.

IP addresses operate at the **Network Layer (Layer 3)**.

Simple idea:

```text
IP
 ↓
Find destination network/device
 ↓
MAC
 ↓
Deliver Ethernet frame locally
```

---

# 11. Switch

A switch connects devices within a local network.

```text
        Switch
       /   |   \
      /    |    \
   PC-1   PC-2  Server
```

A switch primarily operates at **OSI Layer 2**.

It uses MAC addresses to forward Ethernet frames.

---

# 12. Router

A router connects different networks.

```text
LAN
 │
 ▼
Router
 │
 ▼
Internet
 │
 ▼
Another Network
```

Routers make forwarding decisions based primarily on **IP addresses and routing tables**.

---

# 13. Gateway

A gateway is the point through which traffic leaves one network to reach another network.

Example:

```text
Laptop
192.168.1.10
     │
     ▼
Gateway
192.168.1.1
     │
     ▼
Internet
```

On Linux:

```bash
ip route
```

You may see something similar to:

```text
default via 172.31.0.1 dev ens5
```

Here:

```text
default → default route
172.31.0.1 → gateway
ens5 → network interface
```

---

# 14. NAT

NAT = **Network Address Translation**

NAT translates addresses between different network contexts.

Common home-network example:

```text
Laptop
192.168.1.10
     │
     ▼
Router/NAT
     │
     ▼
Public Internet
```

Many private devices can access the Internet through a public address.

### AWS connection

AWS networking also involves NAT concepts. A **NAT Gateway** allows resources in private subnets to initiate connections to external networks without giving those resources public IPv4 addresses.

---

# 15. LAN

LAN = **Local Area Network**

Example:

```text
PC ──┐
PC ──┼── Switch
PC ──┘
```

Examples:

- Home network
- Office network
- College laboratory

---

# 16. WAN

WAN = **Wide Area Network**

A WAN connects networks across larger geographical areas.

```text
Office
  │
Router
  │
Internet/WAN
  │
Router
  │
Cloud/Data Center
```

The Internet is a very large interconnected network of networks.

---

# 17. Ports

A port identifies a particular network service/application endpoint on a host.

A TCP/UDP port number ranges from:

```text
0 – 65535
```

Therefore there are:

```text
65,536
```

possible port numbers.

### Important ports

| Port | Protocol/Service |
|---:|---|
| 22 | SSH |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 3306 | MySQL |
| 5432 | PostgreSQL |

Example:

```text
EC2
 │
 ├── 22  → SSH
 ├── 80  → HTTP
 ├── 443 → HTTPS
 └── 3306 → MySQL
```

---

# 18. HTTP vs HTTPS

### HTTP

```text
HTTP → Port 80
```

### HTTPS

```text
HTTPS → Port 443
```

HTTPS adds TLS encryption and authentication mechanisms to HTTP.

---

# 19. SSH

SSH = **Secure Shell**

Default TCP port:

```text
22
```

Used to securely access remote systems.

Example:

```bash
ssh -i key.pem ec2-user@<public-ip>
```

Architecture:

```text
Your Computer
      │
      │ TCP/22
      ▼
AWS EC2
```

---

# 20. Subnet

A **subnet** is a logical subdivision of an IP network.

Example:

```text
192.168.1.0/24
```

Typical IPv4 `/24`:

```text
Network address:    192.168.1.0
Usable hosts:       192.168.1.1 - 192.168.1.254
Broadcast address:  192.168.1.255
```

---

# 21. CIDR

CIDR = **Classless Inter-Domain Routing**

Example:

```text
192.168.1.0/24
```

The `/24` means:

```text
24 bits → network portion
8 bits  → host portion
```

Therefore:

```text
2⁸ = 256 total addresses
```

For a traditional IPv4 subnet, some addresses have special purposes, so the number of usable host addresses is commonly:

```text
256 - 2 = 254
```

---

# 22. Common CIDR Examples

```text
/32 → 1 IPv4 address
/24 → 256 total addresses
/16 → 65,536 total addresses
/8  → 16,777,216 total addresses
```

Formula:

```text
Total addresses = 2^(32 - prefix)
```

Example:

```text
/24

2^(32-24)
= 2⁸
= 256
```

### Important interview point

Do not confuse:

```text
/32
```

with 256 addresses.

`/32` represents exactly **one IPv4 address**.

---

# 23. Binary, Decimal and Hexadecimal

Computers ultimately process data in binary.

### Binary

```text
0, 1
```

Base:

```text
2
```

### Decimal

```text
0–9
```

Base:

```text
10
```

### Hexadecimal

```text
0–9
A–F
```

Base:

```text
16
```

Hexadecimal mapping:

```text
10 → A
11 → B
12 → C
13 → D
14 → E
15 → F
```

---

# 24. Powers of 2

Important for subnetting:

```text
2⁰  = 1
2¹  = 2
2²  = 4
2³  = 8
2⁴  = 16
2⁵  = 32
2⁶  = 64
2⁷  = 128
2⁸  = 256
2⁹  = 512
2¹⁰ = 1024
```

Memorize these for networking interviews.

---

# 25. OSI Model

The OSI model has seven layers.

```text
7 → Application
6 → Presentation
5 → Session
4 → Transport
3 → Network
2 → Data Link
1 → Physical
```

### Layer 7 — Application

Examples:

```text
HTTP
DNS
SSH
```

### Layer 6 — Presentation

Responsible for concepts such as:

```text
Encoding
Encryption
Compression
```

### Layer 5 — Session

Manages sessions/conversations between systems.

### Layer 4 — Transport

Protocols:

```text
TCP
UDP
```

### Layer 3 — Network

Concepts:

```text
IP
Routing
```

### Layer 2 — Data Link

Concepts:

```text
Ethernet
MAC
Frames
```

### Layer 1 — Physical

Examples:

```text
Cable
Fiber
Radio signals
Electrical signals
```

---

# 26. Easy OSI Memory Trick

From Layer 7 → Layer 1:

```text
Application
Presentation
Session
Transport
Network
Data Link
Physical
```

Remember:

**A P S T N D P**

---

# 27. DNS

DNS = **Domain Name System**

DNS translates domain names into IP addresses.

Instead of remembering:

```text
142.x.x.x
```

we use:

```text
google.com
```

Flow:

```text
Browser
   │
   ▼
DNS Query
   │
   ▼
DNS Server
   │
   ▼
IP Address
   │
   ▼
Web Server
```

Linux commands:

```bash
nslookup google.com
```

```bash
dig google.com
```

```bash
host google.com
```

Check resolver configuration:

```bash
cat /etc/resolv.conf
```

---

# 28. DNS Record Types

Important DNS records:

| Record | Purpose |
|---|---|
| A | Domain → IPv4 |
| AAAA | Domain → IPv6 |
| CNAME | Alias |
| MX | Mail server |
| NS | Authoritative name server |
| TXT | Text/verification information |

Examples:

```bash
dig google.com
```

```bash
dig google.com MX
```

```bash
dig google.com NS
```

---

# 29. DHCP

DHCP = **Dynamic Host Configuration Protocol**

DHCP automatically provides network configuration to clients.

It can provide:

```text
IP Address
Subnet Mask
Default Gateway
DNS Server
```

The common DHCP process is called **DORA**:

```text
Discover
   ↓
Offer
   ↓
Request
   ↓
Acknowledge
```

---

# 30. ARP

ARP = **Address Resolution Protocol**

ARP is used in IPv4 local networks to discover the MAC address associated with an IP address.

Example:

```text
IP Address
192.168.1.20
      ↓
     ARP
      ↓
MAC Address
AA:BB:CC:DD:EE:FF
```

Linux:

```bash
ip neigh
```

Older tool:

```bash
arp -a
```

`ip neigh` is preferred on modern Linux systems.

---

# 31. Routing

Routing determines where packets should be forwarded.

Check routing table:

```bash
ip route
```

Example:

```text
default via 172.31.0.1 dev ens5
172.31.0.0/20 dev ens5
```

Important terms:

```text
Destination
Gateway
Interface
Route
Default route
```

---

# 32. Network Interface Commands

Show all IP addresses:

```bash
ip addr
```

Short format:

```bash
ip -br addr
```

Show network interfaces:

```bash
ip link
```

Show IPv4:

```bash
ip -4 addr
```

Show IPv6:

```bash
ip -6 addr
```

Show a specific interface:

```bash
ip addr show ens5
```

> Interface names vary. Your system may use `ens5`, `eth0`, `enp0s3`, etc.

---

# 33. Check Internet Connectivity

Test using an IP:

```bash
ping 8.8.8.8
```

Test DNS + connectivity:

```bash
ping google.com
```

For a limited test:

```bash
ping -c 4 google.com
```

### Difference

```text
ping 8.8.8.8
```

tests connectivity to an IP without requiring DNS name resolution.

```text
ping google.com
```

requires DNS resolution first.

Therefore:

```text
IP ping works
DNS ping fails
        ↓
Possible DNS problem
```

---

# 34. traceroute

`traceroute` shows the network hops taken toward a destination.

```bash
traceroute google.com
```

On some systems:

```bash
tracepath google.com
```

Install on many RPM-based systems:

```bash
sudo dnf install traceroute -y
```

Conceptually:

```text
Your Server
    ↓
Router
    ↓
ISP
    ↓
Router
    ↓
Router
    ↓
Google
```

This is useful for locating where connectivity may be failing.

---

# 35. TCP

TCP = **Transmission Control Protocol**

TCP is:

- Connection-oriented
- Reliable
- Ordered
- Designed to detect loss and retransmit data

---

# 36. TCP Three-Way Handshake

Before normal TCP data transfer:

```text
Client                    Server

   SYN  -------------------->

        <---------------- SYN-ACK

   ACK  -------------------->

       Connection established
```

Remember:

```text
SYN
 ↓
SYN-ACK
 ↓
ACK
```

---

# 37. UDP

UDP = **User Datagram Protocol**

UDP is connectionless and has lower protocol overhead than TCP.

Conceptually:

```text
Client
  │
  │ Data
  ▼
Server
```

UDP does not perform the TCP three-way handshake.

Common examples include DNS traffic and real-time applications where low latency can be more important than guaranteed delivery.

---

# 38. TCP vs UDP

| TCP | UDP |
|---|---|
| Connection-oriented | Connectionless |
| Reliable delivery | No built-in delivery guarantee |
| Ordered delivery | No built-in ordering |
| More overhead | Lower overhead |
| Three-way handshake | No TCP handshake |
| Used by SSH/HTTP(S) | Used by many DNS and real-time applications |

### Interview answer

> **TCP is connection-oriented and provides reliable, ordered delivery, while UDP is connectionless and has lower overhead but does not provide TCP's reliability and ordering guarantees.**

---

# 39. `ss` Command

`ss` displays socket/network connection information.

Show listening TCP/UDP sockets:

```bash
ss -tuln
```

More detailed:

```bash
ss -tulpn
```

Meaning:

```text
t → TCP
u → UDP
l → Listening
n → Numeric
p → Process information
```

Example:

```bash
ss -tulpn
```

Could show:

```text
LISTEN 0 128 0.0.0.0:80
```

This means something is listening on TCP port 80.

---

# 40. Check Port 80

```bash
sudo lsof -i :80
```

You can also use:

```bash
ss -tulpn | grep :80
```

Check Apache process:

```bash
pidof httpd
```

---

# 41. Netcat

Netcat (`nc`) is a networking utility that can create or test TCP/UDP connections.

Install on Ubuntu:

```bash
sudo apt install netcat-openbsd -y
```

On RPM-based systems, package availability/name can vary.

Test HTTPS port:

```bash
nc -zv google.com 443
```

Here:

```text
-z → scan/check without sending normal data
-v → verbose
```

### Local test

Terminal 1:

```bash
nc -l 8080
```

Terminal 2:

```bash
nc localhost 8080
```

Type:

```text
hello
```

The text should appear in the first terminal.

---

# 42. curl

`curl` is commonly used to communicate with HTTP/HTTPS services and inspect responses.

Example:

```bash
curl https://example.com
```

Check version:

```bash
curl --version
```

Show only response headers:

```bash
curl -I https://example.com
```

Follow redirects:

```bash
curl -L https://example.com
```

Useful for DevOps troubleshooting:

```bash
curl http://localhost
```

If Apache is running locally, this can test the application without involving the external network.

---

# 43. tcpdump

`tcpdump` captures and displays network packets.

List available interfaces:

```bash
sudo tcpdump -D
```

Capture traffic:

```bash
sudo tcpdump -i eth0
```

However, your interface may not be called `eth0`.

First check:

```bash
ip link
```

Then use the actual interface:

```bash
sudo tcpdump -i ens5
```

This is an important tool for advanced network troubleshooting.

---

# 44. Firewall

A firewall controls network traffic according to rules.

Conceptually:

```text
Incoming Traffic
       │
       ▼
   Firewall
    /    \
 Allow   Deny
```

Examples:

```text
Allow TCP 22
Allow TCP 80
Allow TCP 443
Block everything else
```

---

# 45. UFW

UFW is commonly used on Ubuntu.

Install:

```bash
sudo apt install ufw -y
```

Enable:

```bash
sudo ufw enable
```

Check status:

```bash
sudo ufw status
```

Example:

```bash
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

---

# 46. firewalld

`firewalld` is commonly used on RHEL-family distributions and related systems.

Check state:

```bash
sudo firewall-cmd --state
```

Start:

```bash
sudo systemctl start firewalld
```

Enable at boot:

```bash
sudo systemctl enable firewalld
```

Reload:

```bash
sudo firewall-cmd --reload
```

List allowed services:

```bash
sudo firewall-cmd --list-services
```

Allow HTTP permanently:

```bash
sudo firewall-cmd --permanent --add-service=http
```

Then:

```bash
sudo firewall-cmd --reload
```

Verify:

```bash
sudo firewall-cmd --list-services
```

---

# 47. AWS Security Group vs Linux Firewall

This is **very important for AWS interviews**.

Traffic can encounter multiple security controls.

```text
Internet
   │
   ▼
AWS Security Group
   │
   ▼
EC2 Network Interface
   │
   ▼
Linux Firewall
   │
   ▼
Application
```

### AWS Security Group

Controls traffic at the AWS network-resource level.

Example:

```text
Allow TCP 22 from my IP
Allow TCP 80 from Internet
Allow TCP 443 from Internet
```

### Linux Firewall

Runs inside the operating system.

Examples:

```text
ufw
firewalld
nftables
```

---

# 48. Important Linux Configuration Directory

Linux configuration files are commonly stored under:

```bash
/etc
```

Example:

```bash
cat /etc/resolv.conf
```

shows DNS resolver configuration.

---

# 49. Useful Networking Commands Cheat Sheet

| Command | Purpose |
|---|---|
| `ip addr` | Show IP addresses |
| `ip link` | Show network interfaces |
| `ip route` | Show routing table |
| `ip neigh` | Show neighbor/ARP information |
| `ping` | Test connectivity |
| `ss` | Show sockets/connections |
| `nslookup` | DNS lookup |
| `dig` | Detailed DNS lookup |
| `host` | DNS lookup |
| `traceroute` | Show network path |
| `tracepath` | Trace network path |
| `curl` | Test HTTP/HTTPS |
| `nc` | Test TCP/UDP connections |
| `tcpdump` | Capture packets |
| `lsof` | Show processes/files/network sockets |

---

# 50. Modern vs Older Commands

Prefer modern Linux commands where possible.

| Older | Modern |
|---|---|
| `ifconfig` | `ip addr` |
| `route` | `ip route` |
| `arp -a` | `ip neigh` |
| `netstat` | `ss` |

You may still encounter the older commands in existing systems and interviews.

---

# 51. Practical Lab — Network Investigation

Run these commands on your Linux/EC2 machine:

```bash
ip addr
```

```bash
ip -br addr
```

```bash
ip link
```

```bash
ip route
```

```bash
ip neigh
```

```bash
ping -c 4 8.8.8.8
```

```bash
ping -c 4 google.com
```

```bash
nslookup google.com
```

```bash
dig google.com
```

```bash
host google.com
```

```bash
ss -tulpn
```

```bash
curl -I https://example.com
```

Then investigate your web server:

```bash
sudo systemctl status httpd
```

```bash
ss -tulpn | grep :80
```

```bash
curl http://localhost
```

```bash
sudo lsof -i :80
```

---

# 52. DevOps Troubleshooting Scenario #1

## Website is running but users cannot access it.

Suppose Apache is installed:

```bash
sudo systemctl status httpd
```

It says:

```text
active (running)
```

But:

```text
http://<EC2-Public-IP>
```

doesn't work.

Don't immediately reinstall Apache.

Check layer by layer:

### Step 1 — Is the service running?

```bash
sudo systemctl status httpd
```

### Step 2 — Is port 80 listening?

```bash
ss -tulpn | grep :80
```

### Step 3 — Does the application work locally?

```bash
curl http://localhost
```

### Step 4 — Check Linux firewall

```bash
sudo firewall-cmd --list-services
```

or:

```bash
sudo ufw status
```

### Step 5 — Check AWS Security Group

Verify inbound:

```text
TCP 80
Source → appropriate allowed source
```

### Step 6 — Check public IP

Make sure you are using the current public IP/DNS.

---

# 53. DevOps Troubleshooting Scenario #2

## SSH is not working

Suppose:

```bash
ssh -i key.pem ec2-user@<public-ip>
```

fails.

Check:

```text
1. Is EC2 running?
2. Is the public IP correct?
3. Is Security Group allowing TCP 22?
4. Is the instance reachable?
5. Is sshd running?
6. Is the key correct?
7. Is the private key permission correct?
8. Is a network ACL/routing issue involved?
```

On the server:

```bash
sudo systemctl status sshd
```

Check port:

```bash
ss -tulpn | grep :22
```

---

# 54. DevOps Troubleshooting Scenario #3

## Ping IP works but domain doesn't work

Suppose:

```bash
ping 8.8.8.8
```

works.

But:

```bash
ping google.com
```

fails.

This suggests the machine may have Internet connectivity but a **DNS resolution problem**.

Check:

```bash
cat /etc/resolv.conf
```

Then:

```bash
nslookup google.com
```

or:

```bash
dig google.com
```

---

# 55. DevOps Troubleshooting Scenario #4

## Service is running but port is closed

Check:

```bash
sudo systemctl status httpd
```

Then:

```bash
ss -tulpn | grep :80
```

If nothing is listening on port 80:

```text
Application/service
       ↓
Not listening on 80
       ↓
External connection fails
```

The problem is probably at the service/application configuration rather than the network firewall alone.

---

# 56. Why Does EC2 Public IP Change After Stop/Start?

A normal EC2 public IPv4 address can change when an instance is stopped and later started.

Why?

AWS can release the automatically assigned public IPv4 address when the instance is stopped and assign another public IPv4 address when it starts again.

For a stable public IPv4 address, AWS provides **Elastic IP** functionality.

Remember:

```text
Stop → Start
       ↓
Public IPv4 may change
```

An Elastic IP can provide a persistent public IPv4 address associated with your AWS resources.

---

# 57. What Happens If a NIC Is Replaced?

A NIC has its own network-interface identity and configuration.

If a physical or virtual NIC is replaced, its:

- MAC address can change
- IP configuration can change
- Network connectivity configuration may need to be updated

In AWS, network interfaces are virtual resources, and their behavior is managed differently from physical hardware.

An EC2 public IP is **not simply the MAC address of the NIC**.

This distinction is important.

---

# 58. How a Browser Reaches a Web Server

Suppose you type:

```text
https://example.com
```

Simplified process:

```text
                 DNS
                  │
Browser ──────────┤
                  ▼
             Server IP
                  │
                  ▼
              Routing
                  │
                  ▼
              Internet
                  │
                  ▼
           Server :443
                  │
                  ▼
             HTTPS/TLS
                  │
                  ▼
              Response
                  │
                  ▼
              Browser
```

This single process involves many networking concepts:

```text
DNS
IP
Routing
TCP/QUIC
TLS
Port
Firewall
Web Server
HTTP/HTTPS
```

---

# 59. Interview Questions

## Q1. What is networking?

**Answer:**

> Networking is the process of connecting two or more devices so they can communicate and share data or resources.

---

## Q2. What is an IP address?

> An IP address is a logical address assigned to a network interface so devices can communicate over an IP network.

---

## Q3. What is a MAC address?

> A MAC address is an address associated with a network interface and is primarily used for communication at the Data Link layer.

---

## Q4. What is a NIC?

> NIC stands for Network Interface Card. It provides a device with a network connection, such as Ethernet or Wi-Fi. In cloud environments, virtual network interfaces are used.

---

## Q5. What is DNS?

> DNS stands for Domain Name System. It translates domain names such as google.com into IP addresses that systems can use to communicate.

---

## Q6. What is DHCP?

> DHCP automatically provides network configuration such as IP address, subnet mask, default gateway and DNS server to clients.

---

## Q7. What is a subnet?

> A subnet is a logical subdivision of an IP network into smaller networks.

---

## Q8. What is CIDR?

> CIDR stands for Classless Inter-Domain Routing. It represents an IP network using an address and prefix length, such as 192.168.1.0/24.

---

## Q9. What is the difference between TCP and UDP?

> TCP is connection-oriented and provides reliable, ordered delivery. UDP is connectionless, has lower overhead, and does not provide TCP's reliability and ordering guarantees.

---

## Q10. What is the TCP three-way handshake?

> The TCP three-way handshake establishes a TCP connection using SYN, SYN-ACK and ACK.

```text
SYN
 ↓
SYN-ACK
 ↓
ACK
```

---

## Q11. What is a port?

> A port identifies a service or application endpoint on a host. TCP and UDP use port numbers from 0 through 65535.

---

## Q12. What is the default SSH port?

> TCP port 22.

---

## Q13. What is the default HTTP port?

> TCP port 80.

---

## Q14. What is the default HTTPS port?

> TCP port 443.

---

## Q15. How do you check the IP address in Linux?

```bash
ip addr
```

or:

```bash
ip -br addr
```

---

## Q16. How do you check the routing table?

```bash
ip route
```

---

## Q17. How do you check listening ports?

```bash
ss -tulpn
```

---

## Q18. How do you test DNS?

```bash
nslookup google.com
```

or:

```bash
dig google.com
```

---

## Q19. How do you test Internet connectivity?

```bash
ping -c 4 8.8.8.8
```

Then test DNS resolution:

```bash
ping -c 4 google.com
```

---

## Q20. What is the difference between `ip addr` and `ip route`?

> `ip addr` shows network interfaces and their IP addresses, while `ip route` shows the system's routing table and where traffic should be forwarded.

---

## Q21. What is ARP?

> ARP, or Address Resolution Protocol, is used in IPv4 local networks to resolve an IP address to a MAC address.

---

## Q22. What is the difference between a switch and a router?

> A switch primarily connects devices within a LAN and forwards Ethernet frames using MAC addresses. A router connects different IP networks and forwards packets using routing information.

---

## Q23. What is NAT?

> NAT stands for Network Address Translation. It translates IP addresses between different network address spaces, commonly allowing private addresses to communicate with external networks.

---

## Q24. What is a firewall?

> A firewall controls network traffic according to defined rules and can allow or deny connections based on factors such as source, destination, protocol and port.

---

## Q25. Why can an EC2 website be inaccessible even when Apache is running?

Possible reasons include:

```text
Apache not listening on the expected port
AWS Security Group blocking traffic
Linux firewall blocking traffic
Wrong public IP
Wrong routing/network configuration
Application configuration problem
Port mismatch
```

---

# 60. ⭐ Most Important Interview Troubleshooting Framework

When a website is inaccessible, think in this order:

```text
1. Is the server running?
          ↓
2. Is the network interface configured?
          ↓
3. Is the route correct?
          ↓
4. Is the service running?
          ↓
5. Is the port listening?
          ↓
6. Does localhost work?
          ↓
7. Is Linux firewall allowing traffic?
          ↓
8. Is AWS Security Group allowing traffic?
          ↓
9. Is the public IP/DNS correct?
          ↓
10. Is DNS resolving correctly?
```

Useful commands:

```bash
ip addr
ip route
sudo systemctl status httpd
ss -tulpn
curl http://localhost
ip neigh
ping
nslookup
dig
traceroute
sudo firewall-cmd --list-all
```

This troubleshooting mindset is **more valuable in a DevOps interview than simply memorizing commands**.

---

# 61. Day 10 Quick Revision

### Networking

```text
Networking → Communication between devices
```

### NIC

```text
Network connection/interface
```

### IP

```text
Logical network address
```

### MAC

```text
Network interface address at Layer 2
```

### Switch

```text
Connects devices in LAN
```

### Router

```text
Connects different networks
```

### Gateway

```text
Exit point toward another network
```

### DNS

```text
Domain → IP
```

### DHCP

```text
Automatically provides IP configuration
```

### ARP

```text
IPv4 IP → MAC on local network
```

### TCP

```text
Reliable + connection-oriented
```

### UDP

```text
Connectionless + lower overhead
```

### HTTP

```text
80
```

### HTTPS

```text
443
```

### SSH

```text
22
```

### MySQL

```text
3306
```

### PostgreSQL

```text
5432
```

---

# 62. 🎯 Day 10 Practical Challenge

Without looking at the notes, log in to your Linux/EC2 machine and answer these:

### Task 1

Find your:

```text
IPv4 address
Network interface
MAC address
Default gateway
```

Commands:

```bash
ip addr
ip link
ip route
```

### Task 2

Test:

```bash
ping -c 4 8.8.8.8
ping -c 4 google.com
```

Explain the difference.

### Task 3

Find Google's IP:

```bash
nslookup google.com
```

or:

```bash
dig google.com
```

### Task 4

Check listening services:

```bash
ss -tulpn
```

Identify:

```text
Port
Protocol
Process
```

### Task 5

Test HTTPS:

```bash
nc -zv google.com 443
```

### Task 6

Test a website:

```bash
curl -I https://example.com
```

Explain the HTTP status code.

### Task 7

If Apache is installed:

```bash
sudo systemctl status httpd
```

```bash
curl http://localhost
```

```bash
ss -tulpn | grep :80
```

Explain what each command proves.

---

# 🧠 Final Day 10 Interview Goal

By the end of Day 10, you should be able to explain this diagram without memorizing a script:

```text
                    INTERNET
                        │
                        ▼
                  Public Network
                        │
                        ▼
                 AWS / Router
                        │
                  Routing / NAT
                        │
                        ▼
                 Security Group
                        │
                        ▼
                   EC2 ENI
                        │
                        ▼
                 Linux Firewall
                        │
                        ▼
                  TCP Port 80
                        │
                        ▼
                    Apache
                        │
                        ▼
                  Web Application
```

And when something breaks, you should know **which layer to investigate first**.

---

# 📌 Day 10 Must-Remember Commands

```bash
ip addr
ip -br addr
ip link
ip route
ip neigh

ping -c 4 8.8.8.8
ping -c 4 google.com

nslookup google.com
dig google.com
host google.com

ss -tuln
ss -tulpn

traceroute google.com
tracepath google.com

curl https://example.com
curl -I https://example.com
curl -L https://example.com

nc -zv google.com 443

sudo tcpdump -D
sudo tcpdump -i ens5

sudo lsof -i :80
```

---

# 🏆 Day 10 Summary

You learned the foundation of **Linux and DevOps networking**:

```text
Networking
    ↓
IP Addressing
    ↓
IPv4 / IPv6
    ↓
Private / Public IP
    ↓
Subnet / CIDR
    ↓
MAC / ARP
    ↓
Switch / Router / Gateway
    ↓
DNS / DHCP
    ↓
TCP / UDP
    ↓
Ports
    ↓
OSI Model
    ↓
Linux Networking Commands
    ↓
Firewall
    ↓
AWS Networking
    ↓
Troubleshooting
```

### ⭐ Priority for interviews

**Master these first:**

1. IP address
2. Private vs Public IP
3. Subnet + CIDR
4. DNS
5. DHCP
6. MAC + ARP
7. TCP vs UDP
8. TCP 3-way handshake
9. Ports
10. Routing
11. Firewall
12. `ip addr`
13. `ip route`
14. `ss`
15. `ping`
16. `nslookup` / `dig`
17. `curl`
18. AWS Security Groups
19. Network troubleshooting
20. AWS VPC fundamentals — **next-level topic**