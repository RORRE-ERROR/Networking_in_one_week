# Chapter 2 Quiz — Network Protocols and Network Operating Systems

> Exam-style questions. Answers are marked and explained. Cover the explanations and test yourself first, then check.

## Question 1: Message delivery options
A host needs to send the same data to a specific, predefined set of receivers at the same time — but not to every device on the network. Which delivery option is this?

- [ ] unicast
- [x] multicast
- [ ] broadcast
- [ ] anycast

### Explanation
* **Key point:** Multicast delivers to a *specially defined group* of hosts, not one host and not everyone.
* **Why not the others:** Unicast is one-to-one (a single host); broadcast goes to *all* hosts on the local network. Anycast is not one of the three basic delivery options in this chapter.

---

## Question 2: Message timing
Which message-timing mechanism defines **how much** information can be sent and the **speed** at which it can be delivered, so the receiver is not overwhelmed?

- [x] flow control
- [ ] response timeout
- [ ] access method
- [ ] encapsulation

### Explanation
* **Key point:** Flow control manages the volume and rate of data between sender and receiver.
* **Why not the others:** Response timeout governs how long to wait for a reply; access method determines *when* a device may send; encapsulation is a formatting rule, not a timing rule.

---

## Question 3: Encoding
What is the purpose of **encoding** in network communication?

- [ ] to divide a large message into smaller segments
- [x] to convert information into another form that is acceptable for transmission
- [ ] to add source and destination addresses to the message
- [ ] to determine when a device is allowed to transmit

### Explanation
* **Key point:** Encoding converts information into a transmittable form (e.g., bits/signals on the media); **decoding** reverses it at the receiver.
* **Why not the others:** Splitting a message is *segmentation*; adding addresses is *encapsulation/formatting*; deciding when to send is the *access method*.

---

## Question 4: OSI to TCP/IP mapping (Choose three.)
Which three OSI layers map to the single **Application** layer of the TCP/IP model? (Choose three.)

- [x] Application
- [x] Presentation
- [x] Session
- [ ] Transport
- [ ] Network
- [ ] Data Link

### Explanation
* **Key point:** The OSI **Application + Presentation + Session** layers all collapse into the TCP/IP **Application** layer.
* **Why not the others:** OSI Transport ↔ TCP/IP Transport (1:1); OSI Network ↔ TCP/IP Internet (1:1); OSI Data Link + Physical ↔ TCP/IP Network Access.

---

## Question 5: OSI layer matching
Match each OSI layer to its function.

| OSI Layer | Function |
| :--- | :--- |
| **Transport** | End-to-end delivery using ports; segmentation and reliability |
| **Network** | Logical addressing (IP) and routing between networks |
| **Data Link** | Framing and MAC addressing on the local link |
| **Physical** | Transmission of bits as signals on the media |

### Explanation
* **Key point:** Layer 4 = Transport (segments/ports), Layer 3 = Network (packets/IP/routing), Layer 2 = Data Link (frames/MAC), Layer 1 = Physical (bits/signals).
* **Memory aid:** Top-to-bottom (7→1): *All People Seem To Need Data Processing.*

---

## Question 6: PDU at the Transport layer
What is the name of the PDU at the **Transport** layer of the TCP/IP model?

- [ ] packet
- [ ] frame
- [x] segment
- [ ] bits

### Explanation
* **Key point:** Transport layer PDU = **segment**.
* **Why not the others:** Packet = Internet/Network layer (L3); frame = Network Access/Data Link (L2); bits = Physical (L1). Memory aid top→bottom: Data, **S**egment, **P**acket, **F**rame, **B**its.

---

## Question 7: Encapsulation order (Choose the correct sequence.)
As application data is sent down the protocol stack, in what order is it encapsulated?

- [ ] data → packet → segment → frame → bits
- [x] data → segment → packet → frame → bits
- [ ] data → frame → packet → segment → bits
- [ ] bits → frame → packet → segment → data

### Explanation
* **Key point:** Going *down* the stack: the Transport layer makes a **segment**, the Internet layer makes a **packet**, the Network Access layer makes a **frame**, and the Physical layer sends **bits**.
* **Why not the others:** Packet always comes *after* segment (L4 before L3 on the way down) and *before* frame. De-encapsulation at the receiver runs in reverse.

---

## Question 8: Segmentation and multiplexing (Choose two.)
What are two benefits of segmenting data and multiplexing conversations on a network? (Choose two.)

- [x] It increases the reliability of network communications.
- [x] It allows many different conversations to be interleaved on the network.
- [ ] It removes the need for addressing at the network layer.
- [ ] It guarantees that data is transmitted as one uninterrupted stream of bits.
- [ ] It eliminates the need for de-encapsulation at the receiver.

### Explanation
* **Key point:** Segmentation breaks data into smaller pieces (so only a lost *piece* is re-sent, improving reliability), and multiplexing interleaves the pieces of separate conversations on the same link.
* **Why not the others:** Addressing is still required; sending one uninterrupted stream is exactly the *problem* segmentation avoids; the receiver must still de-encapsulate.

---

## Question 9: Same-network addressing
PC1 sends data to PC2, and both are on the **same** Ethernet network. What does PC1 use as the destination Layer 2 address?

- [ ] the MAC address of the default gateway
- [x] the MAC address of PC2
- [ ] the IP address of PC2
- [ ] the broadcast MAC address FFFF.FFFF.FFFF

### Explanation
* **Key point:** On the same local network, the frame is delivered directly, so the destination MAC is the **actual destination host's (PC2's) MAC**.
* **Why not the others:** The gateway's MAC is used only when the destination is on a *different* network; an IP address is a Layer 3 address, not a Layer 2 address; a broadcast MAC would be used only for a broadcast, not a directed unicast.

---

## Question 10: Remote-network addressing (Choose two.)
HostA wants to contact ServerB, which is located on a different network reached through RouterA (HostA's default gateway). Which two statements correctly describe the addressing HostA generates? (Choose two.)

- [ ] A packet with the destination IP address of RouterA.
- [x] A packet with the destination IP address of ServerB.
- [ ] A frame with the destination MAC address of ServerB.
- [x] A frame with the destination MAC address of RouterA.
- [ ] A frame with the destination MAC address of the switch.

### Explanation
* **Key point:** The destination **IP** is always the *final* destination (ServerB) and stays the same end-to-end. The destination **MAC** is the *next hop* — here, the default gateway RouterA — because the destination is on a different network.
* **Why not the others:** The packet's destination IP is never the gateway; the frame's destination MAC is never ServerB while the data is still on HostA's local link (that would only happen on ServerB's own network); switches are transparent at Layer 2 and don't become the frame's destination MAC.

---

## Question 11: IP vs MAC behavior along a path
As a packet is forwarded hop-by-hop from source to a remote destination, what happens to the Layer 3 and Layer 2 addresses?

- [ ] Both the IP and MAC addresses change at every hop.
- [ ] Both the IP and MAC addresses stay the same end-to-end.
- [x] The IP addresses stay the same end-to-end while the MAC addresses change at every hop.
- [ ] The MAC addresses stay the same end-to-end while the IP addresses change at every hop.

### Explanation
* **Key point:** Source/destination **IP** are end-to-end (unchanged); each router builds a **new frame** with new source/destination **MAC** addresses for the next link.
* **Analogy:** IP = the mailing address on the envelope (start to finish); MAC = the local hand-off that changes at each leg.

---

## Question 12: Standards organizations
Which standards organization is responsible for the **global allocation of IP addresses and port numbers**?

- [ ] IEEE
- [ ] ISO
- [x] IANA
- [ ] IETF

### Explanation
* **Key point:** IANA (Internet Assigned Numbers Authority) allocates IP addresses, port numbers, and protocol numbers.
* **Why not the others:** IEEE defines LAN/WAN standards like 802.3 and 802.11; ISO created the OSI model; IETF develops Internet standards through RFCs.

---

## Question 13: ISO vs OSI
Which statement about standards organizations is correct?

- [ ] The IETF created the OSI reference model.
- [x] The ISO created the OSI reference model.
- [ ] IEEE assigns global IP addresses.
- [ ] IANA publishes the 802.11 wireless LAN standards.

### Explanation
* **Key point:** The **ISO** (International Organization for Standardization) created the **OSI** model — easy to confuse because the names look alike, but ISO is the *organization* and OSI is the *model*.
* **Why not the others:** IETF produces RFCs (TCP/IP standards); IANA assigns IP addresses/ports; the 802.11 standard is from **IEEE**, not IANA.

---

## Question 14: CLI access methods
Which CLI access method provides **out-of-band** access and does **not** require any networking services to be configured on the device?

- [x] console
- [ ] Telnet
- [ ] SSH
- [ ] HTTP

### Explanation
* **Key point:** The console port is an out-of-band management port — ideal for initial configuration or recovery because it works even when no network services are running.
* **Why not the others:** Telnet and SSH are *in-band* and require active networking services; HTTP is not one of the listed CLI access methods.

---

## Question 15: Telnet vs SSH
Why is SSH preferred over Telnet for remotely managing a Cisco device?

- [ ] SSH is an out-of-band method, while Telnet is in-band.
- [ ] SSH does not require any networking services to be configured.
- [x] SSH encrypts the session and provides stronger authentication, while Telnet sends data in clear text.
- [ ] SSH uses the console port, while Telnet uses a virtual interface.

### Explanation
* **Key point:** SSH encrypts session data (including passwords) and offers stronger authentication; Telnet transmits everything in plaintext.
* **Why not the others:** Both SSH and Telnet are *in-band* and require networking services; both use a virtual (VTY) interface, not the console port.

---

## Question 16: Command modes and prompts
A technician sees the prompt `S1(config-if)#`. Which mode is the device in?

- [ ] user EXEC mode
- [ ] privileged EXEC mode
- [ ] global configuration mode
- [x] interface configuration mode

### Explanation
* **Key point:** `(config-if)#` indicates **interface configuration** mode (e.g., after `interface vlan 1`).
* **Why not the others:** `>` = user EXEC; `#` = privileged EXEC; `(config)#` = global config; `(config-line)#` = line config.

---

## Question 17: Moving between modes
Which command moves a device from **privileged EXEC** mode into **global configuration** mode?

- [ ] `enable`
- [x] `configure terminal`
- [ ] `line console 0`
- [ ] `interface vlan 1`

### Explanation
* **Key point:** `configure terminal` (often `conf t`) enters global config mode from privileged EXEC.
* **Why not the others:** `enable` goes from user EXEC *up to* privileged EXEC; `line console 0` and `interface vlan 1` move from global config *down into* sub-configuration modes.

---

## Question 18: Securing privileged EXEC
Which command sets an **encrypted** password to restrict access to privileged EXEC mode?

- [ ] `S1(config)# enable password class`
- [x] `S1(config)# enable secret class`
- [ ] `S1(config)# service password-encryption`
- [ ] `S1(config-line)# password class`

### Explanation
* **Key point:** `enable secret` stores an encrypted password for privileged EXEC and takes precedence over `enable password`.
* **Why not the others:** `enable password` is plaintext (legacy); `service password-encryption` only weakly encrypts *other* plaintext passwords already in the config; a line `password` protects a console/VTY line, not privileged EXEC.

---

## Question 19: Saving configuration
A network administrator changed the hostname and configured passwords, but after a reboot every change was gone. What did the administrator most likely forget to do?

- [ ] activate the interface with `no shutdown`
- [ ] enter global configuration mode
- [x] copy the running-config to the startup-config
- [ ] configure a default gateway

### Explanation
* **Key point:** Changes take effect immediately in **running-config (RAM)** but are lost on reboot unless saved with `copy running-config startup-config` (to **NVRAM**).
* **Why not the others:** `no shutdown`, global config, and a default gateway don't make a configuration persist across reboots — only saving to NVRAM does.

---

## Question 20: Banner command syntax
Refer to the command: `S1(config)# banner motd #Authorized Access Only!#`. What is the purpose of the `#` characters?

- [ ] They encrypt the banner text.
- [x] They act as delimiting characters that mark the start and end of the message.
- [ ] They are required to be the literal symbol that begins and ends every banner.
- [ ] They set the privilege level required to view the banner.

### Explanation
* **Key point:** The `#` is a **delimiting character** — any character not used inside the message can be chosen; the banner text sits between the opening and closing delimiter.
* **Why not the others:** It performs no encryption, is not a mandatory fixed symbol, and has nothing to do with privilege levels.

---

## Question 21: SVI for remote management
A Layer 2 switch needs to be reachable for remote management. Which set of commands correctly configures the management interface?

- [ ] `S1(config)# ip address 192.168.1.2 255.255.255.0` (in global config)
- [x] `S1(config)# interface vlan 1` → `ip address 192.168.1.2 255.255.255.0` → `no shutdown`
- [ ] `S1(config)# interface vlan 1` → `ip address 192.168.1.2 255.255.255.0` (no `no shutdown`)
- [ ] `S1(config)# hostname 192.168.1.2`

### Explanation
* **Key point:** A switch gets a manageable IP on its **SVI** (e.g., VLAN 1); the interface must be brought up with `no shutdown`, since interfaces are often administratively down by default.
* **Why not the others:** You can't assign an IP directly in global config; omitting `no shutdown` leaves the SVI down so management fails; `hostname` sets a name, not an address.

---

## Question 22: IOS storage and operation (Choose two.)
Which two statements about how Cisco IOS uses device memory are correct? (Choose two.)

- [x] The IOS image is stored in flash, which provides non-volatile storage.
- [x] IOS is copied into RAM at boot and runs from RAM while the device operates.
- [ ] IOS runs directly from flash and is never copied to RAM.
- [ ] The startup-config is stored in RAM and is lost when the device reboots.
- [ ] The running-config is stored in NVRAM and survives a reboot.

### Explanation
* **Key point:** The IOS image lives in **flash** (non-volatile) and is copied into **RAM** to run; the **startup-config** is in **NVRAM** (persistent) and the **running-config** is in **RAM** (volatile).
* **Why not the others:** IOS does not run straight from flash; the running-config (not startup) is the one in volatile RAM, and it's the one lost on reboot if not saved.

---

## Question 23: Why a frame is sent to the gateway
When a host determines that the destination IP address is on a **different network**, why does it forward the frame to its default gateway?

- [ ] because the destination IP address must be changed to the gateway's IP address
- [x] because it cannot deliver the frame directly to a host on a remote network, so it hands it to the router for forwarding
- [ ] because broadcasts are required to reach remote networks
- [ ] because MAC addresses are routable across networks

### Explanation
* **Key point:** Layer 2 delivery only works on the local link, so for a remote destination the host sends the frame to the **router (default gateway)**, which then forwards the packet toward the destination network.
* **Why not the others:** The destination IP stays as the final host (it is *not* changed to the gateway); remote delivery is unicast, not broadcast; MAC addresses are *not* routable — only IP addresses are.

---

## Question 24: Activating an interface
After configuring an IP address on a switch's VLAN 1 interface, an administrator notices the management interface is still down. Which command activates it?

- [ ] `S1(config-if)# shutdown`
- [x] `S1(config-if)# no shutdown`
- [ ] `S1(config-if)# enable`
- [ ] `S1(config-if)# service password-encryption`

### Explanation
* **Key point:** `no shutdown` administratively enables an interface; interfaces are frequently down by default until explicitly brought up.
* **Why not the others:** `shutdown` *disables* the interface; `enable` is a privileged EXEC command, not an interface command; `service password-encryption` is a global security command unrelated to interface state.
