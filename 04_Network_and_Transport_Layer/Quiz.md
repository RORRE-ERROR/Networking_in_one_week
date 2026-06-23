# Chapter 4 Quiz — Network Layer and Transport Layer

> Exam-style questions. Answers are marked and explained. Cover the explanations and test yourself first.

## Question 1: IP characteristics
Which two characteristics describe the Internet Protocol (IP)? (Choose two.)

- [x] connectionless
- [x] best-effort delivery
- [ ] guaranteed delivery
- [ ] media dependent
- [ ] connection-oriented

### Explanation
* **Key point:** IP is **connectionless** (no session set up before sending) and **best-effort** (delivery is not guaranteed). It is also **media independent**.
* **Why not the others:** Guaranteed delivery and connection orientation are features of **TCP** at the transport layer, not IP. IP is media *independent*, not dependent.

---

## Question 2: Reliability responsibility
A network technician claims that IP guarantees that every packet is delivered in order. Why is this statement incorrect?

- [ ] IP guarantees delivery but not ordering.
- [x] IP is best-effort and connectionless; reliable, in-order delivery is provided by TCP.
- [ ] IP guarantees ordering but not delivery.
- [ ] IP only guarantees delivery on local networks.

### Explanation
* **Key point:** IP itself makes **no delivery or ordering guarantees** — it is best-effort. Reliability, retransmission, and sequencing are added by **TCP** at the transport layer.
* **Why not the others:** IP provides neither guaranteed delivery nor guaranteed ordering anywhere, local or remote.

---

## Question 3: Why IPv6
Which two of the following are advantages of IPv6 over IPv4? (Choose two.)

- [ ] smaller address space that is easier to manage
- [x] a vastly larger 128-bit address space
- [x] a simplified header with fewer fields
- [ ] mandatory use of NAT for all hosts
- [ ] a required header checksum for reliability

### Explanation
* **Key point:** IPv6 uses **128-bit addresses** (huge address space) and a **simplified header with fewer fields** for faster processing.
* **Why not the others:** IPv6 *eliminates* the need for NAT and *removes* the header checksum (it is not required for reliability — Layers 2 and 4 handle that).

---

## Question 4: IPv6 header field
In IPv6, which field performs the same role as the TTL field in IPv4 — being decremented at each router and causing the packet to be dropped at zero?

- [ ] Traffic Class
- [ ] Flow Label
- [x] Hop Limit
- [ ] Next Header

### Explanation
* **Key point:** The IPv4 **TTL** field is renamed **Hop Limit** in IPv6. It is decremented by each router; when it reaches zero the packet is dropped and an ICMPv6 Time Exceeded message is returned.
* **Why not the others:** Next Header identifies the next encapsulated protocol; Traffic Class and Flow Label relate to QoS, not hop counting.

---

## Question 5: How a host routes
A host needs to send a packet to a device on a different IP network. To which device does the host send the packet first?

- [ ] directly to the destination host
- [ ] to the DNS server
- [x] to its default gateway
- [ ] to the loopback address

### Explanation
* **Key point:** When the destination is on a **remote** network, the host forwards the packet to its **default gateway** (the local router interface), which routes it onward.
* **Why not the others:** A host can only deliver directly to devices on its *own* network. DNS resolves names but does not forward packets; the loopback tests the local stack.

---

## Question 6: Local vs. remote decision (IPv4)
How does an IPv4 host determine whether a destination address is on its own local network?

- [ ] It pings the destination first.
- [x] It compares its own IP address and subnet mask against the destination IP address.
- [ ] It queries the default gateway for the answer.
- [ ] It checks the destination's MAC address.

### Explanation
* **Key point:** The host uses its **own subnet mask** with its **own IPv4 address** and the **destination IPv4 address** to determine whether both are in the same network. Same network = local delivery; different = send to the default gateway.
* **Why not the others:** This decision happens *before* any ping; MAC addresses are Layer 2 and don't determine the network; the gateway isn't consulted to make this decision.

---

## Question 7: Default gateway troubleshooting
Refer to the scenario. A user's PC can successfully ping other PCs on the same subnet and can ping its own loopback address, but it cannot reach any website on the Internet. What is the most likely cause?

- [ ] The NIC is faulty.
- [ ] The TCP/IP stack is not installed.
- [x] The default gateway is missing or misconfigured on the PC.
- [ ] The switch port is shut down.

### Explanation
* **Key point:** Successful local pings and loopback pings prove the NIC, cabling, and TCP/IP stack work. The failure is specifically to **remote** networks — the hallmark of a missing or wrong **default gateway**.
* **Why not the others:** A faulty NIC or missing stack would block the loopback/local pings too; a shut switch port would block local connectivity as well.

---

## Question 8: Routing table entry types
A router's routing table contains three categories of route entries. Match each entry type to its description.

| Route type | Description |
| :--- | :--- |
| **Directly connected** | A network attached to one of the router's own active interfaces. |
| **Remote** | A network reached through another router, learned statically or via a dynamic protocol. |
| **Default** | The route used when no other (longer) match exists in the table. |

### Explanation
* **Key point:** Directly connected routes (code **C**) come from the router's own interfaces; remote routes are learned (static **S** or dynamic **O/D**, etc.); the default route (`0.0.0.0/0` or `::/0`) is the gateway of last resort.

---

## Question 9: Missing directly connected route
A network administrator configures interface fa0/0 with `ip address 172.16.1.254 255.255.255.0`, but `show ip route` does not display the directly connected network. What is the most likely cause?

- [ ] The configuration needs to be saved first.
- [ ] No packets have been sent to that network yet.
- [ ] The subnet mask is wrong.
- [x] The interface fa0/0 has not been activated with no shutdown.

### Explanation
* **Key point:** A directly connected network appears in the routing table only when the interface is both correctly addressed **and** enabled. If it's still administratively down, it won't show. Issue **`no shutdown`**.
* **Why not the others:** Saving config isn't required to populate the running routing table; traffic isn't needed to install a connected route; the /24 mask shown is valid for that address.

---

## Question 10: Default route notation
Which destination address and mask represent an IPv4 default route?

- [ ] 255.255.255.255 255.255.255.255
- [ ] 127.0.0.1 255.0.0.0
- [x] 0.0.0.0 0.0.0.0
- [ ] 192.168.0.0 255.255.0.0

### Explanation
* **Key point:** The IPv4 default route is `0.0.0.0 0.0.0.0` (`0.0.0.0/0`), matching *any* destination but only used when no more specific (longer-prefix) route matches. The IPv6 equivalent is `::/0`.
* **Why not the others:** `127.0.0.1` is loopback; the others are specific networks/hosts.

---

## Question 11: Static vs. dynamic
When is a dynamic routing protocol more beneficial than static routing?

- [ ] on a small stub network with a single exit point
- [x] on a network where the topology changes frequently
- [ ] when an administrator wants full manual control of every route
- [ ] on a network that will never grow

### Explanation
* **Key point:** **Dynamic** routing automatically re-converges when the topology changes, so it shines on large or frequently changing networks.
* **Why not the others:** Small, stable, or stub networks are well served by simple static routes, which never auto-update.

---

## Question 12: ICMP message for unreachable destination
A router receives a packet for a destination it cannot deliver. Which ICMP message does it send back to the source?

- [ ] Echo Reply
- [ ] Redirect
- [x] Destination Unreachable
- [ ] Time Exceeded

### Explanation
* **Key point:** When a host or gateway cannot deliver a packet, it returns an ICMP **Destination Unreachable** message to inform the source the destination or service is unreachable.
* **Why not the others:** Echo Reply answers a ping; Redirect suggests a better next-hop; Time Exceeded is for TTL expiry.

---

## Question 13: traceroute mechanics
Which ICMP message makes traceroute able to discover each router along a path?

- [ ] Destination Unreachable
- [ ] Echo Reply
- [x] Time Exceeded
- [ ] Source Quench

### Explanation
* **Key point:** Traceroute sends packets with increasing TTL values. Each router that decrements TTL to **0** drops the packet and returns a **Time Exceeded** message, revealing its identity hop by hop.
* **Why not the others:** Echo Reply only comes from the final destination; Destination Unreachable signals an undeliverable packet, not a normal hop discovery.

---

## Question 14: Reading a traceroute
An administrator runs `tracert` from PC1 to PC2. The first hop (the local router R1) replies normally, but every subsequent hop returns `* * *`. Where should troubleshooting begin?

- [ ] at PC1
- [ ] at PC2
- [x] at R1 / the link beyond R1
- [ ] at the DNS server

### Explanation
* **Key point:** Troubleshooting starts at the **last hop that responded successfully** — here R1. The `* * *` entries indicate lost/unanswered packets beyond that point, so the fault lies just past R1.
* **Why not the others:** PC1 clearly works (it reached R1); PC2 was never reached; DNS is unrelated to a numeric-address trace.

---

## Question 15: Connectivity test order
What is the correct order of `ping` tests when verifying a host's connectivity from local to remote?

- [ ] remote host, default gateway, loopback
- [x] loopback, default gateway, remote host
- [ ] default gateway, loopback, remote host
- [ ] remote host, loopback, default gateway

### Explanation
* **Key point:** Work outward: **loopback** (own stack), then **default gateway** (local router), then a **remote host** (end-to-end). Each step isolates the failure to a wider scope.

---

## Question 16: Role of the transport layer
Which two functions are performed by the transport layer? (Choose two.)

- [ ] assigning IP addresses to hosts
- [x] segmenting data and reassembling it into communication streams
- [x] identifying the correct application for each stream using port numbers
- [ ] forwarding packets between networks
- [ ] resolving domain names to IP addresses

### Explanation
* **Key point:** The transport layer **segments** application data, reassembles segments into the right streams, and uses **port numbers** to direct each stream to the correct application.
* **Why not the others:** IP addressing and forwarding are Layer 3 (network layer); name resolution is DNS, an application-layer service.

---

## Question 17: Sockets and ports
Which statement about source and destination ports is correct?

- [ ] Both the source and destination ports are well-known ports.
- [x] The destination port identifies the requested service; the source port is randomly generated to track the conversation.
- [ ] The source port identifies the requested service; the destination port is random.
- [ ] Ports are used only by UDP, not TCP.

### Explanation
* **Key point:** The **destination port** tells the server which service is wanted (e.g., 80 = HTTP). The **source port** is randomly chosen by the sender so it can match replies and run many conversations at once. A **socket** = IP address + port.
* **Why not the others:** Only the destination is typically well-known; the role assignment in option 3 is reversed; both TCP and UDP use ports.

---

## Question 18: Well-known ports matching
Match each service to its well-known port number.

| Service | Port |
| :--- | :--- |
| HTTP | **80** |
| HTTPS | **443** |
| DNS | **53** |
| SMTP | **25** |
| Telnet | **23** |
| TFTP | **69** |

### Explanation
* **Key point:** Memorize the high-yield ports: HTTP **80**, HTTPS **443**, DNS **53**, SMTP **25**, Telnet **23**, TFTP **69**. Also know FTP **20/21**, SSH **22**, DHCP **67/68**, POP3 **110**, IMAP **143**.

---

## Question 19: TCP 3-way handshake
Place the steps of the TCP three-way handshake in the correct order.

- [ ] ACK → SYN → SYN, ACK
- [ ] SYN, ACK → SYN → ACK
- [x] SYN → SYN, ACK → ACK
- [ ] SYN → ACK → SYN, ACK

### Explanation
* **Key point:** The client sends **SYN**; the server replies with **SYN, ACK** (one segment carrying both flags); the client finishes with **ACK**. The session is then established.
* **Why not the others:** The handshake always begins with the client's SYN, and the server's reply combines SYN and ACK into the middle segment.

---

## Question 20: Closing a TCP connection
Which control flag must be set to begin closing a TCP connection?

- [ ] SYN
- [ ] RST
- [x] FIN
- [ ] PSH

### Explanation
* **Key point:** To close a connection, the **FIN** (Finish) flag is set. Each one-way session is torn down with a FIN + ACK pair.
* **Why not the others:** SYN starts a connection; RST abruptly resets it; PSH pushes buffered data to the application but doesn't close the session.

---

## Question 21: Windowing and flow control
What does the TCP window size field control?

- [ ] the maximum size of an IP packet
- [x] the amount of data a source can send before it must receive an acknowledgment
- [ ] the number of hops a segment can traverse
- [ ] the source port range

### Explanation
* **Key point:** The **window size** is the number of bytes a sender may transmit before an **acknowledgment** is required. It enables **flow control** and management of lost data — a smaller window tells the sender to slow down.
* **Why not the others:** Hop counting is TTL/Hop Limit; packet size and ports are unrelated fields.

---

## Question 22: TCP retransmission
What does TCP do when it does not receive an acknowledgment for sent data within the expected time?

- [ ] It closes the connection immediately.
- [ ] It switches to UDP.
- [x] It returns to the last acknowledged point and retransmits the data from there.
- [ ] It ignores the loss and continues.

### Explanation
* **Key point:** If no ACK arrives within the timeout, TCP goes **back to the last ACK number received** and **retransmits** from that point forward — the heart of reliable delivery.
* **Why not the others:** TCP doesn't drop to UDP or ignore loss; it doesn't close the session simply because one ACK was late.

---

## Question 23: TCP vs UDP overhead
How many bytes of header overhead does a TCP segment carry compared to a UDP segment?

- [ ] TCP 8 bytes, UDP 20 bytes
- [x] TCP 20 bytes, UDP 8 bytes
- [ ] TCP 16 bytes, UDP 16 bytes
- [ ] TCP 8 bytes, UDP 8 bytes

### Explanation
* **Key point:** TCP carries **20 bytes** of header overhead (it tracks sequencing, ACKs, windowing); UDP carries only **8 bytes**, making it lightweight and fast.

---

## Question 24: Choosing the protocol
An engineer is selecting a transport protocol for a real-time Voice over IP (VoIP) application that can tolerate occasional lost packets but cannot tolerate delay. Which protocol is the best fit, and why?

- [ ] TCP, because it guarantees every packet arrives
- [x] UDP, because its low overhead and lack of retransmission minimize delay
- [ ] TCP, because it provides flow control
- [ ] UDP, because it reorders packets for playback

### Explanation
* **Key point:** Real-time voice prefers **UDP** — its **low overhead** and absence of retransmission/handshake keep latency low. A little loss is acceptable; delay is not.
* **Why not the others:** TCP's retransmissions and handshake add delay, which is worse for live voice; UDP does **not** reorder packets.

---

## Question 25: UDP applications
Which two applications typically use UDP? (Choose two.)

- [ ] File Transfer Protocol (FTP)
- [x] Domain Name System (DNS)
- [ ] HyperText Transfer Protocol (HTTP)
- [x] Trivial File Transfer Protocol (TFTP)
- [ ] Simple Mail Transfer Protocol (SMTP)

### Explanation
* **Key point:** **DNS** (port 53 queries) and **TFTP** (port 69) use **UDP** for low overhead. Other UDP examples: VoIP, video streaming, SNMP, DHCP.
* **Why not the others:** FTP, HTTP, and SMTP need reliable, in-order delivery and therefore use **TCP**.

---

## Question 26: UDP characteristics
Which statement accurately describes UDP?

- [ ] It establishes a session before sending data.
- [ ] It guarantees in-order delivery.
- [x] It is connectionless and provides no flow control or retransmission.
- [ ] It uses a three-way handshake.

### Explanation
* **Key point:** UDP is **connectionless**, with **no flow control, no acknowledgments, no reordering, and no retransmission** — just fast best-effort delivery of datagrams.
* **Why not the others:** Sessions, in-order guarantees, and the three-way handshake are all **TCP** features.

---

## Question 27: Loopback test
A technician wants to verify that the TCP/IP protocol stack is functioning on the local host before testing the network. Which command should be used?

- [ ] ping 192.168.1.1
- [ ] tracert 8.8.8.8
- [x] ping 127.0.0.1
- [ ] ping 0.0.0.0

### Explanation
* **Key point:** Pinging the **loopback** address **127.0.0.1** (or **::1** in IPv6) tests the host's own TCP/IP stack without touching the physical network.
* **Why not the others:** Pinging the gateway or a remote host tests connectivity beyond the local stack; `0.0.0.0` is not a valid loopback test target.
