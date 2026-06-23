# Chapter 5 Quiz — IPv4 Addressing and Subnetting

> Exam-style questions. Answers are marked and explained. Cover the explanations and test yourself first — work the subnetting math on paper before peeking.

## Question 1: Hosts on a /26
What is the usable number of host IP addresses on a network that has a /26 mask?

- [ ] 64
- [x] 62
- [ ] 32
- [ ] 30
- [ ] 254

### Explanation
* **Key point:** /26 leaves 32 − 26 = 6 host bits. Total addresses = 2⁶ = 64; subtract 2 (network + broadcast) → **62 usable hosts**.

---

## Question 2: Prefix length of a mask
What is the prefix length notation for the subnet mask 255.255.255.224?

- [ ] /25
- [ ] /26
- [x] /27
- [ ] /28

### Explanation
* **Key point:** 224 = `11100000` = 3 bits. So 8 + 8 + 8 + 3 = **27** network bits → /27.
* **Why not the others:** /26 = .192, /28 = .240, /25 = .128.

---

## Question 3: Which subnet includes the host
Which subnet would include the address 192.168.1.96 as a usable host address?

- [x] 192.168.1.64/26
- [ ] 192.168.1.64/29
- [ ] 192.168.1.32/27
- [ ] 192.168.1.32/28

### Explanation
* **Key point:** /26 block size = 64, so subnet 192.168.1.64 runs .65–.126 (broadcast .127). `.96` falls inside → usable.
* **Why not the others:** /29 (block 8) on .64 only reaches .71; /27 on .32 ends at .63; /28 on .32 ends at .47. `.96` is outside all three.

---

## Question 4: Network and broadcast for a /26 host
Refer to host 192.168.10.45 with a mask of 255.255.255.192. What are the network and broadcast addresses? (Choose two.)

- [x] Network: 192.168.10.0
- [ ] Network: 192.168.10.32
- [ ] Broadcast: 192.168.10.127
- [x] Broadcast: 192.168.10.63
- [ ] Broadcast: 192.168.10.64

### Explanation
* **Key point:** /26 block size = 64 → boundaries .0, .64, .128, .192. `.45` is in the first block (.0). Network = **.0**, broadcast = one below .64 = **.63**, host range .1–.62.

---

## Question 5: Hosts needed → mask
A network administrator needs to create a subnet that supports at least 14 usable host addresses, with the least waste. Which subnet mask should be used?

- [ ] 255.255.255.192
- [ ] 255.255.255.224
- [x] 255.255.255.240
- [ ] 255.255.255.248

### Explanation
* **Key point:** Need 14 hosts. /28 gives 2⁴ − 2 = 14 — exact fit. Mask = **255.255.255.240**.
* **Why not the others:** /29 (.248) gives only 6; /27 (.224) gives 30 (wasteful); /26 (.192) gives 62 (more wasteful).

---

## Question 6: Number of subnets from borrowing
A /24 network is subnetted by borrowing 3 bits. How many subnets are created and how many usable hosts per subnet? (Choose two.)

- [x] 8 subnets
- [ ] 6 subnets
- [ ] 32 usable hosts
- [x] 30 usable hosts
- [ ] 62 usable hosts

### Explanation
* **Key point:** Subnets = 2³ = **8**. Remaining host bits = 8 − 3 = 5 → 2⁵ − 2 = **30** usable hosts. The new mask is /27.

---

## Question 7: Point-to-point link mask
Which subnet mask is most appropriate for a serial point-to-point link between two routers, wasting the fewest addresses?

- [ ] 255.255.255.240
- [ ] 255.255.255.248
- [x] 255.255.255.252
- [ ] 255.255.255.255

### Explanation
* **Key point:** A router-to-router link needs exactly 2 host addresses. /30 (255.255.255.252) provides 2² − 2 = **2 usable hosts** — perfect, no waste.

---

## Question 8: Identify the private address
Which of the following is a private IPv4 address as defined by RFC 1918?

- [ ] 172.32.1.1
- [x] 172.16.5.10
- [ ] 192.169.1.1
- [ ] 11.0.0.1

### Explanation
* **Key point:** The 172 private block is **172.16.0.0 – 172.31.255.255**. `172.16.5.10` is inside it.
* **Why not the others:** `172.32.x` is just past the block; `192.169.x` is not `192.168.x`; `11.x` is outside the 10/8 block.

---

## Question 9: Matching address types
Match each description with an appropriate IPv4 address.

| Description | IP Address |
| :--- | :--- |
| an experimental (class E) address | **240.2.6.255** |
| a private address | **172.19.20.5** |
| a TEST-NET documentation address | **192.0.2.123** |
| a link-local (APIPA) address | **169.254.1.5** |
| a loopback address | **127.0.0.1** |

### Explanation
* **Key point:** Recognise by leading values: 240–255 = experimental (class E); 172.16–172.31 = private; 192.0.2.0/24 = TEST-NET; 169.254.0.0/16 = link-local; 127.0.0.0/8 = loopback.

---

## Question 10: Multicast target
A host is transmitting to 224.0.0.5. Which host or hosts will receive it?

- [ ] all hosts on the Internet
- [ ] one specific host
- [x] a specifically defined group of hosts
- [ ] all hosts with the same IP address

### Explanation
* **Key point:** 224.0.0.0 – 239.255.255.255 is the **multicast** range; multicast delivers to a defined group of subscribers, not everyone and not a single host.

---

## Question 11: Classful identification
A host has the address 190.12.5.40. To which legacy class does this address belong?

- [ ] Class A
- [x] Class B
- [ ] Class C
- [ ] Class D

### Explanation
* **Key point:** First octet 128–191 = **Class B**. (1–126 A, 192–223 C, 224–239 D, 240–255 E.)

---

## Question 12: Which subnet for /27 host
Which subnet (network address) does the host 10.1.1.200/27 belong to, and what is its valid host range? (Choose the matching pair.)

- [ ] Network 10.1.1.160, range .161–.190
- [x] Network 10.1.1.192, range .193–.222
- [ ] Network 10.1.1.200, range .201–.222
- [ ] Network 10.1.1.224, range .225–.254

### Explanation
* **Key point:** /27 block size = 32 → boundaries .0,.32,...,.192,.224. `.200` is in the .192 block. Network = **.192**, broadcast = .223, usable range **.193–.222**.

---

## Question 13: Loopback purpose
What is the purpose of the IPv4 address 127.0.0.1?

- [ ] It is the default gateway for the local network.
- [x] It allows a host to direct traffic to itself for testing.
- [ ] It is a public address assigned by an ISP.
- [ ] It is a multicast address for all routers.

### Explanation
* **Key point:** 127.0.0.0/8 is the **loopback** range; `127.0.0.1` lets TCP/IP apps on the same device communicate and lets you test the local stack.

---

## Question 14: APIPA / link-local symptom
A user's PC is configured for DHCP but displays an IP address of 169.254.45.12. What does this most likely indicate?

- [ ] The PC has been manually assigned a public address.
- [x] The PC could not reach a DHCP server and self-assigned a link-local address.
- [ ] The PC is using a loopback address.
- [ ] The PC is part of a multicast group.

### Explanation
* **Key point:** 169.254.0.0/16 is **link-local (APIPA)**, auto-assigned when no DHCP server responds — a classic "DHCP failed" symptom.

---

## Question 15: ANDing result
A host has IP 192.168.5.130 and mask 255.255.255.128. What is the network address (the result of ANDing)?

- [ ] 192.168.5.0
- [x] 192.168.5.128
- [ ] 192.168.5.129
- [ ] 192.168.5.255

### Explanation
* **Key point:** /25 block size = 256 − 128 = 128 → boundaries .0 and .128. `.130` is in the .128 block → network = **192.168.5.128**, broadcast .255, range .129–.254.

---

## Question 16: Why subnet
Which two reasons describe why an administrator divides a large network into subnets? (Choose two.)

- [x] to reduce the size of broadcast domains
- [x] to improve network performance and manageability
- [ ] to eliminate the need for a router between segments
- [ ] to increase the number of usable hosts in the whole address space
- [ ] to avoid having to use a subnet mask

### Explanation
* **Key point:** Subnetting shrinks **broadcast domains**, cutting broadcast traffic and improving performance/security.
* **Why not the others:** Subnetting actually *requires* a router to move traffic between subnets, and it slightly *reduces* total usable hosts (more network/broadcast pairs).

---

## Question 17: VLSM ordering
When designing a VLSM addressing scheme from a single block, in what order should subnets be assigned?

- [ ] smallest host requirement first
- [x] largest host requirement first
- [ ] point-to-point links first
- [ ] in alphabetical order of the LAN names

### Explanation
* **Key point:** VLSM assigns the **largest** subnet first so the bigger blocks land on clean boundaries and the remaining space packs efficiently without overlap.

---

## Question 18: VLSM allocation
You must subnet 192.168.20.0/24 with VLSM for: LAN A (50 hosts), LAN B (25 hosts), then two /30 WAN links. Starting at .0, which network is assigned to LAN B?

- [ ] 192.168.20.0/27
- [x] 192.168.20.64/27
- [ ] 192.168.20.50/27
- [ ] 192.168.20.96/27

### Explanation
* **Key point:** LAN A (50 hosts) needs /26 → takes .0–.63. LAN B (25 hosts) needs /27 (30 hosts), starting at the next boundary → **192.168.20.64/27** (range .65–.94).

---

## Question 19: Broadcast address
What is the broadcast address of the subnet 172.16.8.0/22?

- [ ] 172.16.8.255
- [ ] 172.16.10.255
- [x] 172.16.11.255
- [ ] 172.16.15.255

### Explanation
* **Key point:** /22 → interesting octet is the 3rd, mask 252, block size = 256 − 252 = 4. Subnet 172.16.8.0 spans third-octet .8–.11. Broadcast = last address = **172.16.11.255**.

---

## Question 20: Number of bits to borrow
A company has the network 192.168.100.0/24 and needs at least 10 subnets. What is the minimum number of bits to borrow and the resulting mask? (Choose two.)

- [ ] borrow 3 bits
- [x] borrow 4 bits
- [ ] mask 255.255.255.224
- [x] mask 255.255.255.240

### Explanation
* **Key point:** Need ≥ 10 subnets. 2³ = 8 (too few), **2⁴ = 16** ✅ → borrow 4 bits → /28 → mask **255.255.255.240**.

---

## Question 21: Subnet mask identifies portions
When IPv4 addressing is manually configured on a server, which property identifies the network and host portions of the address?

- [ ] default gateway
- [x] subnet mask
- [ ] DNS server address
- [ ] MAC address

### Explanation
* **Key point:** The **subnet mask** (its 1s = network, 0s = host) is what divides the address into network and host portions.

---

## Question 22: Unusable host trap
Which address is NOT a usable host address in the subnet 192.168.1.32/27?

- [ ] 192.168.1.33
- [ ] 192.168.1.50
- [x] 192.168.1.63
- [ ] 192.168.1.62

### Explanation
* **Key point:** /27 block size 32 → subnet .32 spans .32–.63. `.63` is the **broadcast** (host bits all 1s) → not assignable. `.33`–`.62` are usable; `.62` is the last host.

---

## Question 23: Increment / magic number
A network uses the mask 255.255.255.248. What is the block size (increment) between consecutive subnets?

- [ ] 4
- [x] 8
- [ ] 16
- [ ] 32

### Explanation
* **Key point:** Block size = 256 − interesting-octet mask value = 256 − 248 = **8**. (This is /29, giving 6 usable hosts per subnet.)

---

## Question 24: Default gateway role
Two PCs are on different subnets and cannot reach each other. What device is required, and how does each PC reach it?

- [ ] a switch; PCs reach it by broadcast
- [x] a router; each PC uses the router interface on its LAN as its default gateway
- [ ] a hub; PCs reach it by multicast
- [ ] no device; subnets communicate directly

### Explanation
* **Key point:** Traffic cannot move between subnets without a **router**. Each device sends off-subnet traffic to its **default gateway** — the router interface attached to its own LAN, which must have an address within that subnet.

---

Great work. If any subnetting item took more than ~30 seconds, redo it with the block-size method until it's automatic — that speed is exactly what the exam rewards.
