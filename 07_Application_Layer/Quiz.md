# Chapter 7 Quiz — Application Layer

> Exam-style questions. Answers are marked and explained. Cover the explanations and test yourself first.

## Question 1: Presentation layer function
Which two functions are performed at the presentation layer of the OSI model? (Choose two.)

- [ ] maintaining and restarting dialogs between two applications
- [x] compression of data so the destination can decompress it
- [x] encryption of data for transmission and decryption on receipt
- [ ] providing the interface between applications and the network
- [ ] determining the path a packet takes through the network

### Explanation
* **Key point:** The presentation layer (OSI Layer 6) **formats, compresses, and encrypts** data into a common form for the destination.
* **Why not the others:** Maintaining/restarting dialogs is the **session** layer; the user-application interface is the **application** layer; path selection is Layer 3 (routing).

---

## Question 2: Communication model
In which networking model does a single device act as both a client and a server at the same time?

- [ ] client/server
- [x] peer-to-peer
- [ ] store-and-forward
- [ ] request/response

### Explanation
* **Key point:** In the **peer-to-peer (P2P)** model there is no dedicated server — each device can request resources (client role) and provide them (server role) simultaneously.
* **Why not the others:** Client/server uses dedicated servers. Store-and-forward and request/response describe protocol behaviours, not network models.

---

## Question 3: Protocol-to-port matching
Match each application-layer protocol to its well-known port.

| Protocol | Port |
| :--- | :--- |
| HTTPS | **443** |
| SSH | **22** |
| SMTP | **25** |
| DNS | **53** |
| FTP control | **21** |
| POP3 | **110** |

### Explanation
* **Key point:** Memorise these. HTTPS 443, SSH 22, SMTP 25, DNS 53, FTP control 21 (data 20), POP3 110.
* Note IMAP is 143, HTTP is 80, Telnet is 23, TFTP is UDP 69, and DHCP uses UDP 67/68.

---

## Question 4: Email retrieval protocol
A user wants to read the same email messages from a laptop, a phone, and a desktop, with the original messages remaining stored on the mail server. Which protocol should be used?

- [ ] SMTP
- [ ] POP3
- [x] IMAP
- [ ] FTP

### Explanation
* **Key point:** **IMAP** downloads *copies* and keeps the originals on the server until manually deleted — ideal for accessing the same mailbox from multiple devices.
* **Why not the others:** **POP3** downloads then **deletes** from the server. **SMTP** only *sends* mail. **FTP** is for file transfer.

---

## Question 5: FTP ports
Which statement correctly describes how FTP uses TCP ports during a session?

- [ ] FTP uses UDP port 69 for both commands and data.
- [x] FTP uses TCP port 21 for control traffic and TCP port 20 for data transfer.
- [ ] FTP uses TCP port 20 for control traffic and TCP port 21 for data transfer.
- [ ] FTP uses a single TCP port 21 for all traffic.

### Explanation
* **Key point:** FTP opens **two** connections — **port 21** for the control channel (commands/replies) and **port 20** for the actual data transfer.
* **Why not the others:** UDP/69 is **TFTP**. The control/data ports are not swapped, and FTP is not single-connection.

---

## Question 6: Secure remote management
A technician must document the configurations of network devices in several off-site buildings and needs to access them securely over the network. Which protocol should be used?

- [ ] Telnet
- [ ] FTP
- [x] SSH
- [ ] HTTP

### Explanation
* **Key point:** **SSH (TCP 22)** provides authenticated, **encrypted** remote CLI access — the secure choice.
* **Why not the others:** **Telnet (23)** sends everything, including passwords, in plaintext. FTP and HTTP are not for device CLI management and are unencrypted.

---

## Question 7: DHCP service
Which network service automatically assigns IP addresses, subnet masks, default gateways, and DNS server addresses to devices on a network?

- [ ] DNS
- [ ] Telnet
- [x] DHCP
- [ ] TFTP

### Explanation
* **Key point:** **DHCP** automates IP configuration so administrators don't assign addresses by hand.
* **Why not the others:** DNS resolves names to addresses; Telnet is remote access; TFTP is simple file transfer.

---

## Question 8: DHCP DORA sequence
Place the steps of the DHCP lease process in the correct order.

- [ ] Offer, Discover, Acknowledge, Request
- [x] Discover, Offer, Request, Acknowledge
- [ ] Request, Discover, Offer, Acknowledge
- [ ] Discover, Request, Offer, Acknowledge

### Explanation
* **Key point:** Remember **DORA**: **D**iscover (client broadcasts), **O**ffer (server offers an address), **R**equest (client requests it), **A**cknowledge (server confirms the lease).
* Detailed DHCP server configuration on a router is covered in **Chapter 10**.

---

## Question 9: DNS records
Match each DNS resource record to what it provides.

| Record | Resolves to |
| :--- | :--- |
| A | **an IPv4 address** |
| AAAA | **an IPv6 address** |
| MX | **a mail exchange server** |
| NS | **an authoritative name server** |
| CNAME | **an alias / canonical name** |

### Explanation
* **Key point:** **A**=IPv4, **AAAA**=IPv6, **MX**=mail, **NS**=name server, **CNAME**=alias. These are heavily tested.

---

## Question 10: DNS troubleshooting
A PC can successfully ping `8.8.8.8` but cannot open `www.example.com` in a browser. Which is the most likely cause, and which tool best confirms it?

- [ ] The default gateway is misconfigured; use `show ip route`.
- [x] DNS name resolution is failing; use `nslookup`.
- [ ] The cable is faulty; use `show interfaces`.
- [ ] DHCP has failed; the PC has a 169.254 address.

### Explanation
* **Key point:** Reaching an IP but not a name means basic connectivity works and **DNS** is the problem. **`nslookup`** queries a name server manually to verify resolution.
* **Why not the others:** If the gateway or cable were down, the ping by IP would also fail. A 169.254 address would break the ping by IP too.

---

## Question 11: tracert purpose
Why would a network administrator use the `tracert` utility?

- [ ] to display the active TCP connections on a PC
- [x] to identify where a packet was lost or delayed along the path
- [ ] to look up a DNS name on the DNS server
- [ ] to show the IP address, gateway, and DNS server of a PC

### Explanation
* **Key point:** **tracert/traceroute** lists every hop, so it pinpoints **where** a packet is dropped or delayed.
* **Why not the others:** Active connections come from `netstat`; DNS lookups from `nslookup`; the PC's own IP info from `ipconfig`.

---

## Question 12: ping vs tracert
Which statement correctly describes the difference between the `ping` and `tracert` commands?

- [ ] Tracert uses IP addresses; ping does not.
- [ ] Both display results in a graphical map.
- [x] Tracert shows each hop along the path, while ping shows only a destination reply.
- [ ] Ping shows whether the transmission succeeded; tracert cannot.

### Explanation
* **Key point:** **Ping** tests reachability (a single round trip to the destination); **tracert** reveals **every router hop** in between.
* **Why not the others:** Both use ICMP and IP addresses, and both are text-based; ping does report success/failure.

---

## Question 13: IOS ping indicators
Refer to a router ping that returns the output `U.U.U`. Which statement is true about Cisco IOS ping indicators?

- [ ] A `.` indicates the ping succeeded but the response was slower than normal.
- [ ] A `!` indicates the ping was unsuccessful due to a DNS problem.
- [x] A `U` may indicate that a router along the path had no route to the destination and the ping was unsuccessful.
- [ ] A `U` indicates a successful echo reply was received.

### Explanation
* **Key point:** In IOS ping output, **`!`** = success, **`.`** = timed out (no reply in time), and **`U`** = a router returned an **ICMP unreachable** (no route to destination).
* **Why not the others:** `.` is a timeout, not "slow success"; `!` is success, not failure; `U` is never a success indicator.

---

## Question 14: tracert troubleshooting (exhibit)
Refer to the exhibit. An administrator runs `tracert` from PC1 toward PC2. The first hop, router R1 (192.168.1.254), replies successfully, but every hop after it times out. Where should troubleshooting begin?

- [ ] PC2
- [ ] the switch nearest PC1
- [x] R1
- [ ] PC1

### Explanation
* **Key point:** Always begin at the **last hop that responded**. R1 answered, so the fault lies at R1's outbound path (or the next device R1 forwards to) — start there.
* **Why not the others:** PC1 clearly has connectivity (it reached R1). Jumping straight to PC2 skips the actual point of failure.

---

## Question 15: extended ping
Host H3 cannot reach host H1. An administrator wants to prove, from router R2, that R2's interface used to reach remote networks can communicate with H1 — specifying the source address of the test. Which tool accomplishes this?

- [ ] `show cdp neighbors`
- [ ] `traceroute`
- [x] an extended ping
- [ ] `telnet`

### Explanation
* **Key point:** An **extended ping** (run from the router's privileged EXEC mode) lets you specify the **source IP address**, proving a particular interface can reach the destination.
* **Why not the others:** A standard ping uses the outgoing interface as source automatically; CDP and Telnet don't test source-specific reachability.

---

## Question 16: show command for next-hop interface
Which command lets an administrator determine which interface a router will use to reach a particular remote network?

- [ ] `show interfaces`
- [x] `show ip route`
- [ ] `show arp`
- [ ] `show protocols`

### Explanation
* **Key point:** **`show ip route`** displays the routing table, including which interface (or next hop) is used to reach each network.
* **Why not the others:** `show interfaces` gives interface stats, `show arp` maps IP↔MAC, `show protocols` shows protocol/interface status but not route selection detail.

---

## Question 17: verifying Layer 2 connectivity
A ping fails between directly connected routers R1 and R2. The administrator then issues `show cdp neighbors`. Why run this command after a failed ping?

- [ ] to verify the IP address configured on R2
- [ ] because a failed ping always indicates a virus
- [x] to verify Layer 2 connectivity between the routers
- [ ] to test connectivity from a non-directly-connected network

### Explanation
* **Key point:** **CDP** operates at **Layer 2**, so `show cdp neighbors` confirms the link is up at Layer 2 even when a Layer 3 ping fails (e.g. an IP/subnet misconfiguration).
* **Why not the others:** CDP shows the neighbor exists but is not primarily an IP-verification or remote-network test.

---

## Question 18: neighbor device details
A network engineer needs the IP address, host name, and IOS version of neighboring Cisco devices to troubleshoot connectivity. Which command provides this?

- [ ] `show ip route`
- [ ] `show interfaces`
- [x] `show cdp neighbors detail`
- [ ] `show version`

### Explanation
* **Key point:** **`show cdp neighbors detail`** adds the neighbor's IP address, host name, platform, and IOS version to the basic CDP listing.
* **Why not the others:** `show version` shows the **local** device's IOS, not neighbors'; the others don't list neighbor device details.

---

## Question 19: baseline and latency
A network engineer reviews a recent network baseline. Which finding would indicate a possible latency problem?

- [ ] a change in RAM shown by `show version`
- [ ] a change in bandwidth shown by `show interfaces`
- [x] an increase in host-to-host ping response times
- [ ] a next-hop timeout in a traceroute

### Explanation
* **Key point:** A **baseline** captures normal performance. Rising **ping response times** compared to that baseline indicate increased **latency**.
* **Why not the others:** RAM/bandwidth changes are capacity facts, not latency; a single traceroute timeout suggests a reachability/filtering issue rather than overall latency.

---

## Question 20: small-network design — redundancy & QoS
A small business is upgrading its network and wants to (a) eliminate single points of failure and (b) prioritise real-time traffic. Which two design choices meet these goals? (Choose two.)

- [x] Build multiple paths between switches so traffic survives a link or device failure.
- [x] Apply QoS to give voice and video traffic higher priority than email and web browsing.
- [ ] Replace SSH with Telnet to simplify management.
- [ ] Use a single high-capacity switch as the only path for all traffic.
- [ ] Disable DNS to reduce broadcast traffic.

### Explanation
* **Key point:** **Redundancy** means multiple paths / **no single point of failure**; **QoS** gives priority to delay-sensitive **voice and video**.
* **Why not the others:** Telnet is less secure than SSH; a single switch *creates* a single point of failure; disabling DNS would break name resolution.

---

## Question 21: HTTP message types and HTTPS
Which statement about HTTP and HTTPS is correct?

- [ ] HTTP encrypts data; HTTPS does not.
- [ ] The HTTP GET message is used to upload a file to the server.
- [x] HTTPS adds authentication and encryption to secure data between client and server.
- [ ] HTTP and HTTPS both use UDP for fast page delivery.

### Explanation
* **Key point:** **HTTPS** secures HTTP with **authentication and encryption** (TLS) over TCP **443**; plain HTTP (TCP 80) is unencrypted.
* **Why not the others:** GET *requests* a page (PUT uploads, POST sends form data); both run over **TCP**, not UDP.
