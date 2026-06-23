# Chapter 4 — Network Layer and Transport Layer

> **Day 2 of 7** · Estimated study time: ~4 hours
> *This chapter is about how data finds its way across networks (Layer 3, the "mailing system") and how two programs hold a reliable — or fast — conversation once it arrives (Layer 4, the "delivery agreement").*

## 🎯 What you'll be able to do after this chapter
- Explain the three core characteristics of IP (connectionless, best-effort, media independent) and why they matter.
- Compare the IPv4 and IPv6 packet headers and say *why* IPv6 was created.
- Describe how a **host** decides where to send a packet (local vs. remote) and what a default gateway is for.
- Read a router's routing table and identify directly connected, remote, and default routes.
- Use ICMP, `ping`, and `traceroute` to test and troubleshoot connectivity.
- Explain the role of the transport layer, port numbers, and sockets.
- Walk through the TCP 3-way handshake, sequencing, acknowledgment, and flow control.
- Decide when to use TCP vs. UDP for a given application.

---

## 1. The Network Layer (Layer 3)

**Intuition first:** if the data link layer (Chapter 3) is about getting a frame across *one* link, the **network layer** is about getting a packet across *many* links, all the way from a source host to a destination host on a possibly distant network. It is the postal service of the network — it doesn't care what truck, plane, or bike carries the letter, it just cares about the address on the envelope.

The network layer performs four basic processes:

1. **Addressing end devices** — every end device gets a unique **IP address** so it can be identified.
2. **Encapsulation** — the network layer takes the segment from the transport layer and wraps it in an **IP header** to form a *packet*.
3. **Routing** — it chooses a path and directs the packet toward a destination on another network.
4. **De-encapsulation** — when the packet reaches the destination host, the IP header is stripped off and the data is passed up to the transport layer.

The two network-layer protocols you must know are **IPv4** and **IPv6**.

### The three characteristics of IP

These three words come up constantly on the exam — memorize them with the analogy:

| Characteristic | Plain English | Analogy |
|---|---|---|
| **Connectionless** | No connection is set up with the destination before sending. | Dropping a letter in a mailbox without phoning the recipient first. |
| **Best Effort (unreliable)** | Delivery is *not* guaranteed; IP doesn't track or resend lost packets. | The post office tries its best but won't refund a lost letter — IP itself doesn't even notice. |
| **Media Independent** | IP works the same over copper, fiber, Wi-Fi, etc. | A letter's address works whether it travels by van, train, or plane. |

> 💡 **Exam tip:** "Reliability" is *not* IP's job — it is handled higher up by **TCP** at the transport layer. If a question describes "guaranteed, in-order delivery," that's TCP, not IP. IP is always *best-effort and connectionless.*

---

## 2. IPv4 and IPv6 Headers

You don't need to memorize every bit, but you should recognize the key fields and understand the *direction of change* from IPv4 to IPv6.

### IPv4 header (key fields)
- **Version** — set to 4.
- **Source IP Address / Destination IP Address** — 32-bit addresses.
- **Time to Live (TTL)** — decremented by 1 at each router; when it hits 0 the packet is dropped (prevents endless loops).
- **Protocol** — tells the receiver what's inside (e.g., 6 = TCP, 17 = UDP, 1 = ICMP).
- **Header Checksum** — error-checks the header.

### Why IPv6 exists — IPv4's three big problems
1. **IP address depletion** — only ~4.3 billion IPv4 addresses exist, and they ran out.
2. **Lack of end-to-end connectivity** — NAT hides internal hosts behind a shared public address, which breaks apps needing direct end-to-end reach.
3. **Increased network complexity** — NAT adds latency and makes troubleshooting harder.

### How IPv6 fixes them
- **Increased address space** — addresses are **128-bit** (vs. 32-bit), an astronomically larger pool.
- **Improved packet handling** — the IPv6 header is *simplified* with **fewer fields** (faster router processing).
- **Eliminates the need for NAT** — there are enough public addresses for every device.
- **Integrated security** — IPv6 natively supports authentication and privacy.

> 💡 **Exam tip:** The IPv6 header has **fewer fields** and is **fixed-length**. Notably, IPv6 has **no header checksum** (Layer 2 and Layer 4 already check errors) and the TTL field is renamed **Hop Limit** — same job, new name.

---

## 3. Routing — How a Host Decides Where to Send

**Intuition first:** before a host sends a packet, it asks one question: *"Is the destination on my own local network, or somewhere else?"* The answer changes who it hands the packet to.

A host can send a packet to three kinds of destinations:

| Destination | What it means | How to test |
|---|---|---|
| **Itself** | The loopback — tests the host's own TCP/IP stack. | `ping 127.0.0.1` (IPv4) or `ping ::1` (IPv6) |
| **Local host** | A device on the *same* local network. | Delivered directly via the switch. |
| **Remote host** | A device on a *different* network. | Sent to the **default gateway** for forwarding. |

### How a host decides: local or remote?
- **IPv4:** the host compares its **own IP + subnet mask** against the **destination IP**. If both fall in the same network, it's local; if not, it's remote.
- **IPv6:** the local router *advertises* the network prefix to all devices, so hosts learn what counts as "local."

If the destination is **remote**, the host forwards the packet to its **default gateway** — the router interface on the local network that routes traffic *off* the local network toward remote networks.

> 💡 **Exam tip:** A wrong or missing **default gateway** is the classic reason a host can reach *local* devices but not *remote* ones (e.g., can't reach the Internet). If local pings work but remote ones fail, suspect the default gateway. (See lab 10.3.5.)

---

## 4. Routing — How a Router Routes

A **router** is a device that connects multiple networks. Each router keeps a **routing table** holding three types of route entries:

1. **Directly connected networks** — networks attached to the router's own active interfaces. These appear automatically once the interface has an IP address *and is enabled* (`no shutdown`). Shown with code **C** (and **L** for the local interface address).
2. **Remote networks** — networks reached *through another router*. Learned either by **static** (manual) configuration or by a **dynamic routing protocol**. Shown with codes like **S** (static), **O** (OSPF), **D** (EIGRP).
3. **Default route** — the "gateway of last resort," used when there is **no better (longer) match** in the table. Written as `0.0.0.0/0` (IPv4) or `::/0` (IPv6). Shown with **S\*** when it's a static default.

> 💡 **Exam tip:** A directly connected network will **not** appear in the routing table if the interface is **not activated** (`no shutdown` was never issued) or has no IP/wrong mask. This is a favorite trap. (See checkpoint Q6.)

### Static vs. Dynamic routing

| | Static routing | Dynamic routing |
|---|---|---|
| Configured | Manually, by an admin | Automatically, via a protocol (OSPF, EIGRP, RIP) |
| On topology change | Does **not** update — admin must fix it | Routers re-converge automatically |
| Best for | Small / stable networks, stub links to an ISP | Larger networks with frequent changes |

Example static route command:
```
R1(config)# ip route 192.168.10.0 255.255.255.0 10.10.10.2
```
This says: "to reach `192.168.10.0/24`, send packets to next-hop `10.10.10.2`."

> 💡 **Exam tip:** When two routes match the same destination, the router prefers the **longest prefix match** (most specific). When two routes to the *same* prefix come from different sources, it prefers the one with the lowest **administrative distance (AD)**. A *floating static route* is a static route given a *higher* AD so it only activates as a backup when the primary fails.

---

## 5. ICMPv4 — Testing and Reporting

**Intuition first:** IP doesn't report problems itself (it's best-effort), so **ICMP** (Internet Control Message Protocol) is the messenger that carries status and error reports across an IP network.

Key ICMP message types to know:

| Message | When it's used |
|---|---|
| **Echo Request / Echo Reply** | Tests reachability. The `ping` command sends an Echo Request; a reachable host returns an Echo Reply. |
| **Destination Unreachable** | A host or router that *can't* deliver a packet tells the source the destination or service is unreachable. (e.g., Type 3, Code 1 = host unreachable.) |
| **Time Exceeded** | A router drops a packet whose **TTL reached 0** and reports it back. This is how `traceroute` discovers each hop — and how routing loops get caught. |
| **Redirect** | A router tells a host a better next-hop exists for a destination. |

### ping
`ping` is the go-to connectivity test. A good troubleshooting order is:
1. **Ping the loopback** (`127.0.0.1`) — confirms the local TCP/IP stack works.
2. **Ping the default gateway** — confirms reach to the local router.
3. **Ping a remote host** — confirms end-to-end reach.

### traceroute / tracert
`traceroute` (or `tracert` on Windows) lists the **hops** along the path to a destination. It works by sending packets with increasing TTL values — each router that drops a packet (TTL=0) replies with a Time Exceeded message, revealing itself.
- It sends **three** packets per hop and reports the **round-trip time (RTT)** for each.
- An asterisk `*` indicates a lost or unanswered packet.

> 💡 **Exam tip:** When troubleshooting with `traceroute`, begin at the **last hop that responded successfully** — the problem lies just beyond it. Also remember: both `ping` and `traceroute` rely on **ICMP**.

---

## 6. The Transport Layer (Layer 4)

**Intuition first:** the network layer gets a packet to the right *computer*. The **transport layer** gets the data to the right *program* on that computer, and decides how carefully to deliver it.

The transport layer:
- Establishes a temporary **communication session** between two applications.
- Performs **segmentation** (breaking data into segments) and reassembly into the right communication streams.
- Identifies the **correct application** for each stream (via port numbers).

Its two protocols are **TCP** (reliable) and **UDP** (fast/lightweight).

### Port numbers and sockets

A **port number** is a numeric identifier inside each segment that tracks a specific conversation and the service requested.
- **Destination port** — tells the *server* which service the client wants. Example: port **80** = HTTP.
- **Source port** — randomly generated by the *sending* device so it can match replies to the right conversation. This lets a host run many conversations at once.

A **socket** is the combination of an **IP address + a port number** (e.g., `192.168.1.10:80`). A pair of sockets uniquely identifies one specific conversation between two devices.

### Well-known ports (memorize these)

Well-known ports are **0–1023**. (Registered ports are 1024–49151; dynamic/private ports are 49152–65535 and are typically used as source ports.)

| Port | Protocol | TCP/UDP | What it does |
|---|---|---|---|
| 20 | FTP (data) | TCP | File transfer — data channel |
| 21 | FTP (control) | TCP | File transfer — control channel |
| 22 | SSH | TCP | Secure remote login |
| 23 | Telnet | TCP | Remote login (insecure, plaintext) |
| 25 | SMTP | TCP | Sending email |
| 53 | DNS | UDP/TCP | Name resolution (UDP for queries, TCP for zone transfers) |
| 67 | DHCP (server) | UDP | Server-side dynamic addressing |
| 68 | DHCP (client) | UDP | Client-side dynamic addressing |
| 69 | TFTP | UDP | Trivial file transfer (no auth, lightweight) |
| 80 | HTTP | TCP | Web pages |
| 110 | POP3 | TCP | Retrieving email |
| 143 | IMAP | TCP | Retrieving/managing email on the server |
| 443 | HTTPS | TCP | Secure web (HTTP over TLS) |

> 💡 **Exam tip:** Watch the DNS, DHCP, and TFTP rows — they ride on **UDP** because speed/low overhead matters more than guaranteed delivery. Email *sending* is SMTP (25); *retrieving* is POP3 (110) or IMAP (143). HTTP=80, HTTPS=443 are the most-tested of all.

---

## 7. TCP — Transmission Control Protocol (reliable)

**Intuition first:** TCP is like a registered, signed-for delivery. It sets up the connection first, numbers every piece, waits for confirmation, and resends anything lost — at the cost of extra overhead.

TCP's reliability features:
- **Connection-oriented** — negotiates and establishes a session *before* sending any data (via the 3-way handshake).
- **Reliable delivery** — retransmits lost or corrupted data.
- **Same-order delivery** — numbers and **sequences** segments so the receiver can reassemble them in order even if they arrive scrambled.
- **Flow control** — adjusts the sending rate so it doesn't overwhelm the receiver's limited memory/bandwidth.

TCP costs **20 bytes** of header overhead per segment (vs. UDP's 8).

### The TCP 3-way handshake (step by step)

This is how TCP establishes a session — know it cold:

1. **SYN** — the **client** starts the handshake by sending a segment with the **SYN** (synchronize) flag set, proposing an initial sequence number.
2. **SYN, ACK** — the **server** replies with one segment that has **both** the **ACK** flag (acknowledging the client's SYN) *and* its own **SYN** flag (synchronizing in the other direction).
3. **ACK** — the **client** sends a final **ACK** to acknowledge the server's SYN. The session is now established and data can flow.

```
Client                          Server
  |  --------- SYN ---------->   |   Step 1
  |  <------ SYN, ACK --------    |   Step 2
  |  --------- ACK ---------->   |   Step 3
  |        (session up)          |
```

> 💡 **Exam tip:** It is **SYN → SYN/ACK → ACK** — three segments, but the middle one carries *two* flags (SYN and ACK together). Closing a connection uses a **FIN** flag; each one-way session is torn down with a FIN + ACK pair.

### Sequencing, acknowledgment, and windowing
- **Sequencing:** segments may arrive out of order; the receiver buffers them and reorders them by **sequence number** before handing them to the application.
- **Acknowledgment:** the receiver ACKs data it has received. If the sender gets **no ACK** within a timeout, it goes back to the last acknowledged byte and **retransmits** from there.
- **Window size (windowing):** the amount of data a sender may transmit *before* it must receive an acknowledgment. It's a field in the TCP header used for **flow control** and managing lost data. A smaller window = receiver asking the sender to slow down.

Examples of TCP applications: **HTTP, FTP, SMTP, Telnet** (anything where every byte must arrive correctly and in order).

---

## 8. UDP — User Datagram Protocol (fast/lightweight)

**Intuition first:** UDP is like dropping a postcard in the mail — no setup, no tracking, no resend. It's fast and cheap, perfect when a little loss is fine but delay is not.

UDP's characteristics (the four "no's"):
- **Connectionless** — no session is set up before sending.
- **Unreliable delivery** — no guarantee data arrives.
- **No ordered reconstruction** — does not reorder out-of-sequence data.
- **No flow control** — does not throttle the sender.

UDP has only **8 bytes** of overhead, making it ideal for apps that can tolerate some loss. Its data units are called **datagrams**, sent as best-effort.

Examples of UDP applications: **DNS, video streaming, VoIP, TFTP, SNMP** (real-time or query/response traffic where speed beats guaranteed delivery).

> 💡 **Exam tip:** If an app "can tolerate some data loss but **not delay**" (voice, live video) → **UDP**. If "all data must arrive correctly and in order" (web page, file, email) → **TCP**.

### TCP vs. UDP comparison

| Feature | TCP | UDP |
|---|---|---|
| Connection | **Connection-oriented** (3-way handshake first) | **Connectionless** (just send) |
| Reliability | Reliable — retransmits lost data | Unreliable — best-effort, no retransmit |
| Ordering | Reassembles in sequence | No reordering |
| Flow control | Yes (windowing) | No |
| Acknowledgments | Yes | No |
| Header overhead | **20 bytes** | **8 bytes** |
| Speed | Slower (more overhead) | Faster (low overhead) |
| Data unit | Segment | Datagram |
| Typical uses | HTTP/HTTPS, FTP, SMTP, Telnet, SSH | DNS, DHCP, TFTP, VoIP, video streaming, SNMP |

---

## 🧪 Hands-on (Packet Tracer)
- **10.1.4 Configure Initial Router Settings** — basic router setup (hostname, passwords, banner) — the starting point before routing.
- **10.3.4 Connect a Router to a LAN** — assign IP addresses to router interfaces and `no shutdown` them so directly connected routes appear.
- **10.3.5 Troubleshoot Default Gateway Issues** — the classic "local works, remote fails" scenario; practice fixing host/router gateway settings.
- **13.2.6 Verify IPv4 and IPv6 Addressing** — confirm addressing with `show` commands on both protocols.
- **13.2.7 Use Ping and Traceroute** — run connectivity tests and read hop-by-hop output.
- **13.3.1 Use ICMP to Test Connectivity** — use ICMP/`ping` to isolate where a path breaks.
- **14.8.1 TCP and UDP Communications** — observe TCP sessions and UDP datagrams, ports, and the handshake in simulation mode.

---

## 🧠 Key terms to memorize

| Term | Meaning in one line |
|---|---|
| Connectionless | No setup before sending (IP, UDP). |
| Best-effort | No guarantee of delivery (IP, UDP). |
| Media independent | IP works over any physical medium. |
| Default gateway | Router interface a host uses to reach remote networks. |
| Routing table | Router's list of directly connected, remote, and default routes. |
| Directly connected route | Network on the router's own active interface (code **C**). |
| Default route | `0.0.0.0/0` — used when no better match exists. |
| Administrative distance (AD) | Trustworthiness ranking of a route's source; lower wins. |
| ICMP | Messenger protocol for IP errors/tests (ping, traceroute). |
| TTL / Hop Limit | Counter decremented per router; at 0 the packet is dropped. |
| Port number | Identifies the application/service in a segment. |
| Socket | IP address + port number — uniquely identifies one conversation endpoint. |
| 3-way handshake | SYN → SYN/ACK → ACK — establishes a TCP session. |
| Window size | Bytes a sender may send before needing an ACK (flow control). |
| Segment / Datagram | TCP's / UDP's data unit at the transport layer. |

---

## ⚡ Exam tips & traps (recap)
- IP is always **connectionless, best-effort, media independent** — reliability is **TCP's** job, not IP's.
- IPv6 header = **fewer fields, fixed length, no checksum**; TTL is renamed **Hop Limit**.
- A directly connected route is missing from the table → check that the interface is **`no shutdown`** with a correct **IP/mask**.
- **Default gateway** problems show as "local OK, remote fails."
- Router chooses **longest-prefix match** first; for a tie on the same prefix, **lowest AD** wins. A **floating static route** has a *higher* AD (backup only).
- `ping` and `traceroute` both use **ICMP**; `traceroute` works via **Time Exceeded** (TTL=0) replies and shows `*` for lost packets.
- 3-way handshake = **SYN, SYN/ACK, ACK** (middle segment carries two flags). Close uses **FIN**.
- TCP overhead = **20 bytes**, UDP = **8 bytes**.
- Memorize ports: 20/21 FTP, 22 SSH, 23 Telnet, 25 SMTP, 53 DNS, 67/68 DHCP, 69 TFTP, 80 HTTP, 110 POP3, 143 IMAP, 443 HTTPS.
- "Tolerates loss but not delay" (VoIP, streaming) → **UDP**; "needs every byte in order" (web, file, email) → **TCP**.

---

## ✅ Quick self-check
1. A host can ping `127.0.0.1` and other devices on its own subnet, but cannot reach any website. What is the most likely misconfiguration?
2. List, in order, the three segments of the TCP 3-way handshake and the flag(s) each carries.
3. Which transport protocol would you choose for a live video call, and why?
4. What protocol do `ping` and `traceroute` use, and what causes a `*` to appear in a traceroute?
5. Name the well-known port numbers for HTTPS, DNS, SMTP, and both halves of DHCP.

*(Answers and full reasoning are in `Quiz.md`.)*
