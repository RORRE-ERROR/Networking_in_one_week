# Chapter 6 — IPv6 Addressing

> **Day 4 of 7** · Estimated study time: ~3 hours
> *IPv6 is the long-term replacement for IPv4. This chapter teaches you how to read, write, compress, and assign 128-bit addresses, and how devices automatically get one.*

## 🎯 What you'll be able to do after this chapter
- Explain *why* we need IPv6 (IPv4 address exhaustion + Internet of Things).
- Read a 128-bit IPv6 address and **compress** or **expand** it correctly using the two rules.
- Identify every IPv6 address **type** from its prefix (global unicast, link-local, loopback, multicast, etc.).
- Build an Interface ID by hand using the **EUI-64** process from a MAC address.
- Tell apart **SLAAC**, **stateless DHCPv6**, and **stateful DHCPv6** (a classic exam trap).
- Describe **ICMPv6 / Neighbor Discovery** (RS, RA, NS, NA) and what each message does.
- Subnet an IPv6 prefix and describe IPv4→IPv6 coexistence (dual-stack, tunneling, translation).

---

## 1. Why IPv6? (The motivation)

Think of IPv4 like a phone-number system that ran out of numbers. IPv4 uses **32 bits**, giving about **4.3 billion** addresses. That sounds huge, but:

- On **31 January 2011**, IANA handed out its **last two /8 IPv4 blocks** to the Regional Internet Registries (RIRs). The top-level pool was empty.
- The Internet became the **Internet of Things** — not just PCs and phones, but cars, sensors, appliances, biomedical devices. Far more devices than 4.3 billion.
- **NAT** (Network Address Translation) bought IPv4 time but breaks true end-to-end connectivity and adds complexity.

**IPv6 uses 128 bits.** That is `2^128` addresses — roughly **340 undecillion** (3.4 × 10³⁸). Effectively unlimited for any foreseeable future.

> 💡 **Exam tip:** Memorize the two numbers — IPv4 = **32 bits**, IPv6 = **128 bits**. And remember the trigger event: *IANA allocated the last IPv4 blocks in 2011.*

---

## 2. The 128-bit format and hextets

An IPv6 address is **128 bits**, written as **8 groups of 16 bits**. Each 16-bit group is called a **hextet** and is written as **4 hexadecimal digits** (each hex digit = 4 bits, so 4 digits × 4 bits = 16 bits per hextet).

- Preferred format: `X:X:X:X:X:X:X:X` where each `X` is a hextet (`xxxx`).
- 8 hextets × 4 hex digits = **32 hex digits total**.
- Groups are separated by colons `:` (not dots like IPv4).
- IPv6 is **case-insensitive**; lowercase is preferred but `ABCD` = `abcd`.

Example (full / preferred form):

```
2001:0db8:0000:1111:0000:0000:0000:0200
```

> 💡 **Exam tip:** A hextet = 16 bits = 4 hex digits. Don't confuse a *hextet* (IPv6) with an *octet* (8 bits, IPv4).

---

## 3. Compression rules (read this carefully — exams love it)

Full IPv6 addresses are long, so two rules shorten them. **You must be able to apply both, and reverse them.**

### Rule 1 — Omit leading zeros
In **any hextet**, drop the **leading** zeros (the zeros on the *left*). You may **not** drop trailing zeros, and a hextet of all zeros becomes a single `0`.

| Hextet (full) | After Rule 1 |
|---|---|
| `0db8` | `db8` |
| `0000` | `0` |
| `0200` | `200` |
| `00ab` | `ab` |
| `ab00` | `ab00` (trailing zeros stay!) |

### Rule 2 — Double colon `::`
A **single** run of one or more **all-zero hextets** can be replaced with `::`.

- You may use `::` **only once** per address (otherwise it's ambiguous — you couldn't tell how many zero hextets each `::` represents).
- If there are two separate runs of zeros, compress the **longer** one with `::`.

### Worked compression examples

**Example A**
```
Full:        2001:0db8:0000:1111:0000:0000:0000:0200
Rule 1:      2001:db8:0:1111:0:0:0:200
Rule 2 (::): 2001:db8:0:1111::200
```
(The longest zero run is the three hextets, so `::` replaces those.)

**Example B**
```
Full:        fe80:0000:0000:0000:0220:0b3f:f0e0:0029
Rule 1:      fe80:0:0:0:220:b3f:f0e0:29
Rule 2 (::): fe80::220:b3f:f0e0:29
```

**Example C — loopback**
```
Full:   0000:0000:0000:0000:0000:0000:0000:0001
Result: ::1
```

**Example D — two zero runs (only compress one!)**
```
Full:        2001:0db8:0000:0000:00ab:0000:0000:1234
Rule 1:      2001:db8:0:0:ab:0:0:1234
Best (::):   2001:db8::ab:0:0:1234     ✅ (compress the FIRST run; both runs are length 2, pick one)
WRONG:       2001:db8::ab::1234        ❌ (two :: is illegal)
```

### Worked expansion examples (reverse the rules)

To **expand**, count how many hextets are written, then `::` fills the rest with zero hextets until you reach 8 total.

**Example E**
```
Compressed: 2001:db8:a::1
Written hextets: 2001, db8, a, 1  → 4 hextets, so :: = 8-4 = 4 zero hextets
Expanded:   2001:0db8:000a:0000:0000:0000:0000:0001
```

**Example F**
```
Compressed: ff02::1
Written hextets: ff02, 1 → 2 hextets, so :: = 6 zero hextets
Expanded:   ff02:0000:0000:0000:0000:0000:0000:0001
```

> 💡 **Exam tip:** `::` can be used **only once**. A favorite trap answer shows an address with two `::` — it's always wrong. Also remember: leading zeros go, **trailing zeros stay**.

---

## 4. IPv6 address types

There are three big categories: **unicast** (one interface), **multicast** (a group of interfaces), and **anycast** (nearest of several interfaces sharing one address). IPv6 has **no broadcast** — its job is done by multicast.

Within unicast there are several sub-types. Learn each one **by its prefix** — that is the highest-yield fact on the exam.

| Address type | Prefix / value | What it's for |
|---|---|---|
| **Global unicast (GUA)** | `2000::/3` (first 3 bits `001`) | Public, Internet-routable — like a public IPv4 address |
| **Link-local (LLA)** | `FE80::/10` (range `FE80`–`FEBF`) | Communication on the **same link only**; **never routed** |
| **Unique local (ULA)** | `FC00::/7` (i.e. `FC00`–`FDFF`) | Private/site-internal; not routed on the global Internet |
| **Loopback** | `::1` | A host talking to itself (like 127.0.0.1) |
| **Unspecified** | `::` (all zeros) | "No address yet" — source only, never a destination/interface |
| **Multicast** | `FF00::/8` (starts with `FF`) | One packet delivered to a group of interfaces |

### Multicast addresses worth memorizing

| Multicast address | Group |
|---|---|
| `FF02::1` | **All-nodes** — every IPv6 device on the link |
| `FF02::2` | **All-routers** — every IPv6 router on the link |
| `FF02::1:FFxx:xxxx` | **Solicited-node** — built from the last 24 bits of a unicast address |

The **solicited-node** multicast address is formed by combining the prefix `FF02::1:FF00:0000/104` with the **last 24 bits** of the device's unicast address. It is created automatically when a global or link-local unicast address is assigned, and is used by Neighbor Discovery (address resolution / DAD) so only relevant hosts process the packet — much more efficient than IPv4 broadcasts.

> 💡 **Exam tip:** Burn these into memory: **`FE80::/10` = link-local**, **`FF02::1` = all-nodes multicast**, **`FF02::2` = all-routers**, **`::1` = loopback**, **`2000::/3` = global unicast**. These exact items appear again and again.

### Anatomy of a global unicast address

A typical GUA splits into three parts, with a **/64** boundary for the host portion:

```
| Global Routing Prefix |  Subnet ID  |        Interface ID         |
|       /48 (ISP)       |   16 bits   |          64 bits            |
2001:0db8:000a   :   0001   :   0000:0000:0000:0100
```

- **Global routing prefix** — assigned by the ISP/RIR (commonly a **/48** to a site).
- **Subnet ID** — chosen by the organization to number its internal subnets.
- **Interface ID** — equivalent to the IPv4 *host* portion; standard size is **64 bits** (`/64`).

> 💡 **Exam tip:** The default IPv6 host/interface boundary is **/64**. The prefix length (e.g. `/64`) marks the network portion, exactly like a subnet mask does in IPv4 — but IPv6 always uses prefix notation, never dotted-decimal masks.

---

## 5. The EUI-64 process (build an Interface ID from a MAC)

A device can generate its own 64-bit **Interface ID** from its 48-bit MAC address using **EUI-64**. Cisco IOS routers do this by default for link-local addresses.

The intuition: a MAC is only 48 bits but we need 64 bits, so we **stuff 16 bits into the middle** and **flip one bit** to mark it.

### Steps
1. **Split** the 48-bit MAC into two halves (24-bit OUI + 24-bit device portion).
2. **Insert `FFFE`** (16 bits) in the middle, between the two halves. Now it's 64 bits.
3. **Flip the 7th bit** of the first byte (the **U/L bit — Universal/Local**). `0`→`1`. In practice this changes the second hex digit of the first byte.

### Worked example

```
MAC address:        FC:99:47:75:CE:E0

Step 1 — split:     FC9947        75CEE0
Step 2 — insert:    FC9947  FFFE  75CEE0   →  FC99:47FF:FE75:CEE0
Step 3 — flip 7th bit of first byte:
        FC = 1111 1100  → flip 7th bit → 1111 1110 = FE
Result Interface ID:  FE99:47FF:FE75:CEE0
```

So with prefix `2001:db8:acad:1::/64`, the full address becomes:

```
2001:db8:acad:1:FE99:47FF:FE75:CEE0
```

**Bit-flip cheat for the first byte** (only the value of the 2nd hex digit changes by ±2 in its bit pattern):

| First byte before | After flip (7th bit) |
|---|---|
| `FC` (`1111 1100`) | `FE` (`1111 1110`) |
| `00` (`0000 0000`) | `02` (`0000 0010`) |
| `52` (`0101 0010`) | `50` (`0101 0000`) |

> 💡 **Exam tip:** The dead-giveaway sign of an EUI-64 address is **`FFFE` sitting in the middle** of the Interface ID. And EUI-64 uses the interface's **MAC address** as the seed — not an IPv4 address, not a random value.

---

## 6. Static vs dynamic configuration — SLAAC vs DHCPv6 (the trap)

A host can get its address **statically** (you type it) or **dynamically**. There are **three dynamic methods**, and telling them apart is one of the most-tested IPv6 topics.

All dynamic methods depend on the router's **ICMPv6 Router Advertisement (RA)** message. The RA carries a flag (the **M** and **O** bits) that tells the host which method to use. To make a router send RAs, you must enable routing:

```
Router(config)# ipv6 unicast-routing
```

Routers send RAs periodically (about every **200 seconds**) and also reply immediately to a host's **Router Solicitation (RS)**.

| Method | Where does the **address** come from? | Does a server track (keep "state" of) addresses? | Extra info (DNS, domain) from? |
|---|---|---|---|
| **SLAAC** | Host builds it itself: **prefix from RA** + Interface ID (EUI-64 or random) | **No server at all** | RA only |
| **Stateless DHCPv6** | Host builds it itself via SLAAC (prefix from RA) | **No** — server keeps no address records | **DHCPv6 server** gives DNS/domain |
| **Stateful DHCPv6** | **DHCPv6 server** hands out the full address | **Yes** — server tracks which address went to which client | DHCPv6 server gives address **and** DNS |

How to remember it:
- **SLAAC** = "Self-Less, Address Auto-Config." The **router's RA** gives the prefix; the host makes the rest itself. **No DHCPv6 server involved.** Like a public IPv4 address chosen by the host. (RA flag: M=0, O=0.)
- **Stateless DHCPv6** = SLAAC for the *address*, but a DHCPv6 server fills in **only extra options** like DNS server and domain name. (RA flag: M=0, O=1.)
- **Stateful DHCPv6** = the closest thing to IPv4 DHCP. The **server assigns the full address** and remembers ("stateful") the assignment. (RA flag: M=1.)

> 💡 **Exam tip:** The classic question is *"which service provides dynamic global IPv6 addressing without a server that keeps records of available addresses?"* → **SLAAC**. The word **"stateless"** = the server keeps **no record (no state)** of which address went where. **Stateful** = it does.

---

## 7. ICMPv6 and Neighbor Discovery Protocol (NDP)

In IPv4 you had ARP (find a MAC) and ICMP (ping/errors) as separate things. IPv6 rolls neighbor functions into **ICMPv6**, via the **Neighbor Discovery Protocol (NDP)**. NDP adds **five** message types:

| Message | Abbrev | Purpose (plain English) |
|---|---|---|
| **Router Solicitation** | **RS** | Host → routers: "Any routers here? Send me an RA." (sent to all-routers `FF02::2`) |
| **Router Advertisement** | **RA** | Router → hosts: "Here's the prefix, prefix length, default gateway, and config method." (SLAAC info) |
| **Neighbor Solicitation** | **NS** | "Who has this IPv6 address? What's your MAC?" — used for **address resolution** and **DAD** |
| **Neighbor Advertisement** | **NA** | "That's me, here's my MAC." — the reply to an NS |
| **Redirect** | — | Router tells a host a better next-hop for a destination |

Two key NDP jobs:

1. **Address resolution** — the IPv6 equivalent of ARP. A device knows a neighbor's IPv6 address but not its MAC, so it sends an **NS** to the destination's **solicited-node multicast** address; the owner replies with an **NA**.
2. **Duplicate Address Detection (DAD)** — before using a new address, a device sends an **NS targeting its own address**. If an **NA** comes back, the address is already in use; if nothing comes back, it's unique and safe to use.

> 💡 **Exam tip:** Mnemonic — **R**outers do **R**S/**R**A (the autoconfig pair), **N**eighbors do **N**S/**N**A (the address-resolution + DAD pair). SLAAC relies on **RS/RA**; finding a MAC or doing DAD relies on **NS/NA**.

---

## 8. IPv6 subnetting

IPv6 subnetting feels easier than IPv4 because **you are not trying to conserve addresses** — you're designing a clean, hierarchical layout. There is so much space you almost never borrow from the host portion.

- A standard site gets a **/48** from the RIR/ISP. That leaves the **16-bit Subnet ID** (bits 49–64) free.
- Subnetting on that 16-bit Subnet ID gives **2¹⁶ = 65,536** `/64` subnets — **without touching the 64-bit Interface ID** at all.
- Best practice: subnet on a **nibble boundary** (4 bits = 1 hex digit). Extending `/64` by one nibble → `/68`, which **shrinks the Interface ID from 64 to 60 bits** and is mainly done for security (fewer hosts per subnet), not to gain subnets.

### Worked subnetting example

Given **`2001:db8::/48`**, you have 16 subnet bits. The 4th hextet is the Subnet ID:

```
Subnet 0:   2001:db8:0:0000::/64
Subnet 1:   2001:db8:0:0001::/64
Subnet 2:   2001:db8:0:0002::/64
...
Subnet n:   2001:db8:0:000n::/64
Last (65535): 2001:db8:0:ffff::/64
```

**If you only borrow to /52** (4 bits → 16 subnets), you increment the **first hex digit** of the 4th hextet:

```
First subnet: 2001:db8:0:0000::/52
Next:         2001:db8:0:1000::/52
Next:         2001:db8:0:2000::/52
...
Last:         2001:db8:0:f000::/52
```

> 💡 **Exam tip:** Two formulas to keep ready: (1) Subnets from a /48 without touching the Interface ID = **2¹⁶ = 65,536**. (2) Subnets gained when you extend by *n* bits = **2ⁿ** (so /48→/52 = 4 bits = 16). And the *last* subnet of a borrowed-nibble scheme uses **`f`** in that nibble.

---

## 9. IPv4-to-IPv6 coexistence

We can't flip the whole Internet to IPv6 overnight, so three migration techniques let IPv4 and IPv6 coexist:

| Technique | What it does | Analogy |
|---|---|---|
| **Dual-stack** | Device runs **both** IPv4 and IPv6 stacks at the same time | Being bilingual — speak whichever the other side speaks |
| **Tunneling** | **Encapsulate an IPv6 packet inside an IPv4 packet** to cross an IPv4-only network | Putting an English letter inside a French envelope to mail it |
| **Translation (NAT64)** | **Translate** IPv6 ↔ IPv4 so an IPv6-only host can talk to an IPv4-only host | A live interpreter converting between two languages |

> 💡 **Exam tip:** Match them by the verb: **dual-stack = coexist/run both**, **tunneling = encapsulate (IPv6 inside IPv4)**, **translation (NAT64) = convert one to the other**.

---

## 🧪 Hands-on (Packet Tracer)
- **12.6.6 Configure IPv6 Addressing** — practice assigning static global unicast and link-local addresses to interfaces and hosts, and verifying with `show ipv6 interface`.
- **12.9.1 Implement a Subnetted IPv6 Addressing Scheme** — split a /48 (or /64) into multiple subnets on nibble boundaries and address each segment correctly.
- **9.3.4 IPv6 Neighbor Discovery** — observe RS/RA/NS/NA messages in simulation mode to see SLAAC, address resolution, and DAD in action.

## 🧠 Key terms to memorize
| Term | Meaning in one line |
|---|---|
| Hextet | One 16-bit group (4 hex digits) of an IPv6 address |
| Global unicast (GUA) | Public, Internet-routable IPv6 address, `2000::/3` |
| Link-local (LLA) | Same-link-only address, `FE80::/10`, never routed |
| Unique local (ULA) | Private site address, `FC00::/7` |
| Loopback | `::1`, a host's own address |
| Unspecified | `::`, "no address," source only |
| Solicited-node multicast | `FF02::1:FFxx:xxxx`, last 24 bits of a unicast address |
| EUI-64 | Builds a 64-bit Interface ID from a MAC by inserting `FFFE` and flipping the U/L bit |
| SLAAC | Host auto-builds its address from the RA prefix, no DHCPv6 server |
| Stateful DHCPv6 | Server assigns full address and tracks it (like IPv4 DHCP) |
| NDP | Neighbor Discovery Protocol (ICMPv6): RS, RA, NS, NA, Redirect |
| DAD | Duplicate Address Detection — checks an address is unique before use |
| Dual-stack | Running IPv4 and IPv6 simultaneously |

## ⚡ Exam tips & traps (recap)
- IPv4 = **32 bits**; IPv6 = **128 bits** = 8 hextets of 16 bits.
- Compression: drop **leading** zeros (trailing stay); `::` replaces one run of all-zero hextets and can be used **only once**.
- **`FE80::/10` = link-local**, **`FF02::1` = all-nodes**, **`FF02::2` = all-routers**, **`::1` = loopback**, **`::` = unspecified**, **`2000::/3` = global unicast**, **`FC00::/7` = unique local**.
- **`FFFE` in the middle** of the Interface ID = the address was built with **EUI-64** (from a **MAC**, then U/L bit flipped).
- **SLAAC = no server records the addresses** (stateless). **Stateful DHCPv6 = server assigns + tracks** addresses. Stateless DHCPv6 = SLAAC address + DHCPv6 for DNS/domain only.
- SLAAC needs **RS/RA**; address resolution and **DAD** use **NS/NA**.
- IPv6 has **no broadcast** — multicast (and anycast) replace it.
- A /48 site without touching the Interface ID = **65,536** `/64` subnets; borrowing *n* bits = **2ⁿ** subnets; subnet on **nibble** (4-bit) boundaries.
- Coexistence: **dual-stack** (both), **tunneling** (IPv6 in IPv4), **translation/NAT64** (convert).
- Enable a router to send RAs / act as an IPv6 router with **`ipv6 unicast-routing`**.

## ✅ Quick self-check
1. Compress `2001:0db8:0000:0000:0ab0:0000:0000:0001` to its shortest legal form.
2. What address type is `FE80::1`, and will a router forward it off the link?
3. Given MAC `00:1A:2B:3C:4D:5E`, what is the EUI-64 Interface ID (show the `FFFE` insertion and the flipped byte)?
4. Which dynamic method gives a host a global address with **no DHCPv6 server keeping any records**?
5. From `2001:db8:cafe::/48`, how many `/64` subnets can you make without borrowing from the Interface ID?
