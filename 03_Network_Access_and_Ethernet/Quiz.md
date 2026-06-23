# Chapter 3 Quiz — Network Access and Ethernet

> Exam-style questions. Answers are marked and explained. Cover the explanations and test yourself first.

## Question 1: Bandwidth vs Throughput vs Goodput
Which statement correctly describes the relationship between bandwidth, throughput, and goodput?

- [ ] Goodput is always greater than throughput because it excludes overhead.
- [ ] Throughput is the theoretical maximum capacity of the medium.
- [x] Goodput is less than or equal to throughput, which is less than or equal to bandwidth.
- [ ] Bandwidth equals throughput on an error-free link.

### Explanation
* **Key point:** **Bandwidth** is the medium's theoretical capacity; **throughput** is the actual bits transferred (always ≤ bandwidth due to traffic, latency, etc.); **goodput** is throughput minus overhead (acknowledgments, headers, retransmissions), so it is the smallest. Order: **goodput ≤ throughput ≤ bandwidth**.
* **Why not the others:** Goodput is smaller, not greater, than throughput. Bandwidth (not throughput) is the theoretical maximum, and real overhead still keeps throughput below bandwidth even on an error-free link.

---

## Question 2: Encoding vs Signaling
What is the purpose of encoding at the physical layer?

- [ ] To define which voltage on the wire represents a 1 and which represents a 0.
- [x] To convert a stream of data bits into a predefined code of bit patterns recognizable to sender and receiver.
- [ ] To resolve an IPv4 address to a MAC address.
- [ ] To detect collisions on a shared medium.

### Explanation
* **Key point:** **Encoding** groups data bits into a predictable *code* (pattern) that both ends understand, and can also mark control events like the start/end of a frame.
* **Why not the others:** Defining which signal means 1 vs 0 is **signaling**, not encoding. Resolving IPv4→MAC is ARP; collision detection is CSMA/CD.

---

## Question 3: Copper Signal Impairments
What does the term "attenuation" mean in data communication?

- [ ] leakage of signals from one cable pair to another
- [ ] time for a signal to reach its destination
- [x] loss of signal strength as distance increases
- [ ] interference from motors and fluorescent lights

### Explanation
* **Key point:** **Attenuation** is the weakening of signal strength as it travels farther through the medium — the reason UTP runs are limited to about 100 m.
* **Why not the others:** Leakage between pairs is **crosstalk**; signal travel time is **latency**; outside electrical interference is **EMI/RFI**.

---

## Question 4: Cable Type Selection
A technician needs to connect a host PC directly to a switch, and also connect that switch to a router. Assuming Auto-MDIX is disabled, which cable types are required?

- [ ] both crossover
- [ ] both straight-through... PC to switch crossover, switch to router straight-through
- [x] straight-through for both connections
- [ ] rollover for both connections

### Explanation
* **Key point:** Host↔switch and switch↔router both connect **unlike** devices (different OSI layers), so both use a **straight-through** cable.
* **Why not the others:** Crossover is for *like* devices (switch↔switch, host↔host, router↔router, host↔router); rollover is for console connections only.

---

## Question 5: Console Port Cabling
A network administrator connects a PC's serial/COM port to the console port of a switch for local out-of-band management. Which UTP cable is used?

- [ ] straight-through
- [ ] crossover
- [x] rollover
- [ ] coaxial

### Explanation
* **Key point:** A **rollover** cable (Cisco proprietary, pinout fully reversed) connects a PC to a device's **console** port for management.
* **Why not the others:** Straight-through and crossover are Ethernet data cables; coax is a different medium entirely.

---

## Question 6: Cable Matching by Topology
Refer to the exhibit. The PC is connected to the console port of the switch. All other links are FastEthernet. Match the correct UTP cable to each link.

*(Topology: [PC] --(1)-- [Switch] --(2)-- [Router] --(3)-- [Router])*

| Link | Devices connected | Correct cable |
| :--- | :--- | :--- |
| 1 | PC → Switch **console port** | **rollover** |
| 2 | Switch → Router (unlike devices) | **straight-through** |
| 3 | Router → Router (like devices) | **crossover** |

### Explanation
* **Link 1 (Rollover):** A PC's COM port to a switch's console port for out-of-band management.
* **Link 2 (Straight-through):** Switch (L2) and Router (L3) are *unlike* devices.
* **Link 3 (Crossover):** Router-to-router are *like* peers, so the Tx/Rx pairs must be crossed.

---

## Question 7: Auto-MDIX
A network administrator connects two new, unconfigured switches using a straight-through cable. Which two statements are correct about the result? (Choose two.)

- [x] The auto-MDIX feature configures the interfaces, eliminating the need for a crossover cable.
- [ ] The connection will fail until the cable is replaced with a crossover cable.
- [x] The link will operate at the fastest speed supported by both switches and run full-duplex.
- [ ] Duplex must be configured manually because it cannot be negotiated.

### Explanation
* **Key point:** **Auto-MDIX** automatically detects the cable type and adjusts the interface's internal Tx/Rx polarity, so a straight-through cable works even between like devices. Modern switches also **auto-negotiate** the highest common speed and full-duplex.
* **Why not the others:** No cable swap is needed thanks to Auto-MDIX, and speed/duplex are auto-negotiated, not manually forced.

---

## Question 8: Fiber Selection
Which statements correctly distinguish single-mode from multimode fiber? (Choose two.)

- [x] Single-mode fiber uses laser light sources and a small core for long-distance runs.
- [ ] Multimode fiber uses laser light over hundreds of kilometers.
- [x] Multimode fiber uses LED sources and a larger core, popular in LANs.
- [ ] Single-mode fiber is preferred in LANs because LEDs are inexpensive.

### Explanation
* **Key point:** **Single-mode (SMF)** = small core + **laser**, used for long-distance (hundreds of km). **Multimode (MMF)** = larger core + **LED**, popular in **LANs** (up to 10 Gb/s over ≤550 m).
* **Why not the others:** The LED/laser and LAN/long-haul roles are swapped in the wrong options.

---

## Question 9: Fiber vs Copper
What makes fiber preferable to copper for interconnecting buildings? (Choose three.)

- [x] greater distances per cable run
- [ ] lower installation cost
- [x] greater bandwidth potential
- [x] limited susceptibility to EMI/RFI
- [ ] easier termination

### Explanation
* **Key point:** Fiber carries light, not electricity, so it spans far greater distances, offers much higher bandwidth, and is essentially **immune to EMI/RFI**.
* **Why not the others:** Fiber is generally **more** expensive and **harder** to terminate than copper — those are copper's advantages.

---

## Question 10: LLC Sublayer Functions
Which two functions are performed at the LLC sublayer of the data link layer? (Choose two.)

- [ ] Controls the NIC that sends/receives data on the physical medium.
- [ ] Performs the framing of data.
- [x] Identifies which network layer protocol is carried in the frame.
- [x] Adds Layer 2 control information to network protocol data.
- [ ] Arbitrates access to the shared medium.

### Explanation
* **Key point:** The **LLC (Logical Link Control)** is the *upper* sublayer and the software interface to the network layer. It identifies the L3 protocol (e.g., via the EtherType field) and adds L2 control info, letting multiple protocols share one link.
* **Why not the others:** Framing, hardware/NIC control, and media-access arbitration all happen in the lower **MAC** sublayer.

---

## Question 11: MAC Sublayer Responsibilities
Which task is the responsibility of the MAC sublayer of the data link layer?

- [ ] identifying the upper-layer network protocol
- [x] delimiting frames and providing data link layer (hardware) addressing
- [ ] resolving IPv4 addresses to MAC addresses
- [ ] establishing end-to-end logical connections

### Explanation
* **Key point:** The **MAC** sublayer (lower) handles **framing/delimiting**, **MAC addressing**, and media access — the hardware-oriented functions.
* **Why not the others:** Identifying the L3 protocol is LLC; IPv4→MAC resolution is ARP; end-to-end logical connections are Layer 4 (TCP).

---

## Question 12: Frame Structure
Which part of a data link layer frame contains the information used for error detection?

- [ ] header
- [ ] data field
- [x] trailer
- [ ] preamble

### Explanation
* **Key point:** The **trailer** holds the **FCS (CRC)** used for error detection, added at the end of the frame.
* **Why not the others:** The header carries addressing/control at the start; the data field carries the payload; the preamble is for synchronization.

---

## Question 13: CSMA/CD vs CSMA/CA
Which media access control method is used by legacy half-duplex Ethernet, and which is used by 802.11 wireless?

- [ ] Ethernet uses CSMA/CA; wireless uses CSMA/CD.
- [x] Ethernet uses CSMA/CD; wireless uses CSMA/CA.
- [ ] Both use CSMA/CD.
- [ ] Both use controlled (token-based) access.

### Explanation
* **Key point:** Wired half-duplex Ethernet uses **CSMA/CD** (collision *detection* — send, then back off if a collision is heard). Wireless uses **CSMA/CA** (collision *avoidance* — signal intent first, then send), because a station can't reliably detect collisions while transmitting.
* **Why not the others:** The methods are reversed in the wrong options; neither uses token-based controlled access.

---

## Question 14: Duplex and Collisions
On which type of connection is CSMA/CD required, and why?

- [ ] full-duplex switch links, because both devices transmit at once
- [x] half-duplex shared media, because two devices can transmit simultaneously and collide
- [ ] fiber-optic links, because light pulses interfere
- [ ] any link using Auto-MDIX

### Explanation
* **Key point:** **Half-duplex** shared media (e.g., a hub) allows only one direction at a time, so simultaneous transmissions **collide** — CSMA/CD manages this. A dedicated **full-duplex** switch link is collision-free, so CSMA/CD is effectively disabled.
* **Why not the others:** Full-duplex links have no collisions; the fiber and Auto-MDIX options are irrelevant to collisions.

---

## Question 15: MAC Address Format
Which statement about an Ethernet MAC address is correct?

- [ ] It is a 32-bit value expressed in dotted decimal.
- [x] It is a 48-bit value expressed as 12 hexadecimal digits.
- [ ] It is assigned dynamically by a DHCP server.
- [ ] The last 6 hex digits identify the manufacturer (OUI).

### Explanation
* **Key point:** A MAC address is a **48-bit** burned-in address shown as **12 hex digits** (4 bits per digit).
* **Why not the others:** 32-bit dotted decimal describes an IPv4 address; MACs are burned in by the vendor, not DHCP-assigned; the **first** 6 hex digits are the OUI (vendor), not the last.

---

## Question 16: Layer 2 Frame Reception
What action occurs when a host receives a frame with a destination MAC address it does not recognize as its own, a broadcast, or a joined multicast group?

- [ ] It forwards the frame to all other hosts.
- [ ] It sends the frame to the switch to update the MAC table.
- [x] It discards (drops) the frame.
- [ ] It floods the frame to the default gateway.

### Explanation
* **Key point:** A host's NIC compares the destination MAC to its own address and its broadcast/multicast memberships. If none match, it **drops** the frame to avoid wasting CPU.
* **Why not the others:** Hosts do not flood or relay frames — flooding is a *switch* behavior for unknown-unicast/broadcast frames.

---

## Question 17: Switch Forwarding Decision
A switch receives a frame whose destination MAC address is not in its MAC address table. What does the switch do?

- [ ] It drops the frame.
- [ ] It sends an ARP request to find the MAC address.
- [x] It floods the frame out all ports except the one it was received on.
- [ ] It forwards the frame only to its default gateway.

### Explanation
* **Key point:** For an **unknown unicast** (destination MAC not yet learned), the switch **floods** the frame out every port except the ingress port. When the destination replies, the switch learns its port from the source MAC.
* **Why not the others:** Switches don't drop unknown unicasts, don't send ARP requests (that's a host/router function), and have no concept of a "default gateway" at Layer 2.

---

## Question 18: Switch Forwarding Methods
Refer to the descriptions. Match each switching method to its behavior.

| Description | Method |
| :--- | :--- |
| Stores the entire frame and runs a CRC error check before forwarding | **store-and-forward** |
| Forwards immediately after reading the destination MAC address | **cut-through (fast-forward)** |
| Buffers only the first 64 bytes before forwarding | **fragment-free** |

### Explanation
* **Store-and-forward:** Buffers the whole frame and performs a **CRC** check, dropping errored frames — the Cisco default; higher latency but reliable.
* **Cut-through / fast-forward:** Forwards as soon as the destination MAC is read — lowest latency, but no error check.
* **Fragment-free:** A cut-through variant that buffers the first **64 bytes** (where collisions occur) to filter runt/collision fragments.

---

## Question 19: Collision and Broadcast Domains
Which two statements about collision and broadcast domains are correct? (Choose two.)

- [x] Each port on a switch is a separate collision domain.
- [ ] A router does not affect broadcast domains.
- [x] A router interface bounds a broadcast domain.
- [ ] All ports on a hub are separate collision domains.

### Explanation
* **Key point:** **Switches break up collision domains** — each port is its own. **Routers break up broadcast domains** — each interface bounds one, because routers do not forward Layer 2 broadcasts.
* **Why not the others:** A router *does* segment broadcast domains; a hub's ports all share **one** collision domain.

---

## Question 20: ARP Across Networks
Refer to the scenario. HostA (192.168.1.10) is sending data to ServerB (10.0.0.5) on a different network, through its default gateway RouterA. Which two statements correctly describe the addressing HostA generates? (Choose two.)

- [ ] A packet with the destination IP address of RouterA.
- [x] A packet with the destination IP address of ServerB.
- [ ] A frame with the destination MAC address of ServerB.
- [x] A frame with the destination MAC address of RouterA.

### Explanation
* **Key point:** The destination **IP** is always the final host (**ServerB**), while the destination **MAC** is the **next hop** — here, the default gateway **RouterA** (because ServerB is on a different network, HostA ARPs for the gateway's MAC).
* **Why not the others:** The destination IP is never the gateway's IP, and HostA cannot frame directly to ServerB's MAC across a routed boundary.

---

## Question 21: The ARP Request
Which statement correctly describes an ARP request?

- [ ] It is a unicast sent directly to the destination's known MAC address.
- [x] It is a broadcast frame sent to FF-FF-FF-FF-FF-FF asking which device owns a given IPv4 address.
- [ ] It resolves a MAC address into an IPv4 address.
- [ ] It is used by IPv6 hosts to find neighbors.

### Explanation
* **Key point:** An **ARP request** is **broadcast** (destination MAC **FF-FF-FF-FF-FF-FF**) and asks, "Who has this IPv4 address?" The owner replies with a **unicast** ARP reply.
* **Why not the others:** ARP resolves IPv4→MAC (not the reverse), and IPv6 uses **ICMPv6 Neighbor Discovery (NDP)** instead of ARP.

---

## Question 22: ARP Spoofing
How does an ARP spoofing (poisoning) attack succeed?

- [ ] The attacker floods the switch MAC table until it fails open.
- [ ] The attacker encrypts ARP replies so victims cannot read them.
- [x] The attacker sends forged ARP replies claiming its own MAC for another device's IP, so victims store the wrong mapping.
- [ ] The attacker disables the ARP cache timer on the router.

### Explanation
* **Key point:** A threat actor replies to ARP requests with **its own MAC** for an IP it doesn't own (often the **default gateway**). Victims trust the reply and update their ARP table, sending traffic to the attacker — a man-in-the-middle attack. Dynamic ARP Inspection (DAI) mitigates it.
* **Why not the others:** Flooding the MAC table is a separate attack (CAM overflow); ARP isn't encrypted; the cache timer isn't what's exploited.

---

## Question 23: ARP Failure
What happens if no device responds to a host's ARP request for a destination on the local network?

- [ ] The host broadcasts the data packet to all devices anyway.
- [ ] The host sends the packet to the default gateway.
- [x] The packet is dropped, because a frame cannot be built without a destination MAC address.
- [ ] The host installs a static ARP entry pointing to itself.

### Explanation
* **Key point:** Without an ARP reply, the host has no destination MAC, so it **cannot encapsulate** the packet into a frame and **drops** it.
* **Why not the others:** Data isn't broadcast; the gateway is only used when the destination is on a *different* network; no self-pointing static entry is created.
