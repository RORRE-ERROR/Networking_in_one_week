# CCNA Cisco Networking Academy - Quiz Compilation

This document contains a compilation of the networking quiz questions, their correct answers, and concise explanations from this session.

---

### Question 1
**What is the usable number of host IP addresses on a network that has a /26 mask?**

- [ ] 32
- [ ] 64
- [x] 62
- [ ] 16
- [ ] 254
- [ ] 256

**Explanation:** A `/26` mask leaves 6 bits for hosts ($32 - 26 = 6$). The total number of addresses is $2^6 = 64$. Subtracting 2 for the network and broadcast addresses gives **62 usable host addresses**.

---

### Question 12
**Which service provides dynamic global IPv6 addressing to end devices without using a server that keeps a record of available IPv6 addresses?**

- [ ] static IPv6 addressing
- [x] SLAAC
- [ ] stateless DHCPv6
- [ ] stateful DHCPv6

**Explanation:** **SLAAC** (Stateless Address Autoconfiguration) allows end devices to configure their own global unicast IPv6 addresses using Router Advertisement messages without a centralized server maintaining state records.

---

### Question 13
**Which protocol supports Stateless Address Autoconfiguration (SLAAC) for dynamic assignment of IPv6 addresses to a host?**

- [x] ICMPv6
- [ ] ARPv6
- [ ] DHCPv6
- [ ] UDP

**Explanation:** SLAAC relies on **ICMPv6** Neighbor Discovery Protocol (NDP) messages, specifically Router Solicitations (RS) and Router Advertisements (RA).

---

### Question 14
**Match the IPv6 address with the IPv6 address type.**

| IPv6 Address | Address Type |
| :--- | :--- |
| `FF02::1` | **all node multicast** |
| `2001:DB8::BAF:3F57:FE94` | **global unicast** |
| `FF02::1:FFAE:F85F` | **solicited node multicast** |
| `::1` | **loopback** |

---

### Question 15
**Three methods allow IPv6 and IPv4 to co-exist. Match each method with its description.**

| Description | Method |
| :--- | :--- |
| IPv6 packets are converted into IPv4 packets, and vice versa. | **translation** |
| The IPv6 packet is transported inside an IPv4 packet. | **tunneling** |
| The IPv4 packets and IPv6 packets coexist in the same network. | **dual-stack** |

---

### Question 16
**What type of IPv6 address is FE80::1?**

- [x] link-local
- [ ] multicast
- [ ] loopback
- [ ] global unicast

**Explanation:** Any IPv6 address in the range of `FE80::/10` is classified as a **link-local** address, used for communication on a single local network segment.

---

### Question 17
**Given IPv6 address prefix 2001:db8::/48, what will be the last subnet that is created if the subnet prefix is changed to /52?**

- [x] `2001:db8:0:f000::/52`
- [ ] `2001:db8:0:f00::/52`
- [ ] `2001:db8:0:f::/52`
- [ ] `2001:db8:0:8000::/52`

**Explanation:** Changing from `/48` to `/52` means borrowing 4 bits. This modifies the first hexadecimal character of the 4th hextet. The maximum binary value for 4 bits is `1111` (`f` in hex), making the 4th hextet `f000`.

---

### Question 18
**Which IPv6 prefix is reserved for communication between devices on the same link?**

- [x] `FE80::/10`
- [ ] `FDFF::/7`
- [ ] `2001::/32`
- [ ] `FC00::/7`

**Explanation:** `FE80::/10` is explicitly reserved for Link-Local communication.

---

### Question 19
**A network administrator has received the IPv6 prefix 2001:DB8::/48 for subnetting. Assuming the administrator does not subnet into the interface ID portion of the address space, how many subnets can the administrator create from the /48 prefix?**

- [ ] 16
- [x] 65536
- [ ] 256
- [ ] 4096

**Explanation:** The interface ID boundary starts at `/64`. The bits available for subnetting are $64 - 48 = 16	ext{ bits}$. Thus, $2^{16} = 65,536$ possible subnets.

---

### Question 20
**An IPv6 enabled device sends a data packet with the destination address of FF02::1. What is the target of this packet?**

- [ ] only IPv6 DHCP servers
- [x] all IPv6 enabled devices on the local link or network
- [ ] only IPv6 configured routers
- [ ] the one IPv6 device on the link that has been uniquely configured with this address

**Explanation:** `FF02::1` is the all-nodes link-local multicast address, reaching all IPv6-enabled nodes on that link.

---

### Question 21
**Refer to the exhibit. (Address: 2001:DB8:1234:0000::/64 with 1 hex char for Site, 2 hex chars for Sub-site, and 1 hex char for Subnet). What is the maximum number of subnets achieved per sub-site?**

- [x] 16
- [ ] 4
- [ ] 256
- [ ] 0

**Explanation:** Since the "Subnet" field spans exactly 1 hexadecimal character, it consists of 4 bits. $2^4 = 16$ possible subnets.

---

### Question 22
**Which address is a valid IPv6 link-local unicast address?**

- [x] `FE80::1:4545:6578:ABC1`
- [ ] `FC90:5678:4251:FFFF`
- [ ] `FEC8:1::FFFF`
- [ ] `FE0A::100:7788:998F`
- [ ] `FD80::1:1234`

**Explanation:** Link-local unicast addresses must start with the prefix range `FE80` to `FEBF` (conforming to `FE80::/10`).

---

### Question 23
**What is used in the EUI-64 process to create an IPv6 interface ID on an IPv6 enabled interface?**

- [x] the MAC address of the IPv6 enabled interface
- [ ] an IPv6 address that is provided by a DHCPv6 server
- [ ] an IPv4 address that is configured on the interface
- [ ] a randomly generated 64-bit hexadecimal address

**Explanation:** The EUI-64 process expands a 48-bit physical **MAC address** into a unique 64-bit Interface ID by inserting `FFFE` in the middle and flipping the 7th bit.

---

### Question 24
**Refer to the exhibit. An administrator is trying to troubleshoot connectivity between PC1 and PC2 and uses the tracert command from PC1 to do it. Based on the displayed output, where should the administrator begin troubleshooting?**

- [ ] SW1
- [ ] SW2
- [x] R1
- [ ] R2
- [ ] PC2

**Explanation:** The first hop (R1, `192.168.1.254`) responded successfully, but subsequent requests timed out. Troubleshooting always begins at the last known responsive hop (**R1**).

---

### Question 25
**Which protocol is used by the traceroute command to send and receive echo-requests and echo-replies?**

- [x] ICMP
- [ ] TCP
- [ ] Telnet
- [ ] SNMP

**Explanation:** The `ping` and `traceroute` commands rely on **ICMP** (Internet Control Message Protocol) to send echo messages and decode status messages like time-exceeded or unreachable errors.

---

### Question 26
**A user executes a traceroute over IPv6. At what point would a router in the path to the destination device drop the packet?**

- [ ] when the router receives an ICMP time exceeded message
- [x] when the value of the Hop Limit field reaches zero
- [ ] when the target host responds with an ICMP echo reply message
- [ ] when the value of the Hop Limit field reaches 255

**Explanation:** When a router receives a packet and decrements its **Hop Limit** field down to zero, it drops the packet and responds with an ICMPv6 Time Exceeded error packet.

---

### Question 27
**What is the prefix length notation for the subnet mask 255.255.255.224?**

- [x] /27
- [ ] /28
- [ ] /25
- [ ] /26

**Explanation:** Converting `255.255.255.224` to binary yields $8 + 8 + 8 + 3 = 27$ contiguous network bits, written as **/27**.

---

### Question 28
**Match each description with an appropriate IP address.**

| Description | IP Address |
| :--- | :--- |
| an experimental address | **240.2.6.255** |
| a private address | **172.19.20.5** |
| a TEST-NET address | **192.0.2.123** |
| a link-local address | **169.254.1.5** |
| a loopback address | **127.0.0.1** |

---

### Question 29
**Which subnet would include the address 192.168.1.96 as a usable host address?**

- [x] `192.168.1.64/26`
- [ ] `192.168.1.64/29`
- [ ] `192.168.1.32/27`
- [ ] `192.168.1.32/28`

**Explanation:** For `192.168.1.64/26`, the valid host range runs from `.65` to `.126`. Therefore, `.96` falls within its usable spectrum. In other specific subnet options, `.96` would act as an unusable network identity bound.

---

### Question 31
**A host is transmitting a multicast. Which host or hosts will receive it?**

- [ ] all hosts on the Internet
- [ ] all hosts with the same IP address
- [x] a specially defined group of hosts
- [ ] one specific host

**Explanation:** **Multicast** routes traffic selectively to a distinct, pre-defined group of subscription listeners.

---

### Question 32
**Which is the compressed format of the IPv6 address fe80:0000:0000:0000:0220:0b3f:f0e0:0029?**

- [ ] `fe80:9ea0::2020::bf:e0:9290`
- [ ] `fe80:9ea0::2020:0:bf:e0:9290`
- [ ] `fe80:9ea0:0:2200::fe0:290`
- [x] `fe80::220:b3f:f0e0:29`

**Explanation:** Stripping leading zeros changes `0220` $ightarrow$ `220` and `0029` $ightarrow$ `29`. Replacing consecutive zeros yields the correct form: `fe80::220:b3f:f0e0:29`.

---

### Question 33
**A user issues a ping 192.135.250.103 command and receives a response that includes a code of 1. What does this code represent?**

- [ ] beyond scope of the source address
- [x] host unreachable
- [ ] address unreachable
- [ ] communication with the destination administratively prohibited

**Explanation:** In an ICMP Type 3 (Destination Unreachable) frame, a **Code 1** denotes that the destination **host is unreachable**.