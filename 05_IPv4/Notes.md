# Chapter 5 — IPv4 Addressing and Subnetting

> **Day 3 of 7** · Estimated study time: ~4 hours
> *This chapter teaches you how to read an IPv4 address, work out which network it belongs to, and slice one big network into many smaller ones (subnetting). Subnetting is the single most-tested skill on the exam — slow down and work the examples by hand.*

## 🎯 What you'll be able to do after this chapter
- Read a 32-bit IPv4 address in dotted-decimal and binary, and explain the network vs host portions.
- Use a subnet mask (and ANDing) to find the network address of any host.
- Identify the network address, broadcast address, and usable host range of any subnet.
- Tell apart unicast, multicast, and broadcast addresses, and recognise private, public, loopback, and link-local ranges.
- Recall the old classful (A/B/C/D/E) boundaries and read CIDR/prefix notation.
- Subnet a network: borrow bits to make *N* subnets, calculate hosts per subnet, and find the increment ("magic number").
- Design a **VLSM** scheme that gives each subnet only as many addresses as it needs, without waste.

---

## 1. What an IPv4 address really is

Intuition first: an IPv4 address is just a **32-bit number** that we write in four chunks so humans can read it. Computers see `11000000.10101000.00000001.00001010`; we write `192.168.1.10`.

- 32 bits, split into **four octets** (8 bits each), written in **dotted-decimal**: `A.B.C.D`.
- Each octet ranges **0–255** (because 8 bits = 2⁸ = 256 values, 0 through 255).
- Every IPv4 address has two parts: a **network portion** (which network you're on) and a **host portion** (which specific device you are on that network).

Analogy: think of `192.168.1.10` like a street address. The network portion is the *street name* (everyone on it is neighbours); the host portion is the *house number* (unique on that street).

### Binary you must be fluent in
The exam assumes you can convert an octet to binary instantly. Memorise the **bit values**:

| Bit position | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|---|---|---|---|---|---|---|---|---|
| Value | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |

To convert `192`: 128 + 64 = 192 → `11000000`. To convert `168`: 128 + 32 + 8 = 168 → `10101000`. Add the values where the bit is `1`.

> 💡 **Exam tip:** Know the powers of two cold: 2,4,8,16,32,64,128,256. Most subnetting questions are solved in seconds once these are automatic.

---

## 2. The subnet mask and ANDing

Intuition: the address alone doesn't tell you where the network ends and the host begins — the **subnet mask** draws that line.

- A **subnet mask** is also 32 bits. The `1`s mark the **network** portion; the `0`s mark the **host** portion. The 1s are always contiguous and on the left.
- Example: `255.255.255.0` = `11111111.11111111.11111111.00000000` → first 24 bits are network, last 8 are host. We write this as **/24** (prefix length = number of 1s).

### Finding the network address with ANDing
The router finds your network address by performing a bitwise **AND** between the IP address and the mask. AND rule: `1 AND 1 = 1`, everything else = `0`.

Worked example — host `192.168.1.10` with mask `255.255.255.0`:

```
IP    192.168.1.10  = 11000000.10101000.00000001.00001010
Mask  255.255.255.0 = 11111111.11111111.11111111.00000000
AND ------------------------------------------------------
Net   192.168.1.0   = 11000000.10101000.00000001.00000000
```

So the host lives on network **192.168.1.0/24**. Wherever the mask is `1`, the IP bit passes through; wherever it's `0`, the result is `0`.

> 💡 **Exam tip:** "Which property identifies the network and host portion of an IPv4 address?" → the **subnet mask**. This exact question appears in the banks.

---

## 3. The three address types inside every network

Every subnet, no matter its size, has exactly these three special concepts:

1. **Network address** — host bits **all 0s**. Names the network itself. *Cannot* be assigned to a device.
2. **Broadcast address** — host bits **all 1s**. The highest address in the range; reaches every host on that network at once. *Cannot* be assigned to a device.
3. **Host addresses** — everything *between* the network and broadcast addresses. These are the usable addresses you give to PCs, routers, printers, etc.

Worked example for `192.168.1.0/24`:

| Role | Address | Host bits |
|---|---|---|
| Network | 192.168.1.0 | 00000000 |
| First usable host | 192.168.1.1 | 00000001 |
| Last usable host | 192.168.1.254 | 11111110 |
| Broadcast | 192.168.1.255 | 11111111 |

That's why the usable-host formula subtracts 2: you lose the network and broadcast addresses.

> 💡 **Exam tip:** Network = all-0 host bits, Broadcast = all-1 host bits. The broadcast is always the address **one below the next network**.

---

## 4. Unicast, multicast, broadcast — and the special ranges

- **Unicast**: one sender → one specific host. Normal host-to-host traffic. IPv4 host range is **0.0.0.0 – 223.255.255.255**.
- **Broadcast**: one sender → all hosts on the local network (host bits all 1s).
- **Multicast**: one sender → a *specific group* of subscribed hosts. Reserved range **224.0.0.0 – 239.255.255.255**.
  - `224.0.0.0 – 224.0.0.255` = reserved **link-local** multicast (e.g. routing protocols).
  - `224.0.1.0 – 238.255.255.255` = globally scoped multicast.

### Reserved / special-purpose ranges to memorise

| Range | Prefix | Purpose |
|---|---|---|
| 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8 | **Private** (RFC 1918) |
| 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12 | **Private** (RFC 1918) |
| 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 | **Private** (RFC 1918) |
| 127.0.0.0 – 127.255.255.255 | 127.0.0.0/8 | **Loopback** (e.g. 127.0.0.1 = "myself") |
| 169.254.0.0 – 169.254.255.255 | 169.254.0.0/16 | **Link-local / APIPA** (auto-assigned when no DHCP) |

- **Private addresses** (RFC 1918) are reused inside private networks; they are *not* routable on the public Internet. NAT translates them to public addresses for Internet access.
- **Public addresses** are the rest of the unicast space and must be globally unique.
- **Loopback** (`127.0.0.1`) lets a host send traffic to itself — a shortcut for apps on the same device. A whole /8 is reserved, but `127.0.0.1` is the one you'll see.
- **Link-local (169.254.x.x)** is auto-assigned by the OS when no DHCP server answers — seeing this on a PC usually means "DHCP failed."

> 💡 **Exam tip:** Banks love a *matching* question: experimental → 240.x (class E), private → 172.19.x, TEST-NET → 192.0.2.x, link-local → 169.254.x, loopback → 127.0.0.1. Learn the leading numbers.

---

## 5. Classful addressing (legacy, but still tested)

Before subnetting flexibility, addresses were grouped into fixed-size classes by their first octet:

| Class | 1st octet range | Default mask | Purpose |
|---|---|---|---|
| A | 1 – 126 | 255.0.0.0 (/8) | Large networks |
| B | 128 – 191 | 255.255.0.0 (/16) | Medium networks |
| C | 192 – 223 | 255.255.255.0 (/24) | Small networks |
| D | 224 – 239 | — | **Multicast** |
| E | 240 – 255 | — | **Experimental** |

Note `127` is skipped (loopback). Modern networks use **classless** addressing (CIDR), so the class no longer dictates the mask — but the exam still asks you to identify a class from the first octet.

> 💡 **Exam tip:** First octet 1–126 = A, 128–191 = B, 192–223 = C, 224–239 = D (multicast), 240–255 = E (experimental).

---

## 6. CIDR / prefix length

**CIDR** (Classless Inter-Domain Routing) writes the mask as a **prefix length** — a slash and the number of network bits. `255.255.255.0` = **/24**, `255.255.255.224` = **/27**.

How to convert a mask to a prefix: count the `1` bits. `255.255.255.224` → `11111111.11111111.11111111.11100000` → 8+8+8+3 = **/27**.

The non-/8/16/24 octet values come from these "magic" mask values:

| Mask octet | Binary | Bits added |
|---|---|---|
| 128 | 10000000 | 1 |
| 192 | 11000000 | 2 |
| 224 | 11100000 | 3 |
| 240 | 11110000 | 4 |
| 248 | 11111000 | 5 |
| 252 | 11111100 | 6 |
| 254 | 11111110 | 7 |
| 255 | 11111111 | 8 |

> 💡 **Exam tip:** "What is the prefix length for 255.255.255.224?" → **/27**. Memorise the eight mask values above and you can convert either direction instantly.

---

## 7. Subnetting — the headline topic

### Why subnet?
Intuition: one giant network = one giant **broadcast domain**. Every broadcast (ARP, etc.) hits *every* host, wasting bandwidth and CPU. Splitting it into smaller **subnets** shrinks each broadcast domain, improving performance, security, and management.

Key facts:
- Devices on **different subnets need a router** to communicate. The router interface on your LAN is your **default gateway**, and every router interface must have an address inside the subnet it connects to.
- We create subnets by **borrowing** host bits and turning them into network bits (lengthening the mask).

### The two formulas (memorise)

```
Number of subnets       = 2 ^ (borrowed bits)
Number of usable hosts  = 2 ^ (remaining host bits) − 2
```

The `− 2` is for the network and broadcast addresses you can't assign.

### The "magic number / block size" trick
The fastest way to find subnet boundaries by hand:

1. Find the **interesting octet** — the octet where the mask isn't 255 or 0.
2. **Block size (increment) = 256 − (mask value in that octet).**
3. Subnets start at 0 and step up by the block size in that octet.

Example: mask `/26` = `255.255.255.192`. Interesting octet = 4th, mask value 192. Block size = 256 − 192 = **64**. Subnets are `.0, .64, .128, .192`.

---

### 📐 Subnet mask reference table (/24 – /30)

| Prefix | Mask | Host bits | Block size (increment) | Total addresses | Usable hosts |
|---|---|---|---|---|---|
| /24 | 255.255.255.0 | 8 | 256 | 256 | 254 |
| /25 | 255.255.255.128 | 7 | 128 | 128 | 126 |
| /26 | 255.255.255.192 | 6 | 64 | 64 | 62 |
| /27 | 255.255.255.224 | 5 | 32 | 32 | 30 |
| /28 | 255.255.255.240 | 4 | 16 | 16 | 14 |
| /29 | 255.255.255.248 | 3 | 8 | 8 | 6 |
| /30 | 255.255.255.252 | 2 | 4 | 4 | 2 |

> 💡 **Exam tip:** /30 gives exactly **2 usable hosts** — perfect for router-to-router (point-to-point) links. /26 = 62 hosts, /27 = 30 hosts. These three numbers (62, 30, 2) appear constantly.

---

### Worked example (a): Given an IP + mask, find network / broadcast / host range

**Given: `192.168.1.100 /26`.**

1. Mask /26 = `255.255.255.192`. Interesting octet = 4th, mask value 192.
2. Block size = 256 − 192 = **64**. Subnet boundaries: .0, .64, .128, .192.
3. `.100` falls between **.64 and .128**, so it lives in subnet **192.168.1.64**.
4. Network address = **192.168.1.64**.
5. Broadcast = one below the next subnet (.128) = **192.168.1.127**.
6. Usable host range = **192.168.1.65 – 192.168.1.126** (62 hosts).

Answer: `.100` is a usable host in subnet **192.168.1.64/26**.

---

### Worked example (b): Borrow bits to create N subnets

**Task: You're given `192.168.10.0/24`. Create at least 6 subnets. How many bits do you borrow, and what's the new mask?**

1. Need ≥ 6 subnets. `2^borrowed ≥ 6` → 2²=4 (too few), **2³ = 8** ✅ → borrow **3 bits**.
2. New prefix = 24 + 3 = **/27**, mask `255.255.255.224`.
3. Remaining host bits = 32 − 27 = 5 → usable hosts = 2⁵ − 2 = **30 per subnet**.
4. Block size = 256 − 224 = **32**. The 8 subnets:

| # | Subnet (network) | First host | Last host | Broadcast |
|---|---|---|---|---|
| 0 | 192.168.10.0 | .1 | .30 | .31 |
| 1 | 192.168.10.32 | .33 | .62 | .63 |
| 2 | 192.168.10.64 | .65 | .94 | .95 |
| 3 | 192.168.10.96 | .97 | .126 | .127 |
| 4 | 192.168.10.128 | .129 | .158 | .159 |
| 5 | 192.168.10.160 | .161 | .190 | .191 |
| 6 | 192.168.10.192 | .193 | .222 | .223 |
| 7 | 192.168.10.224 | .225 | .254 | .255 |

Result: 8 subnets of 30 hosts each, mask /27. (You needed 6; the other 2 are spare.)

---

### Worked example (c): A full VLSM scenario

**VLSM** (Variable Length Subnet Mask) means "subnet a subnet" — give each subnet a *different* mask sized to its real need, so you stop wasting addresses. **Golden rule: always assign the largest subnet first.**

**Task: You have `192.168.20.0/24`. Address these requirements:**
- LAN A: 50 hosts
- LAN B: 25 hosts
- LAN C: 10 hosts
- WAN link 1 (router-to-router): 2 hosts
- WAN link 2 (router-to-router): 2 hosts

Step 1 — Sort largest to smallest: A(50), B(25), C(10), WAN1(2), WAN2(2).

Step 2 — Pick the smallest mask that fits each (using 2^h − 2 ≥ need):

| Subnet | Hosts needed | Bits needed | Mask | Block | Provides |
|---|---|---|---|---|---|
| LAN A | 50 | 6 (2⁶−2=62) | /26 | 64 | 62 |
| LAN B | 25 | 5 (2⁵−2=30) | /27 | 32 | 30 |
| LAN C | 10 | 4 (2⁴−2=14) | /28 | 16 | 14 |
| WAN 1 | 2 | 2 (2²−2=2) | /30 | 4 | 2 |
| WAN 2 | 2 | 2 (2²−2=2) | /30 | 4 | 2 |

Step 3 — Allocate from the start of the block, each new subnet beginning right after the previous one ends:

| Subnet | Network | Mask | Host range | Broadcast |
|---|---|---|---|---|
| LAN A | 192.168.20.0 | /26 | .1 – .62 | .63 |
| LAN B | 192.168.20.64 | /27 | .65 – .94 | .95 |
| LAN C | 192.168.20.96 | /28 | .97 – .110 | .111 |
| WAN 1 | 192.168.20.112 | /30 | .113 – .114 | .115 |
| WAN 2 | 192.168.20.116 | /30 | .117 – .118 | .119 |

Everything from `.120` up is still free for future growth. If you had used a fixed /26 for *all five* subnets, you'd have run out after four and wasted dozens of addresses — that's the whole point of VLSM.

> 💡 **Exam tip:** VLSM questions are graded on order: assign the **biggest subnet first**. Use point-to-point **/30** for every router-to-router serial link.

---

## 🧪 Hands-on (Packet Tracer)
- **11.5.5 Subnet an IPv4 Network** — practice borrowing bits and assigning the resulting subnets to LANs.
- **11.7.5 Subnetting Scenario** — a fixed-length subnetting design from given host requirements.
- **11.9.3 VLSM Design and Implementation Practice** — apply the largest-first VLSM method to a topology.
- **11.10.1 / 11.10.2 Design and Implement a VLSM Addressing Scheme** — full design: derive masks, assign networks, configure interfaces and gateways, verify connectivity.

---

## 🧠 Key terms to memorize

| Term | Meaning in one line |
|---|---|
| Octet | One 8-bit chunk of an IPv4 address (0–255). |
| Subnet mask | 32-bit value whose 1s mark the network portion, 0s the host portion. |
| Prefix length / CIDR | Mask written as a slash + number of network bits (e.g. /26). |
| ANDing | Bitwise AND of IP and mask to find the network address. |
| Network address | Host bits all 0s; names the subnet (not assignable). |
| Broadcast address | Host bits all 1s; reaches all hosts on the subnet (not assignable). |
| Broadcast domain | Set of devices that receive each other's broadcasts; shrunk by subnetting. |
| Default gateway | Router interface a host uses to reach other subnets. |
| RFC 1918 | Defines private ranges 10/8, 172.16/12, 192.168/16. |
| Block size / increment | 256 − interesting-octet mask value; spacing between subnets. |
| Borrowed bits | Host bits turned into network bits to create subnets. |
| VLSM | Using different mask lengths per subnet to avoid wasting addresses. |

---

## ⚡ Exam tips & traps (recap)
- **#subnets = 2^borrowed**, **#usable hosts = 2^hostbits − 2**. The −2 is network + broadcast.
- **Block size = 256 − mask octet.** Subnets step by the block size in the interesting octet.
- **/30 = 2 hosts** (point-to-point links), **/29 = 6**, **/28 = 14**, **/27 = 30**, **/26 = 62**, **/25 = 126**, **/24 = 254**.
- Network address = lowest in range (host bits 0). Broadcast = one less than the next subnet.
- "Which subnet includes host X?" → find block size, step up until X falls inside a block; the block's start is the network, range is start+1 to broadcast−1.
- An address equal to a subnet's network or broadcast value is **NOT** a usable host.
- **Private vs public:** 10/8, 172.16–172.31, 192.168 are private (not Internet-routable). `169.254.x.x` = DHCP failed (APIPA). `127.x` = loopback.
- **VLSM:** assign the **largest** subnet first; never overlap; serial links get /30.
- First-octet class: 1–126 A, 128–191 B, 192–223 C, 224–239 D, 240–255 E (127 = loopback).

---

## ✅ Quick self-check
1. For `172.16.50.10/20`, what is the network address and the broadcast address?
2. You need 14 usable hosts per subnet. What prefix length and mask do you use?
3. Which subnet (network address) does host `10.1.1.200/27` belong to, and what is its valid host range?
4. Why must you assign the largest subnet first when doing VLSM?
5. A PC shows an IP of `169.254.13.5`. What most likely happened?
