🌐 IP Addressing, Subnetting, Subnet Mask & CIDR

📌 Why I Learned This Properly

While working in DevOps and learning networking, I had many doubts:

  - What exactly is IP address?

  - What is network and host portion?

  - Why subnet mask?

  - Why CIDR?

  - Why do we subtract 256 - 192?

  - Why first IP is network and last IP is broadcast?

Initially, everything felt disconnected.

So I decided to understand it in the correct sequence:

IP → Binary → Network/Host → Subnet Mask → CIDR → Host Bits → Block Size → Range Calculation → Real-world usage

This document explains everything clearly in that order.


1️⃣ What is an IP Address?

An IP address is a logical address assigned to a device in a network.

Just like:

    - Every house has a house number

    - Every phone has a phone number

    - Every server has an IP address

Example:

192.168.1.10

IPv4 characteristics:

    - It has 4 octets

    - Each octet ranges from 0 to 255

    - Total size = 32 bits

Total possible IPv4 addresses:

2^32 = 4,294,967,296


Binary Structure of IPv4

 Each octet contains 8 bits.

 8 + 8 + 8 + 8 = 32 bits total.

Example:

192 in binary = 11000000
168 in binary = 10101000

Internally, networking works in binary, not decimal.


2️⃣ Network Portion and Host Portion

Every IP address has two parts:

Network portion

Host portion

Network portion identifies the network.
Host portion identifies the device inside that network.

Example:

192.168.1.10/24

Here:

/24 means first 24 bits are network

Remaining 8 bits are host

So:

Network = 192.168.1

Host = 10

All devices in same network share same network portion.

3️⃣ What is Subnet Mask?

Subnet mask tells us:

   Which bits are network
   Which bits are host

Example:

255.255.255.0

Binary:

11111111.11111111.11111111.00000000

Rule:

   - 1 = Network bit

   - 0 = Host bit

So in this case:

  - First 24 bits → Network

  - Last 8 bits → Host

Subnet mask defines boundary between network and host.



4️⃣ What is CIDR?

CIDR stands for Classless Inter-Domain Routing.

It is shorthand representation of subnet mask.

Instead of writing:

255.255.255.0

We write:

/24

Why?

Because there are 24 ones in binary subnet mask.

Examples:

   - 255.255.255.0 → /24

   - 255.255.255.192 → /26

   - 255.255.0.0 → /16

CIDR simply tells number of network bits.

Subnet mask and CIDR represent the same concept.


5️⃣ Understanding /26 Step-by-Step

Let’s take subnet mask:

255.255.255.192

Convert 192 to binary:

192 = 11000000

So full mask becomes:

11111111.11111111.11111111.11000000

Count total number of 1s:

8 + 8 + 8 + 2 = 26

So CIDR = /26

Now calculate host bits:

Total bits = 32
Host bits = 32 - 26 = 6

Total IPs in this subnet:

2^6 = 64 IP addresses


6️⃣ Why Do We Subtract 256 - 192?

This was one of my major doubts.

Formula:

Block Size = 256 - Subnet Value

If subnet mask last octet is 192:

256 - 192 = 64

That means:

Each subnet increases in steps of 64.

So subnets will start at:

   192.168.1.0
   192.168.1.64
   192.168.1.128
   192.168.1.192

Each block contains 64 IP addresses.

7️⃣ Network ID, Broadcast, and Usable Range

Let’s take:

192.168.1.128/26

Block size = 64

Network ID:

192.168.1.128

Broadcast address:

128 + 63 = 191
So broadcast = 192.168.1.191

Usable host range:

192.168.1.129
to
192.168.1.190

Total IPs = 64
Usable IPs = 62

Why 62?

First IP (all host bits 0) = Network ID

Last IP (all host bits 1) = Broadcast

Remaining = usable

Visual Understanding of /26

8️⃣ Why Subnetting is Required

    - If we do not divide networks:

    - Too many devices in one broadcast domain

    - Network congestion increases

    - Security becomes weak

    - Hard to manage IP allocation

Subnetting allows:

    - Logical segmentation

    - Performance improvement

    - Security isolation

    - Better IP management

9️⃣ Real-Time DevOps and Cloud Example

In AWS:

When creating a VPC:

10.0.0.0/16

This means:

     - Network bits = 16

     - Host bits = 16

     - Total IPs = 2^16 = 65,536

Then we create subnets:

     - 10.0.1.0/24 → Web servers

     - 10.0.2.0/24 → Application servers

     - 10.0.3.0/24 → Database servers

This separation improves:

    - Security group control
   
    - Route table management

    - Isolation between layers

    - Better architecture design

In Docker:

Bridge network may use 172.17.0.0/16

Understanding CIDR helps in:

   - Port exposure

   - Network troubleshooting

   - Container communication

🔟 Important Points Summary

   - IPv4 is 32-bit logical address.

   - IP address has network and host portions.

   - Subnet mask defines boundary between network and host.

   - CIDR is shorthand notation of subnet mask.

   - Host bits = 32 - CIDR.

   - Total IPs = 2^(host bits).

   - Block size = 256 - subnet value.

   - First IP = Network ID.

   - Last IP = Broadcast address.

   - Remaining IPs = Usable hosts.

🎯 Final Understanding

Earlier, I was confused because I learned topics randomly.

But once I understood in this order:

IP structure
Binary
Network vs Host
Subnet Mask
CIDR
Host bits calculation
Block size
Range calculation
Real-world implementation

Everything became logically connected.



