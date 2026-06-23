# Chapter 3 — Network Access and Ethernet

> **Day 2 of 7** · Estimated study time: ~3.5 hours
> *This chapter is about the bottom two OSI layers — how bits physically travel over wire, fiber, and air (Layer 1), and how those bits get organised into addressed frames and put onto shared media without devices talking over each other (Layer 2). Ethernet, switching, and ARP are the stars.*

## 🎯 What you'll be able to do after this chapter
- Explain what the physical layer does and tell apart **bandwidth**, **throughput**, and **goodput**.
- Choose the right network medium (copper UTP/STP, coax, fiber, wireless) and the right UTP cable type (straight-through / crossover / rollover) for any connection.
- Describe the two data link sublayers (**LLC** and **MAC**) and what each one is responsible for.
- Compare the media-access methods **CSMA/CD** vs **CSMA/CA**, and **half-** vs **full-duplex**.
- Read an Ethernet frame, interpret a 48-bit **MAC address**, and tell unicast / broadcast / multicast apart.
- Explain how a switch **learns and forwards** using its MAC address table, and compare **store-and-forward** vs **cut-through** switching.
- Walk through the **ARP** request/reply process step by step and recognise an **ARP spoofing** attack.

---

## 1. The Physical Layer (Layer 1)

**Intuition first:** the physical layer is the part of the network that actually moves *bits* — 1s and 0s — across a cable or through the air. It takes a finished frame from the layer above and turns it into something physical: electrical pulses, flashes of light, or radio waves.

The **physical layer** accepts a complete frame from the data link layer and **encodes** it as a series of signals transmitted onto the local media. The signal type depends on the medium:

| Medium | Signal is a pattern of… |
|---|---|
| Copper cable | electrical pulses |
| Fiber-optic cable | light |
| Wireless | microwave / radio transmissions |

### The three functional areas of physical-layer standards
Physical-layer standards (from bodies like IEEE, TIA/EIA, ITU) define three things:

1. **Physical components** — the hardware: NICs (network adapters), interfaces, connectors, cable materials, and cable designs.
2. **Encoding** — converting a stream of data bits into a predefined *code*: a predictable grouping of bits both sender and receiver agree on. Encoding can also mark control events like the start/end of a frame.
   - **Manchester encoding:** a `0` is a high-to-low voltage transition; a `1` is a low-to-high transition. (Used by older 10BASE-T Ethernet.)
   - **Non-Return to Zero (NRZ):** a `0` is one voltage level and a `1` is a different voltage level.
3. **Signaling** — the method of *representing the bits on the medium*. The standard must say what signal means `1` and what means `0` (e.g., a long pulse = 1, a short pulse = 0).

> 💡 **Exam tip:** Don't confuse **encoding** with **signaling**. Encoding = grouping bits into a recognisable code (the pattern). Signaling = the actual physical thing on the wire that represents a 1 or a 0 (the voltage/light/wave).

### Bandwidth vs Throughput vs Goodput
These three are *constantly* confused on the exam. Learn the hierarchy.

- **Bandwidth** = the *capacity* of a medium to carry data — the theoretical maximum. Measured in kbps, Mbps, Gbps. (Think: the width of a pipe.)
- **Throughput** = the *actual* measure of bits transferred across the media over a period of time. Always **less than** bandwidth because of real-world factors:
  - the **amount** of traffic
  - the **type** of traffic
  - **latency** — the delay introduced by every networking device between source and destination.
- **Goodput** = *usable* data delivered over time. It is **throughput minus overhead** (session setup, acknowledgments, encapsulation headers, and retransmitted bits).

> 💡 **Exam tip:** Memorise the ordering — **Goodput ≤ Throughput ≤ Bandwidth.** Goodput is the smallest because it strips out all the "non-data" bytes. **Latency** = total delay (including queuing) for data to get from A to B.

---

## 2. Network Media

Three big families: **copper**, **fiber**, and **wireless**. Each has trade-offs in cost, distance, speed, and interference.

### Copper
Copper is **cheap, easy to install, and has low resistance** — but it's limited by **distance** and by **signal interference**. Three types:

| Copper type | Notes | Connector |
|---|---|---|
| **Unshielded Twisted-Pair (UTP)** | Most common LAN media. Twists in the pairs cancel interference. | **RJ-45** |
| **Shielded Twisted-Pair (STP)** | Adds shielding → better noise protection, but more expensive and harder to install. | **RJ-45** |
| **Coaxial (coax)** | Attaches antennas to wireless devices (carries RF energy between antenna and radio); also used for **cable Internet**. | (various, e.g. F-type/BNC) |

**Three enemies of copper signals** — know these by name:
- **Attenuation** — loss of signal strength as distance increases. (This is why UTP runs are capped at ~100 m.)
- **Crosstalk** — leakage/interference of a signal from one wire pair into an adjacent pair. The *twists* in twisted-pair cable exist specifically to cancel crosstalk.
- **EMI / RFI** — Electromagnetic / Radio-Frequency Interference from outside sources (motors, fluorescent lights, microwaves, etc.).

> 💡 **Exam tip:** Classic trap — *attenuation* = signal weakening over distance; *crosstalk* = signal leaking between pairs. If you strip back the cable jacket too far and leave wires **untwisted at the connector**, you get excessive crosstalk. The jacket must extend into the connector's strain-relief.

### UTP Cable Types — straight-through vs crossover vs rollover ⭐ (high-yield)
Different connections need the wire pairs arranged differently. This is one of the **most-tested** topics in the chapter.

| Cable | Pinout | Use it to connect… | Memory hook |
|---|---|---|---|
| **Straight-through** | Same pinout both ends | **Unlike** devices (different OSI layers): host↔switch, switch↔router | "different devices, straight wires" |
| **Crossover** | Tx and Rx pairs crossed | **Like** devices (same type): switch↔switch, host↔host, router↔router, host↔router | "same devices, cross them" |
| **Rollover** (Cisco proprietary) | Pinout fully reversed | A PC's COM/serial port → a device's **console** port (out-of-band management) | "roll over to the console" |

A quick way to decide straight vs crossover: **PC, router, and server interfaces are "like" each other; switch and hub interfaces are "like" each other.** Connecting two devices from the *same* group → crossover. Connecting across the two groups → straight-through.

> 💡 **Exam tip:** Console port = **rollover**, every time. And remember **Auto-MDIX**: modern switch/router interfaces auto-detect the cable type and adjust internally, so a straight-through cable *will* work even where you'd technically need a crossover. (More on Auto-MDIX in §5.)

### Fiber-Optic
Fiber permits transmission over **longer distances** and at **higher bandwidth** than any other media, and is **immune to EMI/RFI** because it carries light, not electricity. Used in four industries: **Enterprise**, **FTTH/Access** (fiber-to-the-home), **Long-Haul** (dozens to thousands of km, up to 10 Gbps), and **Submarine** (undersea, transoceanic).

| Fiber type | Core | Light source | Where used |
|---|---|---|---|
| **Single-mode (SMF)** | very small core | **laser** (expensive) — single ray of light | long-distance: long-haul telephony, cable TV, hundreds of km |
| **Multimode (MMF)** | larger core | **LED** (low cost) — light enters at many angles | **LANs**; up to 10 Gb/s over up to **550 m** |

**Fiber connectors:**
- **ST (Straight-Tip)** — older bayonet style, common with **multimode**.
- **SC (Subscriber/Square/Standard Connector)** — push-pull mechanism; works with **multimode AND single-mode**.
- **LC (Lucent/Little/Local Connector)** — small size, growing in popularity; used with **single-mode** (also supports multimode).

> 💡 **Exam tip:** Single-mode = **laser**, long distance, small core. Multimode = **LED**, shorter distance, larger core. If the question says "long-haul / hundreds of km" → single-mode. If it says "LAN, low cost" → multimode.

### Wireless
Carries electromagnetic signals (radio/microwave) representing the binary data. Limitations:
- **Coverage area** — building materials and terrain limit effective range.
- **Interference** — susceptible to cordless phones, some fluorescent lights, microwave ovens, and other wireless gear.
- **Security** — a major concern, since the medium is open to anyone in range.

---

## 3. The Data Link Layer (Layer 2) & its two sublayers

**Intuition first:** if Layer 1 just throws bits onto the wire, Layer 2 is the layer that wraps those bits into addressed packages (**frames**), decides *whose turn it is* to use the shared wire, and checks for transmission errors.

The **data link layer** is responsible for:
- exchanging **frames** between nodes over the physical media,
- controlling **media access**, and
- performing **error detection**.

It accepts Layer 3 **packets** and packages them into **frames**.

### The two sublayers ⭐ (high-yield)
The data link layer is split into two sublayers. Knowing which one does what is a favourite exam item.

| Sublayer | Position | Responsibilities |
|---|---|---|
| **LLC — Logical Link Control** | **Upper** sublayer | The *software* interface to the network layer. Identifies **which Layer 3 protocol** is carried (IPv4 vs IPv6 etc.) and adds Layer-2 control info to network-protocol data. Lets multiple L3 protocols share one link. |
| **MAC — Media Access Control** | **Lower** sublayer | The *hardware* processes: **framing** (delimiting data), **Layer-2 addressing** (MAC addresses), and **media access** according to the physical signaling and the L2 protocol in use. |

> 💡 **Exam tip:** **LLC = upper = talks to the software/network layer ("which protocol?"). MAC = lower = talks to the hardware/media ("framing, addressing, access").** If an answer mentions "identifies the network layer protocol" → LLC. If it mentions framing, MAC addressing, or controlling the NIC on the media → MAC.

### Frame structure
Every L2 frame has three parts:

1. **Header** — control information at the *beginning*: addressing (source/dest MAC), type, etc.
2. **Data** — the payload: the IP header, transport-layer header, and application data.
3. **Trailer** — control information at the *end* for **error detection** (the FCS / CRC).

### Topology
Media-access methods depend on:
- **Topology** — how the connection between nodes appears to the data link layer (logical topology), and
- **Media sharing** — how nodes share the medium.

---

## 4. Media Access Control methods

How do devices share one medium without all talking at once? Two basic approaches for **shared media**:

- **Contention-based access** — all nodes *compete* for the medium but follow a plan when collisions happen. Simple, but **doesn't scale well**: as nodes increase, collision probability increases.
- **Controlled access** — each node gets its own scheduled turn. No collisions, but more overhead.

**CSMA** (Carrier Sense Multiple Access) is the contention foundation, paired with a collision strategy:

| Method | Used on | How it handles collisions | One-line summary |
|---|---|---|---|
| **CSMA/CD** (Collision **Detection**) | **Wired** Ethernet (legacy half-duplex / hubs) | Listen; if free, send. If a collision is **detected**, all senders stop and **retry later**. | "Send and listen for a crash, then back off." |
| **CSMA/CA** (Collision **Avoidance**) | **Wireless** (802.11 Wi-Fi) | Listen; if free, **signal intent** first and wait for clearance, *then* send. | "Ask permission first to avoid the crash." |

> 💡 **Exam tip:** **CD = wired Ethernet, CA = wireless.** Wireless can't reliably *detect* collisions (a station can't transmit and listen at the same time on the same channel), so it tries to *avoid* them by announcing intent first.

### Duplex (closely tied to media access)
- **Half-duplex** — send **or** receive, one direction at a time (like a walkie-talkie). Used on shared media / with hubs. **CSMA/CD is required** here because collisions are possible.
- **Full-duplex** — send **and** receive simultaneously (like a phone call). A switch port connected to one device is full-duplex. With a dedicated full-duplex link there are **no collisions**, so CSMA/CD is effectively disabled.

> 💡 **Exam tip:** Hubs = half-duplex + collisions (one collision domain). Switches give each port its own full-duplex, collision-free link. Mismatched duplex (one side full, one side half) is a classic real-world fault → late collisions and slow throughput.

---

## 5. Ethernet

**Ethernet** is the most widely used LAN technology. It operates at the **data link layer and the physical layer**, and is a family of technologies defined by the **IEEE 802.2 (LLC)** and **802.3 (MAC/physical)** standards.

- Supported bandwidths: **10 Mbps, 100 Mbps, 1 Gbps, 10 Gbps, 40 Gbps, 100 Gbps**.
- Ethernet frame size: **minimum 64 bytes, maximum 1518 bytes**. (Frames smaller than 64 bytes are "runts"; larger than 1518 are "jumbo/giants" and normally dropped.)

### Ethernet frame fields (high level)
```
| Preamble + SFD | Destination MAC | Source MAC | Type/Length | Data (payload) | FCS |
|     8 bytes    |     6 bytes     |  6 bytes   |   2 bytes   |  46–1500 bytes | 4 B |
```
- **Preamble/SFD** — synchronisation, marks the start (not counted in the 64–1518 range).
- **Destination & Source MAC** — 6 bytes each.
- **Type/Length** — which L3 protocol (the LLC's EtherType, e.g. IPv4 = 0x0800).
- **FCS (Frame Check Sequence)** — the CRC in the trailer for error detection.

### MAC addresses ⭐
A **MAC address** is a unique **48-bit** value (also called the burned-in address, BIA) written as **12 hexadecimal digits** (4 bits per hex digit), e.g. `00:60:2F:3A:07:BC`.

- First 6 hex digits = **OUI** (Organisationally Unique Identifier — the vendor).
- Last 6 hex digits = vendor-assigned, unique to the NIC.
- Find it with **`ipconfig /all`** (Windows) or `ifconfig`/`ip link` (Linux).

**Three types of Layer-2 addressing:**

| Type | Destination MAC | Meaning |
|---|---|---|
| **Unicast** | one specific NIC's MAC | one-to-one |
| **Broadcast** | **FF:FF:FF:FF:FF:FF** | one-to-all on the LAN |
| **Multicast** | starts `01:00:5E…` (IPv4) | one-to-a-group |

> 💡 **Exam tip:** MAC = **48 bits = 12 hex digits**. Broadcast MAC = **all Fs (FF-FF-FF-FF-FF-FF)**. ARP requests use this broadcast destination (see §7).

### Auto-MDIX
**Auto-MDIX** (Automatic Medium-Dependent Interface Crossover) lets an interface **automatically detect** whether it's connected with a straight-through or crossover cable and adjust its internal Tx/Rx pin assignment. Result: you can use a straight-through cable even between like devices, and it just works. Modern switches enable it by default.

---

## 6. Switching

A **switch** is a Layer-2 device that forwards frames intelligently based on MAC addresses (unlike a hub, which blindly repeats everything).

### How a switch learns and forwards (the MAC address table)
1. **Learn:** When a frame arrives on a port, the switch reads the **source MAC** and records "this MAC lives on this port" in its **MAC address table** (CAM table). It builds the table dynamically.
2. **Forward / Filter:** It looks up the **destination MAC**:
   - If the destination MAC is in the table → forward **only out that one port** (filtering).
   - If the destination MAC is **unknown** (not in the table) → **flood** the frame out all ports except the one it arrived on.
   - If the destination is the **broadcast** address → flood out all ports (except incoming).

### Frame forwarding methods
| Method | When it forwards | Error checking | Notes |
|---|---|---|---|
| **Store-and-forward** | After the **entire frame** is received and buffered | **Yes** — runs a **CRC** check before forwarding; drops bad frames | Higher latency, but no bad frames pass. Cisco default. |
| **Cut-through** | As soon as enough of the frame is read | No CRC (may forward errored frames) | Lower latency. Two variants ↓ |
| → **Fast-forward** | Immediately after reading the **destination MAC** | No | Lowest latency; can forward runts/errored frames |
| → **Fragment-free** | After reading the **first 64 bytes** | Partial | Catches most collision fragments (collisions happen in first 64 B) |

Switches also use **buffering** to store frames temporarily — e.g. when the destination port is busy due to congestion.

### Collision domains vs broadcast domains ⭐
- A **collision domain** is a segment where frames can collide. **Each switch port = its own collision domain.** A hub's ports are all **one** collision domain.
- A **broadcast domain** is the set of devices that receive a broadcast. **By default a switch forwards broadcasts everywhere**, so a switch (or set of connected switches) is **one broadcast domain**. A **router** breaks up broadcast domains (each router interface = a new broadcast domain). VLANs also segment broadcast domains.

> 💡 **Exam tip:** **Switches break collision domains; routers break broadcast domains.** Counting: each switch port adds a collision domain; each router interface bounds a broadcast domain.

---

## 7. Address Resolution Protocol (ARP)

**Intuition first:** a device knows the destination's **IP address** (the "mailing address") but to actually build a frame it needs the destination's **MAC address** (the "name on the door"). **ARP** is how it looks up the MAC that belongs to an IPv4 address on the local network.

> ARP resolves **IPv4 addresses → MAC addresses.** (IPv6 doesn't use ARP — it uses **ICMPv6 Neighbor Discovery / NDP** instead.)

### The ARP process — step by step
1. A device wants to send an IPv4 packet. It checks its **ARP table (ARP cache)** for the destination IP's MAC mapping.
2. **If the mapping is found** → it uses that MAC as the destination MAC in the frame. Done.
3. **If no entry is found** → it sends an **ARP request**: a frame **broadcast** to **FF-FF-FF-FF-FF-FF** asking, "Who has this IPv4 address?" Every device on the local network receives it.
4. The device that **owns that IP** replies with a **unicast ARP reply** containing its MAC address. (All others ignore it.)
5. The sender **adds the mapping (IP ↔ MAC) to its ARP table** and now sends the frame.
6. If **no device responds**, the packet is **dropped** — no frame can be built without a destination MAC.

### ARP and different networks (the default gateway)
If the destination IPv4 is on a **different network** from the source, the device doesn't ARP for the remote host. Instead it ARPs for (and frames the packet to) the **default gateway's** MAC address. The gateway router then routes the packet onward. (The destination IP stays the remote host; the destination MAC is the gateway.)

> 💡 **Exam tip:** Same network → ARP for and frame to the *destination host's* MAC. Different network → ARP for and frame to the *default gateway's* MAC, but the destination **IP** stays the final host. This "IP = final destination, MAC = next hop" idea is tested constantly.

### ARP table aging
An **ARP cache timer** removes entries that haven't been used for a while — typically **15 to 45 seconds** on Windows. View the table with **`arp -a`** (Windows/PC) or **`show ip arp`** (Cisco IOS).

### ARP issues
- **Overhead on the media:** an ARP request is a **broadcast** processed by *every* device on the LAN. If many devices ARP at once, there's a brief performance hit.
- **Security — ARP spoofing / poisoning:** a threat actor **replies to ARP requests with its own MAC**, claiming to be another device (often the **default gateway**). Victims store the wrong MAC in their ARP table and start sending traffic to the attacker — a man-in-the-middle attack. Mitigated by **Dynamic ARP Inspection (DAI)** on switches.

> 💡 **Exam tip:** ARP request = **broadcast** (destination FF-FF-FF-FF-FF-FF). ARP reply = **unicast**. ARP spoofing works because a host blindly trusts any ARP reply and overwrites its table.

---

## 🧪 Hands-on (Packet Tracer)
- **4.6.5 Connect a Wired and Wireless LAN** — practice cabling end devices to switches/APs and seeing wired vs wireless media in one topology.
- **4.7.1 / 4.7.2 Connect the Physical Layer** — pick the correct cable type (straight-through / crossover / console-rollover) for each link; reinforces §2.
- **9.1.3 Identify MAC and IP Addresses** — use `ipconfig /all` and device tables to read MAC vs IP; reinforces §5.
- **9.2.9 Examine the ARP Table** — generate traffic, then `arp -a` / `show ip arp` to watch the ARP cache populate; reinforces §7.
- **9.3.4 IPv6 Neighbor Discovery** — see how IPv6 replaces ARP with ICMPv6 NDP (the IPv6 analogue of the ARP process).

## 🧠 Key terms to memorize
| Term | Meaning in one line |
|---|---|
| Bandwidth | Capacity of the medium (theoretical max). |
| Throughput | Actual bits transferred per unit time (< bandwidth). |
| Goodput | Usable data delivered = throughput minus overhead (smallest). |
| Latency | Total delay for data to travel A→B. |
| Encoding | Grouping bits into a recognisable code/pattern. |
| Signaling | The physical signal that represents a 1 or 0. |
| Attenuation | Signal weakening over distance. |
| Crosstalk | Signal leaking from one wire pair to another. |
| EMI/RFI | Outside electromagnetic/radio interference. |
| Straight-through | Cable for *unlike* devices (host↔switch, switch↔router). |
| Crossover | Cable for *like* devices (switch↔switch, host↔host). |
| Rollover | Cisco cable to a device's **console** port. |
| Auto-MDIX | Interface auto-detects straight vs crossover cabling. |
| LLC | Upper L2 sublayer; identifies the L3 protocol (software). |
| MAC (sublayer) | Lower L2 sublayer; framing, L2 addressing, media access (hardware). |
| MAC address | 48-bit / 12-hex-digit L2 hardware address. |
| CSMA/CD | Collision *detection* — wired Ethernet. |
| CSMA/CA | Collision *avoidance* — wireless. |
| Store-and-forward | Buffers whole frame + CRC check before forwarding. |
| Cut-through | Forwards after reading destination MAC; no CRC. |
| Collision domain | Segment where frames can collide; per switch port. |
| Broadcast domain | Set of devices receiving a broadcast; bounded by routers. |
| ARP | Resolves IPv4 address → MAC (local network). |
| ARP spoofing | Attacker forges ARP replies to redirect traffic. |

## ⚡ Exam tips & traps (recap)
- **Goodput ≤ Throughput ≤ Bandwidth.** Goodput excludes all overhead.
- **Encoding ≠ signaling.** Encoding = the bit pattern/code; signaling = the actual voltage/light on the wire.
- **Attenuation** (weakening over distance) ≠ **crosstalk** (leakage between pairs).
- Cable rules: **straight-through = unlike devices**, **crossover = like devices**, **rollover = console port**. **Auto-MDIX** removes the straight-vs-crossover worry on modern gear.
- Fiber: **single-mode = laser, long distance**; **multimode = LED, LAN/shorter (≤550 m)**. Fiber is immune to EMI/RFI.
- **LLC = upper** (which network protocol?), **MAC = lower** (framing, addressing, media access).
- **CSMA/CD = wired**, **CSMA/CA = wireless.** Half-duplex needs CSMA/CD; full-duplex is collision-free.
- **MAC = 48 bits = 12 hex digits.** Broadcast MAC = **FF-FF-FF-FF-FF-FF.**
- **Store-and-forward does a CRC check (Cisco default); cut-through forwards after reading the destination MAC** (lower latency, no error check). Fragment-free buffers the first 64 bytes.
- **Switches break collision domains; routers break broadcast domains.**
- **ARP request = broadcast, ARP reply = unicast.** Different-network traffic is framed to the **default gateway's MAC**, but the **destination IP stays the remote host**. IPv6 uses **ICMPv6/NDP**, not ARP.

## ✅ Quick self-check
1. You need to connect two switches with a UTP cable, and Auto-MDIX is **disabled** on both. Which cable type do you need — and why would a straight-through still work if Auto-MDIX were enabled?
2. Order these from largest to smallest and define each: bandwidth, goodput, throughput.
3. Which data link sublayer identifies whether the frame carries IPv4 or IPv6, and which one handles framing and MAC addressing?
4. PC-A (192.168.1.10) sends a packet to a server at 10.0.0.5 through its default gateway. What destination **IP** and destination **MAC** go in PC-A's first frame?
5. A switch receives a frame whose destination MAC is **not** in its MAC address table. What does it do — and how does that differ from when the destination is the broadcast address?
