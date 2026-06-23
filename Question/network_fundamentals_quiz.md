# Network Fundamentals Quiz Review

## Question 26: Modern Switch Connections
A network administrator is connecting two modern switches using a straight-through cable. The switches are new and have never been configured. Which three statements are correct about the final result of the connection? (Choose three.)

- [x] The link between the switches will work at the fastest speed that is supported by both switches.
- [x] The link between switches will work as full-duplex.
- [ ] The connection will not be possible unless the administrator changes the cable to a crossover cable.
- [ ] The duplex capability has to be manually configured because it cannot be negotiated.
- [ ] If both switches support different speeds, they will each work at their own fastest speed.
- [x] The auto-MDIX feature will configure the interfaces eliminating the need for a crossover cable.

### Explanation
* **Auto-negotiation:** Modern switches automatically negotiate the highest shared speed and full-duplex capabilities. A link must always synchronize to the same operational speed on both sides.
* **Auto-MDIX:** Automatically detects the cable connection type (straight-through vs. crossover) and configures the physical interface internal polarity accordingly, enabling like-devices to connect using standard straight-through cabling.

---

## Question 27: Signal Attenuation
What does the term "attenuation" mean in data communication?

- [ ] leakage of signals from one cable pair to another
- [ ] time for a signal to reach its destination
- [x] loss of signal strength as distance increases
- [ ] strengthening of a signal by a networking device

### Explanation
* **Attenuation:** As electromagnetic or optical signals propagate through a transmission medium, internal resistance, scattering, and physical impairments cause the signal amplitude and energy to weaken over distance.
* **Distinctions:** Signal leakage across pairs is called *crosstalk*, propagation delay is called *latency*, and signal boost is known as *amplification* or *regeneration*.

---

## Question 28: Network Topology Cable Selection
Refer to the exhibit. The PC is connected to the console port of the switch. All the other connections are made through FastEthernet links. Which types of UTP cables can be used to connect the devices?

*(Topology: [PC] --(1)-- [Switch] --(2)-- [Router] --(3)-- [Router])*

- [x] 1 - rollover, 2 - straight-through, 3 - crossover
- [ ] 1 - rollover, 2 - crossover, 3 - straight-through
- [ ] 1 - crossover, 2 - rollover, 3 - straight-through
- [ ] 1 - crossover, 2 - straight-through, 3 - rollover

### Explanation
* **Link 1 (Rollover):** Connects a host computer's serial COM port to a networking device's standard console port for local out-of-band management.
* **Link 2 (Straight-through):** Connects structurally "unlike" devices functioning at different OSI reference model layers (Switch at Layer 2, Router at Layer 3).
* **Link 3 (Crossover):** Connects structurally "like" peer interfaces directly (Router to Router), crossing the transmit (Tx) and receive (Rx) pairs.

---

## Question 29: RJ-45 Connector Termination
Refer to the exhibit. What is wrong with the displayed termination?

- [ ] The wires are too thick for the connector that is used.
- [ ] The wrong type of connector is being used.
- [x] The untwisted length of each wire is too long.
- [ ] The woven copper braid should not have been removed.

### Explanation
* **Termination Quality:** Stripping the cable sheath back too far leaves individual conductors exposed and untwisted. This removes the natural electromagnetic noise immunity provided by the twisted pairs, exposing the transmission path to extensive *crosstalk* and physical instability. The outer protective jacket must always extend deep enough to be permanently secured by the connector's internal plastic strain-relief wedge.

---

## Question 31: Fiber-Optic vs. Copper Mediums
What makes fiber preferable to copper cabling for interconnecting buildings? (Choose three.)

- [x] greater distances per cable run
- [ ] durable connections
- [x] greater bandwidth potential
- [ ] lower installation cost
- [x] limited susceptibility to EMI/RFI
- [ ] easily terminated

### Explanation
* **Distance & Bandwidth:** Fiber uses optical light pulses rather than electrical currents, escaping physical copper limits (~100 meters max) to travel multiple kilometers with immense data bandwidth capabilities.
* **EMI/RFI Immunity:** Glass core constructions do not generate or react to electromagnetic fields, providing total immunity from outside industrial ambient electrical noise, atmospheric interference, or lightning strikes across building boundaries.

---

## Question 33: Data Link Layer LLC Sublayer
Which two functions are performed at the LLC sublayer of the OSI data link layer? (Choose two.)

- [ ] Integrates various physical technologies.
- [ ] Performs data encapsulation.
- [ ] Controls the NIC responsible for sending and receiving data on the physical medium.
- [x] Places information in the frame that identifies which network layer protocol is being used for the frame.
- [x] Adds Layer 2 control information to network protocol data.

### Explanation
* **Logical Link Control (LLC):** The upper sublayer of Layer 2 acts as the software interface to the Network layer above. It injects specific control headers (such as the EtherType field) to clearly delineate whether an inbound payload contains an IPv4 packet, an IPv6 packet, or another network protocol, allowing multiple protocols to safely multiplex across the same underlying physical connection.
* **Media Access Control (MAC):** Framing structures, media access arbitration, hardware addressing, and physical network signaling integration are handled down in the lower MAC sublayer.

---

## Question 34: Layer 2 Frame Processing Host Actions
What action will occur if a host receives a frame with a destination MAC address it does not recognize?

- [ ] The host sends the frame to the switch to update the MAC address table.
- [ ] The host forwards the frame to all other hosts.
- [x] The host will discard the frame.
- [ ] The host forwards the frame to the router.

### Explanation
* **NIC Processing:** The network interface card (NIC) explicitly evaluates incoming Destination MAC frames. If the address matches neither its own unique hardware burned-in address (BIA) nor an active broadcast or multicast registration group it belongs to, it drops the frame instantly to protect upper CPU structures from unnecessary overhead processing.
