# Chapter 7 — Application Layer

> **Day 4 of 7** · Estimated study time: ~3 hours
> *This chapter is about the top of the stack: how the apps you actually use (browsers, email, file transfer) talk over the network, the protocols that make it work, how to design a small network, and — most importantly for the exam — how to troubleshoot when something breaks.*

## 🎯 What you'll be able to do after this chapter
- Explain the role of the application layer and how the OSI application, presentation, and session layers split the work.
- Tell the difference between the **client/server** and **peer-to-peer** communication models.
- Name the common application-layer protocols (HTTP/HTTPS, DNS, DHCP, SMTP/POP3/IMAP, FTP/TFTP, Telnet/SSH, SMB) and the **ports** they use.
- Walk through how **DNS** resolves a name into an IP address, and recognise DNS record types.
- Describe how **DHCP** hands out addresses (the DORA process).
- Design a small network: pick devices, plan for **scaling** and **redundancy**.
- Troubleshoot connectivity with **ping**, **traceroute/tracert**, **ipconfig**, and **show** commands — and read Cisco IOS ping output symbols (`!`, `.`, `U`).

---

## 1. What the application layer actually does

Think of the network as a postal system. Layers 1–6 are the trucks, sorting offices, and envelopes. The **application layer** is the *person writing and reading the letter* — it's the only layer the user ever sees directly.

The **application layer** is the layer closest to the end user. It provides the **interface** between the applications we use (a browser, an email client) and the network underneath. It does **not** mean "the application itself" (Chrome is a program); it means the *protocols* the application uses to exchange data with another host (e.g. HTTP).

In the **OSI model**, the top three layers split this job:

| OSI layer | Job | Plain-English example |
|---|---|---|
| **Application** (7) | Interface to the user; the protocol that exchanges data | HTTP fetches a web page |
| **Presentation** (6) | Format, **compress**, and **encrypt**/decrypt data into a common form | Encode video as MPG, encrypt with TLS |
| **Session** (5) | **Create and maintain dialogs** between source and destination; restart dropped/idle sessions | Keeping you logged into a site |

In the **TCP/IP model**, these three OSI layers are squashed into a single **Application layer**. CCNA expects you to know both views.

> 💡 **Exam tip:** Presentation layer = **format, compress, encrypt** (think "MKV, MPG, MOV" and encryption). Session layer = **start/keep/restart dialogs**. If a question mentions encryption or data format, it's *presentation*; if it mentions maintaining a conversation, it's *session*.

---

## 2. Client/server vs peer-to-peer

**Intuition:** in client/server, one machine is the boss that holds the data; in peer-to-peer (P2P), everyone is equal and shares directly.

- **Client/server model:** A **client** requests information, a **server** responds. The server is dedicated (a web server, a mail server). Data is centralised, easier to secure and back up, but the server is a single point of load/failure. HTTP, FTP, and email all use this model.
- **Peer-to-peer (P2P) model:** Each device can be **both** client and server at the same time, with no dedicated central server. Cheap and simple for tiny networks (two PCs sharing files), but hard to secure and scale. *P2P applications* (e.g. file-sharing apps) are different from a *P2P network* — the app can run on top of any network.

> 💡 **Exam tip:** In P2P, a single device acts as **both client and server simultaneously**. That phrase is the giveaway.

---

## 3. Network services & protocols

This is the heart of the chapter. Learn the protocol, what it does, whether it uses **TCP or UDP**, and its **port number**. The exam loves port-number questions.

### 3.1 HTTP / HTTPS — the web
- **HTTP (Hypertext Transfer Protocol)** is a **request/response** protocol. You type a **URL** into a browser; the browser opens a connection to the web service and asks for the page.
- Three common HTTP message types: **GET** (request a page), **POST** (send form data to the server), **PUT** (upload a file to the server).
- **HTTPS (HTTP Secure)** adds **authentication and encryption** (via TLS) so data is protected in transit. Same idea as HTTP, but secure.

### 3.2 Email — SMTP, POP3, IMAP
Email is **store-and-forward**: messages sit in databases on mail servers until retrieved. Clients talk to servers; servers talk to other servers to move mail between domains. Three protocols, two jobs:

| Protocol | Job | Behaviour |
|---|---|---|
| **SMTP** | **Send** mail (client→server, and server→server) | Reliable transfer; needs header + body |
| **POP3** | **Retrieve** mail | **Downloads then deletes** from server (mail lives on the client) |
| **IMAP** | **Retrieve** mail | **Copies** to client; **originals stay on server** until you delete them |

> 💡 **Exam tip:** **SMTP = Send Mail To People** (it only sends). POP **deletes from the server** after download; IMAP **keeps a copy on the server**. If a question says "user can read mail from multiple devices and it stays on the server," that's **IMAP**.

### 3.3 FTP / TFTP — file transfer
- **FTP (File Transfer Protocol)** moves files between a client and server. It uses **two TCP connections**:
  - **Port 21** — the **control** connection (commands and replies).
  - **Port 20** — the **data** connection (the actual file), opened each time data moves.
  - The client can **pull** (download) or **push** (upload).
- **TFTP (Trivial FTP)** is a stripped-down, **UDP**-based (port **69**) file transfer with no authentication — used for things like loading IOS images or config files. "Trivial" = simple and unreliable.

> 💡 **Exam tip:** FTP uses **two** ports: **21 = control, 20 = data**. TFTP is the *trivial* one — **UDP/69, no login**.

### 3.4 SMB — file/printer sharing
- **SMB (Server Message Block)** is a client/server, **request-response** file-sharing protocol for shared directories, files, printers, and ports. Clients open a **long-term** connection and use server resources as if they were local (unlike FTP's transfer-and-done model).
- Linux/UNIX implement SMB via **SAMBA**; macOS also supports SMB.

### 3.5 Telnet / SSH — remote device management
- **Telnet** opens a remote CLI session over **TCP port 23**, but sends everything (including passwords) in **plaintext** — insecure.
- **SSH (Secure Shell)** does the same job over **TCP port 22**, but **encrypted** and **authenticated**. Always prefer SSH.

> 💡 **Exam tip:** To **securely** manage a device remotely, use **SSH (22)**, never Telnet (23). This shows up constantly.

### 3.6 DHCP — automatic addressing (see §4)
### 3.7 DNS — name resolution (see §5)

### Protocol → Port quick reference (memorise this table)

| Protocol | Transport | Port(s) | Purpose |
|---|---|---|---|
| FTP (data) | TCP | **20** | File data transfer |
| FTP (control) | TCP | **21** | File transfer commands |
| SSH | TCP | **22** | Secure remote CLI |
| Telnet | TCP | **23** | Insecure remote CLI |
| SMTP | TCP | **25** | Sending email |
| DNS | UDP/TCP | **53** | Name resolution |
| DHCP (server/client) | UDP | **67 / 68** | Dynamic addressing |
| TFTP | UDP | **69** | Simple file transfer |
| HTTP | TCP | **80** | Web (unsecured) |
| POP3 | TCP | **110** | Retrieve email (download & delete) |
| IMAP | TCP | **143** | Retrieve email (keep on server) |
| HTTPS | TCP | **443** | Web (secured) |
| SMB | TCP | **445** | File/printer sharing |

> 💡 **Exam tip:** The ones tested most often: **HTTP 80, HTTPS 443, FTP 20/21, SSH 22, Telnet 23, DNS 53, SMTP 25, POP3 110, IMAP 143, DHCP 67/68, TFTP 69.** Note which are **UDP** (DNS, DHCP, TFTP) — that's a favourite trap.

---

## 4. DHCP — Dynamic Host Configuration Protocol

**Intuition:** instead of typing an IP address into every device by hand, DHCP is a clerk that hands each new device an address (plus subnet mask, default gateway, and DNS server) the moment it joins.

**DHCP (Dynamic Host Configuration Protocol)** automates assignment of:
- IP address
- Subnet mask
- Default gateway
- DNS server address (and other parameters)

A client gets its lease through a four-step exchange, remembered as **DORA**:

1. **D**iscover — the client broadcasts "Is there a DHCP server out there?" (DHCPDISCOVER).
2. **O**ffer — a server replies "Here's an address you can have" (DHCPOFFER).
3. **R**equest — the client says "Yes, I'll take that one" (DHCPREQUEST).
4. **A**cknowledge — the server confirms and finalises the lease (DHCPACK).

> 📎 **Cross-reference:** DHCP server *configuration* on a Cisco router (pools, excluded addresses, `ip helper-address` relay) is covered in **Chapter 10**. For Chapter 7, just know what DHCP does and the **DORA** sequence.

> 💡 **Exam tip:** If you ever see a host with a **169.254.x.x** address, DHCP **failed** — that's an APIPA self-assigned address, meaning the client never got a DHCPOFFER/ACK.

---

## 5. DNS — Domain Name System

**Intuition:** humans remember names (`www.cisco.com`), computers route by numbers (`198.133.219.25`). DNS is the phone book that translates one into the other.

- A **fully-qualified domain name (FQDN)** like `www.cisco.com` is far easier to remember than its numeric IP. **DNS (Domain Name System)** is the automated service that resolves names to addresses.
- DNS uses a **distributed, hierarchical** set of servers (broken into manageable **zones**), so no single server holds the whole internet. Servers run name-resolution software such as **BIND**.

### 5.1 DNS resolution walk-through
What happens when you type `www.cisco.com`:

1. The PC checks its **local DNS cache** first (recently resolved names live in memory — `ipconfig /displaydns` shows them on Windows).
2. If not cached, the PC sends a **query** to its configured **DNS server** (UDP port **53**).
3. The DNS server checks **its own records**. If it has the answer, it replies.
4. If it **can't** resolve the name, it **contacts other DNS servers** (walking the hierarchy: root → top-level domain `.com` → cisco.com's authoritative server) to find the answer.
5. The answer (the IP address) is returned to the PC and **cached** for next time.
6. The PC now opens the connection to that IP.

### 5.2 DNS record types

| Record | Meaning |
|---|---|
| **A** | Maps a name to an **IPv4** address |
| **AAAA** | Maps a name to an **IPv6** address (say "quad-A") |
| **NS** | Identifies an **authoritative name server** for the zone |
| **CNAME** | An alias — points one FQDN to another canonical name |
| **MX** | **Mail exchange** — maps a domain to its mail servers |

### 5.3 DNS troubleshooting tools
- **`nslookup`** — manually query a name server to resolve a host name; great for verifying DNS works and which server answered.
- **`ipconfig /displaydns`** (Windows) — show the local resolver cache.

> 💡 **Exam tip:** **A = IPv4, AAAA = IPv6, MX = mail, NS = name server, CNAME = alias.** And remember: DNS uses **port 53**. If a PC can ping by IP but not by name, suspect **DNS**, not connectivity.

---

## 6. Designing a small network

A small network has predictable needs. Good design plans ahead.

**Device selection — what to consider:**
- **Cost** and the number/types of **ports** required.
- **Speed** and expandability of the device.
- **Operating system features and services** (security, QoS, etc.).

**Protocols a small network commonly runs:** DHCP, DNS, HTTP/HTTPS, SMTP/POP3/IMAP, FTP, SSH.

**Scaling:** as the network grows, document and analyse traffic with a **protocol analyzer** so upgrades match real demand. Plan address space and device capacity for growth, not just today.

**Redundancy:** build **multiple paths** between devices so there is **no single point of failure**. If one link or switch dies, traffic still has a route. (Redundancy = eliminating single points of failure — memorise that phrasing.)

**Real-time traffic (QoS):** **voice and video** are delay-sensitive and should be given **priority** over things like email and web browsing.

> 💡 **Exam tip:** **Redundancy = multiple paths / no single point of failure.** **QoS** prioritises **voice and video**. A **protocol analyzer / baseline** is how you document traffic before upgrading.

---

## 7. Troubleshooting methodology & tools

**Intuition:** troubleshoot the way you'd diagnose a road trip gone wrong — confirm you can leave your driveway (local config), reach the end of the street (gateway), then trace where the journey actually stops.

### 7.1 The baseline
A **network baseline** is a record of normal performance (ping times, bandwidth, etc.) captured when things work. Later, you compare against it: e.g. an **increase in host-to-host ping response times** versus the baseline points to a **latency** problem.

### 7.2 ping — "can I reach it at all?"
`ping` sends **ICMP** echo requests and waits for echo replies. It tells you *whether* a destination is reachable and the round-trip time.
- Ping **127.0.0.1** (loopback) to test the local TCP/IP stack.
- Ping the **default gateway** to test local connectivity.
- Ping a remote host to test end-to-end.
- An **extended ping** (run from a router) lets you set the **source address**, useful to prove a specific interface can reach a host.

### 7.3 traceroute / tracert — "where does it stop?"
`traceroute` (Cisco/Linux) and `tracert` (Windows) show **every hop** along the path, using ICMP and incrementing **TTL/Hop Limit**. Each router that decrements TTL to zero sends back a Time-Exceeded message, revealing itself.
- Use it to **identify where a packet is lost or delayed**.
- **Troubleshoot from the last hop that responded** — that's where the problem starts.
- `tracert -6` forces the trace over **IPv6**.

> 💡 **Exam tip:** **Ping shows reachability only; traceroute shows every hop.** Both use **ICMP**. To find *where* a path breaks, use **traceroute/tracert**.

### 7.4 ipconfig and friends (host commands)
- **`ipconfig`** — show IP, subnet mask, default gateway.
- **`ipconfig /all`** — adds MAC address, DHCP server, DNS server.
- **`ipconfig /displaydns`** — show the DNS resolver cache.
- **`nslookup`** — query DNS manually.

### 7.5 Cisco IOS show commands
| Command | What it tells you |
|---|---|
| `show ip interface brief` | Quick status of every interface (up/down, IP) |
| `show ip route` | The routing table — what interface reaches which network |
| `show interfaces` | Detailed interface stats, errors, bandwidth |
| `show running-config` | Current live config (e.g. verify IPv6 routing / SSH is enabled) |
| `show version` | IOS version, uptime, hardware, RAM |
| `show cdp neighbors [detail]` | Directly connected Cisco devices — verifies **Layer 2** connectivity; `detail` adds IP, hostname, IOS |

> 💡 **Exam tip:** `show cdp neighbors` checks **Layer 2** connectivity (even if a ping fails). `show ip route` tells you which **interface** reaches a network. `show cdp neighbors detail` and `show version` reveal the **IOS version**.

### 7.6 Reading IOS ping output symbols
When you `ping` from a Cisco router, each reply is a single character:

| Symbol | Meaning |
|---|---|
| **`!`** | **Success** — an echo reply was received |
| **`.`** | **Timeout** — waiting for a reply timed out (possible problem, e.g. no reply in time) |
| **`U`** | A router on the path sent an **ICMP unreachable** — **no route to destination** |

A typical good result is `!!!!!` (5 successes). A result like `U.U.U` means routers can't reach the destination.

> 💡 **Exam tip:** Memorise **`!` = success, `.` = timed out, `U` = destination/route unreachable.** A common trap answer swaps these — read carefully.

---

## 🧪 Hands-on (Packet Tracer)
- **13.2.7 Use Ping and Traceroute** — practise `ping` and `traceroute/tracert` to test reachability and trace the path hop-by-hop.
- **17.5.9 Interpret show Command Output** — read `show ip interface brief`, `show ip route`, `show version`, etc. to assess device state.
- **17.7.7 Troubleshoot Connectivity Issues** — apply ping/show commands to find and fix a broken path.
- **17.8.3 Troubleshooting Challenge** — an end-to-end fault-finding exercise tying the whole methodology together.

## 🧠 Key terms to memorize
| Term | Meaning in one line |
|---|---|
| Application layer | Interface between user apps and the network |
| Presentation layer | Formats, compresses, and encrypts data |
| Session layer | Starts, maintains, and restarts dialogs |
| Client/server | Dedicated server responds to client requests |
| Peer-to-peer | A device is both client and server at once |
| HTTP / HTTPS | Web; HTTPS adds encryption + authentication |
| DNS | Resolves names ↔ IP addresses (port 53) |
| FQDN | Full domain name, e.g. `www.cisco.com` |
| DHCP / DORA | Auto-assigns IP config; Discover-Offer-Request-Ack |
| SMTP / POP3 / IMAP | Send / retrieve-delete / retrieve-keep email |
| FTP / TFTP | File transfer (TCP 21/20) / trivial (UDP 69) |
| Telnet / SSH | Remote CLI: plaintext (23) / encrypted (22) |
| Baseline | Recorded "normal" performance to compare against |
| Redundancy | Multiple paths; no single point of failure |
| ICMP | Protocol behind ping and traceroute |

## ⚡ Exam tips & traps (recap)
- **Presentation = format/compress/encrypt; Session = maintain dialogs.** Don't confuse them.
- **P2P = one device is client AND server simultaneously.**
- **Ports:** HTTP 80, HTTPS 443, FTP 21(control)/20(data), SSH 22, Telnet 23, DNS 53, SMTP 25, POP3 110, IMAP 143, TFTP 69, DHCP 67/68. **DNS, DHCP, TFTP run over UDP.**
- **SMTP sends; POP3 downloads & deletes; IMAP keeps copies on the server.**
- **DHCP order = DORA** (Discover, Offer, Request, Acknowledge). A **169.254.x.x** address = DHCP failed.
- **DNS records:** A=IPv4, AAAA=IPv6, MX=mail, NS=name server, CNAME=alias.
- **Use SSH, not Telnet,** for secure management.
- **Ping = reachability; traceroute = every hop.** Both use **ICMP**. Begin troubleshooting at the **last responsive hop**.
- **IOS ping: `!`=success, `.`=timeout, `U`=unreachable.**
- **`show cdp neighbors` = Layer 2 check; `show ip route` = which interface reaches a network.**
- **Redundancy = no single point of failure; QoS prioritises voice/video.**

## ✅ Quick self-check
1. Which OSI layer encrypts and compresses data, and which one keeps a dialog alive?
2. What are the four steps of DHCP, in order, and what does each accomplish?
3. A PC can ping `8.8.8.8` but `www.cisco.com` fails — what's the most likely cause, and which port/tool is involved?
4. Name the transport protocol and port for FTP-control, TFTP, SSH, and HTTPS.
5. On a Cisco router, what do the ping outputs `!!!!!` and `U.U.U` each tell you?
