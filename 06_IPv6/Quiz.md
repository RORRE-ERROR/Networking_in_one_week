# Chapter 6 Quiz — IPv6 Addressing

> Exam-style questions. Answers are marked and explained. Cover the explanations and test yourself first.

## Question 1: Why IPv6
What is the primary reason the Internet is transitioning from IPv4 to IPv6?

- [ ] IPv6 packets are encrypted by default, which IPv4 cannot do.
- [x] IPv4 public address space has been exhausted and cannot meet demand.
- [ ] IPv6 removes the need for routers on the network.
- [ ] IPv4 does not support multicast traffic.

### Explanation
* **Key point:** IPv4 provides only ~4.3 billion 32-bit addresses, and IANA allocated its last /8 blocks to the RIRs in 2011. The growing Internet of Things needs far more addresses, which the 128-bit IPv6 space provides.
* **Why not the others:** IPv6 is not encrypted by default, still requires routers, and IPv4 does support multicast.

---

## Question 2: Address size
How many bits are in an IPv6 address, and how are they grouped?

- [ ] 64 bits in 4 hextets of 16 bits
- [ ] 32 bits in 4 octets of 8 bits
- [x] 128 bits in 8 hextets of 16 bits
- [ ] 128 bits in 16 octets of 8 bits

### Explanation
* **Key point:** An IPv6 address is **128 bits**, written as **8 hextets** of 16 bits, each hextet shown as 4 hexadecimal digits (32 hex digits total).
* **Why not the others:** 32 bits / 4 octets describes IPv4. IPv6 is grouped into hextets, not octets, for display.

---

## Question 3: IPv6 compression
Which is the compressed format of the IPv6 address `fe80:0000:0000:0000:0220:0b3f:f0e0:0029`?

- [ ] `fe80:9ea0::2020:0:bf:e0:9290`
- [ ] `fe80:9ea0:0:2200::fe0:290`
- [x] `fe80::220:b3f:f0e0:29`
- [ ] `fe80:0:0:0:220:b3f:f0e0:0029`

### Explanation
* **Key point:** Drop leading zeros (`0220`→`220`, `0b3f`→`b3f`, `0029`→`29`) and replace the run of three all-zero hextets with `::`.
* **Why not the others:** The last option only applied Rule 1 (no `::`), so it is valid but not *fully* compressed; the other two are scrambled values.

---

## Question 4: Double-colon rule
Which statement about the double colon `::` in IPv6 is correct?

- [ ] It can be used multiple times in one address as long as the runs are separated.
- [x] It can be used only once in an address, to replace a single contiguous run of all-zero hextets.
- [ ] It replaces trailing zeros within any hextet.
- [ ] It must always appear at the end of the address.

### Explanation
* **Key point:** `::` replaces **one** contiguous run of all-zero hextets and may appear **only once** — using it twice would make the number of zero hextets ambiguous.
* **Why not the others:** It works on whole zero hextets, not the trailing zeros inside a hextet, and can appear anywhere a zero run occurs.

---

## Question 5: Compress this address (Choose the best)
What is the fully compressed form of `2001:0db8:0000:0000:00ab:0000:0000:1234`?

- [ ] `2001:db8::ab::1234`
- [x] `2001:db8::ab:0:0:1234`
- [ ] `2001:0db8:0:0:ab:0:0:1234`
- [ ] `2001:db8:0:0:ab::1234`

### Explanation
* **Key point:** There are two separate zero runs (hextets 3-4 and 6-7). You may compress only **one** with `::`. Compressing the first run gives `2001:db8::ab:0:0:1234`.
* **Why not the others:** `2001:db8::ab::1234` uses `::` twice (illegal). The third option only stripped leading zeros. The fourth tries to put `::` in the second run while leaving the first uncompressed, which leaves stray zeros — and it is no shorter.

---

## Question 6: Expand the address
The address `2001:db8:a::1` is expanded to full form as which of the following?

- [ ] `2001:0db8:00a0:0000:0000:0000:0000:0001`
- [x] `2001:0db8:000a:0000:0000:0000:0000:0001`
- [ ] `2001:0db8:0a00:0000:0000:0000:0000:1000`
- [ ] `2001:0db8:a000:0000:0000:0000:0000:0001`

### Explanation
* **Key point:** Restore leading zeros (`a`→`000a`, `1`→`0001`) and expand `::` into the missing zero hextets until there are 8 total.
* **Why not the others:** When restoring leading zeros, the value sits on the **right** of the hextet (`000a`), not the left (`a000`/`0a00`).

---

## Question 7: Address type FE80
What type of IPv6 address is `FE80::1`?

- [x] link-local
- [ ] multicast
- [ ] loopback
- [ ] global unicast

### Explanation
* **Key point:** Any address in `FE80::/10` (the range `FE80` to `FEBF`) is **link-local**, used only on the local link and never routed.
* **Why not the others:** Multicast starts with `FF`, loopback is `::1`, and global unicast is `2000::/3`.

---

## Question 8: Valid link-local address
Which address is a valid IPv6 link-local unicast address?

- [x] `FE80::1:4545:6578:ABC1`
- [ ] `FEC8:1::FFFF`
- [ ] `FE0A::100:7788:998F`
- [ ] `FD80::1:1234`

### Explanation
* **Key point:** Link-local addresses must fall in `FE80::/10`, i.e. begin with `FE80` through `FEBF`. `FE80::1:4545:6578:ABC1` qualifies.
* **Why not the others:** `FEC8` and `FE0A` are outside the FE80–FEBF range, and `FD80` is in the unique-local `FC00::/7` range.

---

## Question 9: FF02::1 target
An IPv6-enabled device sends a packet with the destination address `FF02::1`. What is the target of this packet?

- [ ] only IPv6 DHCP servers
- [x] all IPv6-enabled devices on the local link
- [ ] only IPv6-configured routers
- [ ] the single device on the link configured with this address

### Explanation
* **Key point:** `FF02::1` is the **all-nodes** link-local multicast group — every IPv6-enabled device on the link processes it.
* **Why not the others:** All-routers is `FF02::2`; `FF` addresses are multicast (a group), never a single unicast device.

---

## Question 10: All-routers multicast
Which IPv6 multicast address do all IPv6 routers on a link join when `ipv6 unicast-routing` is enabled?

- [ ] `FF02::1`
- [x] `FF02::2`
- [ ] `FF02::1:2`
- [ ] `::1`

### Explanation
* **Key point:** `FF02::2` is the **all-routers** multicast group. A router joins it when it is enabled as an IPv6 router with `ipv6 unicast-routing`.
* **Why not the others:** `FF02::1` is all-nodes and `::1` is loopback.

---

## Question 11: Match address to type
Match each IPv6 address with its address type.

| IPv6 Address | Address Type |
| :--- | :--- |
| `FF02::1` | **all-nodes multicast** |
| `2001:DB8::BAF:3F57:FE94` | **global unicast** |
| `FF02::1:FFAE:F85F` | **solicited-node multicast** |
| `::1` | **loopback** |
| `FE80::AB8:1` | **link-local** |
| `::` | **unspecified** |

### Explanation
* **Key point:** Identify by prefix — `FF02::1` all-nodes, `FF` + `FF` in the solicited-node pattern (`FF02::1:FFxx:xxxx`), `2000::/3` global unicast, `::1` loopback, `FE80::/10` link-local, `::` unspecified.
* **Why this matters:** Prefix-to-type identification is one of the most heavily tested IPv6 skills.

---

## Question 12: Solicited-node multicast
How is an IPv6 solicited-node multicast address formed?

- [ ] from the first 24 bits of the device's global unicast address
- [x] by combining the `FF02::1:FF00:0000/104` prefix with the last 24 bits of the device's unicast address
- [ ] from the device's full 48-bit MAC address
- [ ] by flipping the 7th bit of the link-local address

### Explanation
* **Key point:** The solicited-node multicast address joins the special prefix `FF02::1:FF00:0000/104` with the **last 24 bits** of the device's unicast address, so only relevant hosts process Neighbor Discovery packets.
* **Why not the others:** It uses the last 24 bits, not the first 24 or the whole MAC; bit-flipping is part of EUI-64, not this.

---

## Question 13: EUI-64 source
What is used in the EUI-64 process to create an IPv6 Interface ID on an interface?

- [x] the MAC address of the IPv6-enabled interface
- [ ] an IPv6 address provided by a DHCPv6 server
- [ ] an IPv4 address configured on the interface
- [ ] a randomly generated 64-bit value

### Explanation
* **Key point:** EUI-64 expands the 48-bit **MAC address** into a 64-bit Interface ID by inserting `FFFE` in the middle and flipping the 7th (U/L) bit.
* **Why not the others:** It does not use DHCPv6, IPv4 addresses, or random values (random IDs are a privacy-extension alternative, not EUI-64).

---

## Question 14: EUI-64 worked
A host has MAC address `00:1A:2B:3C:4D:5E`. Using EUI-64, what is the resulting Interface ID?

- [ ] `001A:2BFF:FE3C:4D5E`
- [x] `021A:2BFF:FE3C:4D5E`
- [ ] `001A:2BFE:FF3C:4D5E`
- [ ] `041A:2BFF:FE3C:4D5E`

### Explanation
* **Key point:** Insert `FFFE` in the middle (`001A2B` + `FFFE` + `3C4D5E`), then flip the 7th bit of the first byte: `00` = `0000 0000` → `0000 0010` = `02`. Result: `021A:2BFF:FE3C:4D5E`.
* **Why not the others:** `001A...` forgot the bit flip, one option mis-placed `FFFE` as `FEFF`, and `04` flips the wrong bit.

---

## Question 15: EUI-64 telltale sign
You see an Interface ID of `FE99:47FF:FE75:CEE0`. What does the `FFFE` in the middle most likely indicate?

- [ ] the address is a multicast address
- [ ] the address was assigned by stateful DHCPv6
- [x] the Interface ID was generated using the EUI-64 process from a MAC address
- [ ] the address has failed Duplicate Address Detection

### Explanation
* **Key point:** `FFFE` inserted in the middle of the Interface ID is the signature of **EUI-64**, where 16 bits are stuffed into the 48-bit MAC to make 64 bits.
* **Why not the others:** Multicast is identified by the `FF` *prefix* of the whole address, not `FFFE` in the middle; DHCPv6 and DAD don't produce this pattern.

---

## Question 16: SLAAC definition
Which service provides dynamic global IPv6 addressing to end devices without using a server that keeps a record of available IPv6 addresses?

- [ ] static IPv6 addressing
- [x] SLAAC
- [ ] stateless DHCPv6
- [ ] stateful DHCPv6

### Explanation
* **Key point:** **SLAAC** (Stateless Address Autoconfiguration) lets a host build its own global address from the prefix in a Router Advertisement — **no server keeps any address records**.
* **Why not the others:** Static is manual; stateful DHCPv6 assigns and *tracks* addresses; stateless DHCPv6 still uses SLAAC for the address but adds a DHCPv6 server for DNS/domain options.

---

## Question 17: SLAAC protocol
Which protocol supports SLAAC for dynamic assignment of IPv6 addresses to a host?

- [x] ICMPv6
- [ ] ARPv6
- [ ] DHCPv6
- [ ] UDP

### Explanation
* **Key point:** SLAAC relies on **ICMPv6** Neighbor Discovery messages — Router Solicitations (RS) and Router Advertisements (RA).
* **Why not the others:** There is no "ARPv6" (NDP replaces ARP); DHCPv6 is a different (server-based) method; UDP is just a transport.

---

## Question 18: Stateful vs stateless DHCPv6
A host receives its full global unicast address, prefix length, and DNS server addresses directly from a DHCPv6 server that tracks which address was given to which client. Which method is in use?

- [ ] SLAAC only
- [ ] stateless DHCPv6
- [x] stateful DHCPv6
- [ ] static configuration

### Explanation
* **Key point:** **Stateful DHCPv6** assigns the full address *and* keeps state (records) of each assignment — the IPv6 method most like IPv4 DHCP. The router's RA sets the M flag to direct hosts here.
* **Why not the others:** SLAAC and stateless DHCPv6 both have the host self-build the address via the RA prefix; stateless DHCPv6's server supplies only options like DNS, keeping no address state.

---

## Question 19: Enabling IPv6 routing (RAs)
Which global configuration command must be entered so that a Cisco router will send ICMPv6 Router Advertisements and act as an IPv6 router?

- [ ] `ipv6 enable`
- [ ] `ipv6 address autoconfig`
- [x] `ipv6 unicast-routing`
- [ ] `ip routing`

### Explanation
* **Key point:** `ipv6 unicast-routing` enables IPv6 routing, causing the router to send periodic RAs and join the all-routers (`FF02::2`) group.
* **Why not the others:** `ipv6 enable` only turns on IPv6/link-local on an interface; `ip routing` is for IPv4.

---

## Question 20: Neighbor Discovery messages
Which two ICMPv6 Neighbor Discovery messages are used for address resolution and Duplicate Address Detection? (Choose two.)

- [ ] Router Solicitation (RS)
- [ ] Router Advertisement (RA)
- [x] Neighbor Solicitation (NS)
- [x] Neighbor Advertisement (NA)
- [ ] Redirect

### Explanation
* **Key point:** **NS** and **NA** handle address resolution (find a MAC, IPv6's ARP replacement) and DAD (verify uniqueness before use).
* **Why not the others:** RS/RA are the router/SLAAC pair; Redirect tells a host a better next-hop.

---

## Question 21: SLAAC message flow
When a host is configured for SLAAC and wants addressing information immediately rather than waiting, which message does it send, and to which address?

- [ ] a Neighbor Solicitation to `FF02::1`
- [x] a Router Solicitation to the all-routers group `FF02::2`
- [ ] a Router Advertisement to `FF02::1`
- [ ] a Neighbor Advertisement to `::1`

### Explanation
* **Key point:** The host sends a **Router Solicitation (RS)** to the all-routers multicast `FF02::2`; the router replies immediately with a Router Advertisement (RA).
* **Why not the others:** RAs are sent *by routers*, not hosts; NS/NA are for neighbor address resolution and DAD, not requesting SLAAC info.

---

## Question 22: IPv6 subnetting count
A network administrator received the IPv6 prefix `2001:DB8::/48`. Assuming the administrator does not subnet into the Interface ID, how many subnets can be created?

- [ ] 16
- [x] 65536
- [ ] 256
- [ ] 4096

### Explanation
* **Key point:** The Interface ID boundary is `/64`. Bits available for subnetting = 64 − 48 = **16 bits**, so 2¹⁶ = **65,536** `/64` subnets.
* **Why not the others:** 16 = 2⁴ (a single nibble), 256 = 2⁸, 4096 = 2¹² — none match the full 16-bit Subnet ID.

---

## Question 23: Last subnet calculation
Given the IPv6 prefix `2001:db8::/48`, what is the last subnet created if the subnet prefix is changed to `/52`?

- [x] `2001:db8:0:f000::/52`
- [ ] `2001:db8:0:f00::/52`
- [ ] `2001:db8:0:f::/52`
- [ ] `2001:db8:0:8000::/52`

### Explanation
* **Key point:** Going from `/48` to `/52` borrows 4 bits — the first hex digit of the 4th hextet. The maximum nibble value is `1111` = `f`, so the last subnet is `2001:db8:0:f000::/52`.
* **Why not the others:** `f00` and `f` put the `f` in the wrong nibble position; `8000` is only the midpoint, not the last subnet.

---

## Question 24: Nibble boundary
Why is it best practice in IPv6 to subnet on a nibble (4-bit) boundary?

- [ ] because it is the only mathematically valid way to subnet IPv6
- [x] because each hexadecimal digit represents exactly 4 bits, keeping subnet boundaries readable in the address
- [ ] because it conserves the maximum number of host addresses
- [ ] because routers cannot route prefixes that are not on a nibble boundary

### Explanation
* **Key point:** A nibble is 4 bits = one hex digit. Subnetting on nibble boundaries keeps each subnet's value aligned to a single hex character, making addresses easy to read and manage.
* **Why not the others:** Non-nibble prefixes are perfectly routable and valid; IPv6 subnetting isn't about conserving hosts (there are plenty).

---

## Question 25: Coexistence methods (matching)
Three methods allow IPv6 and IPv4 to co-exist. Match each method with its description.

| Description | Method |
| :--- | :--- |
| IPv6 packets are converted into IPv4 packets, and vice versa. | **translation (NAT64)** |
| The IPv6 packet is transported inside an IPv4 packet. | **tunneling** |
| IPv4 and IPv6 run on the same device/network at the same time. | **dual-stack** |

### Explanation
* **Key point:** Match by the verb — **translation/NAT64 = convert**, **tunneling = encapsulate IPv6 inside IPv4**, **dual-stack = run both simultaneously**.
* **Why this matters:** These three are the standard CCNA IPv4→IPv6 migration techniques and are frequently tested as a matching item.

---

## Question 26: Loopback and unspecified
Which two statements correctly describe special IPv6 addresses? (Choose two.)

- [x] `::1` is the loopback address a host uses to send a packet to itself.
- [ ] `::` (unspecified) can be assigned to a physical interface as a valid address.
- [x] `::` is the unspecified address and is used only as a source address.
- [ ] `::1` can be assigned to a physical interface and routed across the network.

### Explanation
* **Key point:** `::1` is loopback (cannot be assigned to a physical interface), and `::` is the unspecified address used only as a source (e.g., during DAD before an address is confirmed).
* **Why not the others:** Neither `::` nor `::1` can be assigned to a physical interface; loopback is not routed off the device.

---

## Question 27: Global unicast prefix
Which range is currently used for assigned IPv6 global unicast addresses?

- [ ] `FE80::/10`
- [ ] `FC00::/7`
- [x] `2000::/3`
- [ ] `FF00::/8`

### Explanation
* **Key point:** Global unicast addresses currently come from `2000::/3` (first three bits `001`) — the public, Internet-routable space.
* **Why not the others:** `FE80::/10` is link-local, `FC00::/7` is unique local, and `FF00::/8` is multicast.
