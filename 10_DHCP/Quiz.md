# Chapter 10 Quiz — DHCP

> Exam-style questions. Answers are marked and explained. Cover the explanations and test yourself first.

## Question 1: DORA order
A client has just booted with no IP address. What is the correct order of the DHCPv4 messages it exchanges with the server?

- [ ] Offer, Discover, Acknowledge, Request
- [x] Discover, Offer, Request, Acknowledge
- [ ] Request, Offer, Discover, Acknowledge
- [ ] Discover, Request, Offer, Acknowledge

### Explanation
* **Key point:** The handshake is **DORA** — **D**iscover → **O**ffer → **R**equest → **A**cknowledge.
* **Why not the others:** The client must first *find* a server (Discover) before anything is offered; Offer cannot come first.

---

## Question 2: Broadcast vs unicast
Which two DHCPv4 messages are sent as broadcasts? (Choose two.)

- [x] DHCPDISCOVER
- [ ] DHCPOFFER
- [x] DHCPREQUEST
- [ ] DHCPACK

### Explanation
* **Key point:** Pattern is **B-U-B-U**. Discover is broadcast (client has no idea where servers are); Request is broadcast so the chosen server is notified *and* the other servers withdraw their offers.
* **Why not the others:** Offer and Ack are **unicast** — by then the server knows the client's MAC.

---

## Question 3: Lease renewal
When a client renews a lease that has not yet expired, how is the initial DHCPREQUEST sent?

- [ ] as a broadcast to all DHCP servers
- [x] as a unicast directly to the server that issued the lease
- [ ] as a multicast to FF02::1:2
- [ ] it sends a new DHCPDISCOVER instead

### Explanation
* **Key point:** For renewal the client already knows its server, so it sends the DHCPREQUEST **unicast** straight to that server. Only if it gets no DHCPACK in time does it fall back to **broadcasting** so another server can extend the lease.
* **Why not the others:** Multicast `FF02::1:2` is DHCPv6, not DHCPv4 renewal.

---

## Question 4: UDP ports
Which UDP source and destination ports does a DHCPv4 *client* use when sending messages to the server?

- [ ] source 67, destination 68
- [x] source 68, destination 67
- [ ] source 546, destination 547
- [ ] source 547, destination 546

### Explanation
* **Key point:** DHCPv4 client → server uses **source port 68, destination port 67**. The server listens on 67.
* **Why not the others:** 546/547 are the DHCPv6 ports.

---

## Question 5: Allocation methods
Which DHCPv4 allocation method assigns an address from the pool **permanently, with no lease**?

- [ ] dynamic allocation
- [x] automatic allocation
- [ ] manual allocation
- [ ] stateful allocation

### Explanation
* **Key point:** **Automatic allocation** picks from the pool but assigns the address **permanently — no lease.**
* **Why not the others:** **Dynamic** allocation is the one that *leases* an address for a limited time. Manual ties a specific address to a specific device (admin-defined).

---

## Question 6: Excluding addresses
Refer to the configuration. In which mode is the command that prevents the DHCP server from leasing addresses `192.168.1.1` through `192.168.1.10` entered?

- [x] global configuration mode
- [ ] DHCP pool configuration mode
- [ ] interface configuration mode
- [ ] line configuration mode

### Explanation
* **Key point:** `ip dhcp excluded-address 192.168.1.1 192.168.1.10` is entered in **global config mode**, before/outside the pool.
* **Why not the others:** `network`, `default-router`, and `dns-server` are entered *inside* the pool (DHCP pool config mode).

---

## Question 7: Pool configuration command
Which command, entered in DHCP pool configuration mode, sets the **default gateway** that clients will receive?

- [ ] `network 192.168.1.0 255.255.255.0`
- [ ] `dns-server 192.168.1.1`
- [x] `default-router 192.168.1.1`
- [ ] `ip helper-address 192.168.1.1`

### Explanation
* **Key point:** `default-router` sets the clients' default gateway.
* **Why not the others:** `network` defines the subnet to lease from; `dns-server` sets DNS; `ip helper-address` is a relay command on an interface, not a pool command.

---

## Question 8: Build the pool (Choose three.)
A network administrator is configuring R1 as a DHCPv4 server for the `10.0.0.0/24` LAN with gateway `10.0.0.1` and DNS `10.0.0.2`. Which three commands belong **inside** the pool? (Choose three.)

- [ ] `ip dhcp excluded-address 10.0.0.1 10.0.0.2`
- [x] `network 10.0.0.0 255.255.255.0`
- [x] `default-router 10.0.0.1`
- [x] `dns-server 10.0.0.2`
- [ ] `service dhcp`

### Explanation
* **Key point:** Inside `ip dhcp pool` you define `network`, `default-router`, and `dns-server` (plus optional `domain-name`/`lease`).
* **Why not the others:** `ip dhcp excluded-address` and `service dhcp` are global commands, not pool commands.

---

## Question 9: Router as DHCP client
Which command configures a router interface to obtain its IPv4 address from an upstream DHCP server?

- [ ] `ip address dhcp-client`
- [x] `ip address dhcp`
- [ ] `ip helper-address dhcp`
- [ ] `ip dhcp client request`

### Explanation
* **Key point:** On the interface, `ip address dhcp` makes the router a DHCP **client**.
* **Why not the others:** The other forms are not valid IOS commands for this purpose.

---

## Question 10: Why relay is needed
Why must a router be configured as a DHCP relay agent when the DHCP server is on a different subnet from the clients?

- [ ] because DHCP servers can only serve one VLAN at a time
- [x] because routers do not forward the broadcast DHCPDISCOVER messages between subnets
- [ ] because the clients use UDP and routers only forward TCP
- [ ] because the lease time differs across subnets

### Explanation
* **Key point:** DHCPDISCOVER is a Layer 2/Layer 3 **broadcast**, and routers do not forward broadcasts. The relay agent catches it and forwards it as a unicast to the server.
* **Why not the others:** The issue is purely about broadcasts not crossing a router boundary.

---

## Question 11: ip helper-address placement
Refer to the exhibit. Clients are on R1's `G0/0/0` interface; the DHCP server is `172.16.5.10`, reachable out R1's `G0/0/1`. Where and how is `ip helper-address` configured?

- [ ] on G0/0/1, pointing to G0/0/0's address
- [x] on G0/0/0, with the command `ip helper-address 172.16.5.10`
- [ ] on G0/0/1, with the command `ip helper-address 172.16.5.10`
- [ ] in global config, with `ip helper-address 172.16.5.10`

### Explanation
* **Key point:** `ip helper-address` goes on the **interface that receives the client broadcasts** (the client-facing G0/0/0) and points to the **server's IP**.
* **Why not the others:** Placing it on the server-facing interface, or in global config, will not relay the clients' broadcasts.

---

## Question 12: Verification command
Which command displays the table of IPv4 addresses currently leased to clients, including the bound MAC/identifier and lease expiry?

- [ ] `show ip dhcp pool`
- [x] `show ip dhcp binding`
- [ ] `show running-config | section dhcp`
- [ ] `show ip dhcp conflict`

### Explanation
* **Key point:** `show ip dhcp binding` shows the **binding table** — which client holds which leased IP and when it expires.
* **Why not the others:** `show ip dhcp pool` shows utilisation stats; `show running-config | section dhcp` shows the configuration, not active leases.

---

## Question 13: SLAAC without a server
Which method provides dynamic global IPv6 addressing to end devices **without** a server keeping a record of available addresses?

- [ ] static IPv6 addressing
- [x] SLAAC
- [ ] stateless DHCPv6
- [ ] stateful DHCPv6

### Explanation
* **Key point:** **SLAAC** lets a host build its own global unicast address from the prefix in the RA — no server, no state.
* **Why not the others:** Stateful DHCPv6 *does* track addresses; stateless DHCPv6 still uses a server (for DNS etc.).

---

## Question 14: Reading the M flag
A router's ICMPv6 Router Advertisement has the **M (Managed Address Configuration) flag set to 1**. How will a client obtain its IPv6 address?

- [ ] it builds the address itself via SLAAC
- [x] it requests the address from a stateful DHCPv6 server
- [ ] it uses a link-local address only
- [ ] it sends a DHCPv4 Discover

### Explanation
* **Key point:** **M=1 means stateful DHCPv6** — the client gets its address (and other settings) from a DHCPv6 server that tracks state.
* **Why not the others:** SLAAC happens when M=0; the client still gets a global address, not just link-local.

---

## Question 15: Reading M=0, O=1
A client receives an RA with **M=0 and O=1**. Which statement is true?

- [ ] The client gets both its address and DNS from a DHCPv6 server.
- [x] The client builds its own address via SLAAC but gets other info (such as DNS) from a DHCPv6 server.
- [ ] The client uses pure SLAAC and contacts no server at all.
- [ ] The client uses stateful DHCPv6.

### Explanation
* **Key point:** **M=0, O=1 = stateless DHCPv6.** The host self-assigns its address (SLAAC) but queries a server for the *other* configuration like DNS.
* **Why not the others:** M=0/O=0 would be pure SLAAC; M=1 would be stateful.

---

## Question 16: DHCPv6 ports
Which UDP destination port does a DHCPv6 **client** use to send messages to the server?

- [ ] 67
- [ ] 68
- [ ] 546
- [x] 547

### Explanation
* **Key point:** DHCPv6 client → server uses **destination port 547**; server → client uses 546.
* **Why not the others:** 67/68 are DHCPv4.

---

## Question 17: DHCPv6 SOLICIT destination
To which address does a DHCPv6 client send its initial SOLICIT message?

- [ ] FF02::1 (all-nodes)
- [x] FF02::1:2 (all-DHCPv6-servers)
- [ ] FF02::2 (all-routers)
- [ ] 255.255.255.255

### Explanation
* **Key point:** The SOLICIT goes to the reserved multicast **FF02::1:2**, the all-DHCPv6-servers address. It has **link-local scope**, so routers don't forward it.
* **Why not the others:** FF02::1 is all-nodes, FF02::2 is all-routers; the broadcast `255.255.255.255` is IPv4.

---

## Question 18: Stateless vs stateful response message
A DHCPv6 client that already builds its own address with SLAAC and needs only DNS information will respond to the ADVERTISE with which message?

- [ ] DHCPv6 REQUEST
- [x] DHCPv6 INFORMATION-REQUEST
- [ ] DHCPv6 SOLICIT
- [ ] DHCPv6 RELEASE

### Explanation
* **Key point:** A **stateless** client sends **INFORMATION-REQUEST** — it only wants configuration parameters, not an address.
* **Why not the others:** A **stateful** client sends **REQUEST** because it needs an address *and* parameters.

---

## Question 19: DHCP attack identification
An attacker uses a tool to send a flood of DHCP requests with spoofed MAC addresses until the server has no addresses left to lease. What kind of attack is this?

- [x] DHCP starvation
- [ ] DHCP spoofing
- [ ] ARP poisoning
- [ ] DNS cache poisoning

### Explanation
* **Key point:** Exhausting the address pool with bogus requests is **DHCP starvation** — a denial-of-service.
* **Why not the others:** DHCP *spoofing* uses a rogue server; it doesn't exhaust the pool.

---

## Question 20: DHCP spoofing goal
What is the primary goal of a DHCP spoofing attack?

- [ ] to exhaust the DHCP address pool
- [x] to give clients a default gateway controlled by the attacker so traffic flows through the attacker
- [ ] to disable the switch's MAC address table
- [ ] to overload the DNS server

### Explanation
* **Key point:** A rogue DHCP server hands clients a **malicious default gateway** (and/or DNS), placing the attacker in the middle of the traffic path.
* **Why not the others:** Pool exhaustion is starvation, a different attack.

---

## Question 21: DHCP snooping defaults
Which statement correctly describes the default behaviour of DHCP snooping on a switch?

- [ ] All interfaces are trusted by default.
- [x] All interfaces are untrusted by default; you must explicitly trust the ports facing the legitimate server and uplinks.
- [ ] Only trunk ports are untrusted by default.
- [ ] DHCP snooping must be enabled per host MAC address.

### Explanation
* **Key point:** With snooping enabled, **all ports are untrusted by default.** You mark uplinks and the real DHCP server's port as **trusted** with `ip dhcp snooping trust`.
* **Why not the others:** Trust is never automatic for any port type.

---

## Question 22: Trusted vs untrusted ports
On a switch running DHCP snooping, which DHCP messages may an **untrusted** port legitimately source?

- [ ] all DHCP messages, including OFFER and ACK
- [x] DHCP client requests only (such as DISCOVER and REQUEST)
- [ ] only DHCPACK messages
- [ ] no DHCP messages at all

### Explanation
* **Key point:** **Untrusted ports can source requests only.** If a device on an untrusted port sends a *server* message (OFFER/ACK), it's acting as a rogue server and the switch **shuts the port down**.
* **Why not the others:** Sourcing server replies is reserved for **trusted** ports.

---

## Question 23: Mitigating starvation specifically
Which DHCP snooping command specifically limits how many DHCP messages an untrusted port can send per second, countering a starvation attack?

- [ ] `ip dhcp snooping trust`
- [ ] `ip dhcp snooping vlan 10`
- [x] `ip dhcp snooping limit rate 6`
- [ ] `ip dhcp excluded-address`

### Explanation
* **Key point:** `ip dhcp snooping limit rate <n>` caps DHCP packets/second on untrusted ports, so an attacker can't flood the server to exhaust the pool.
* **Why not the others:** `trust` designates a trusted port; `vlan` enables snooping on a VLAN; `excluded-address` is a server-pool command, not a snooping defence.

---

## Question 24: ip helper-address services
Besides DHCP/BOOTP, the `ip helper-address` command also forwards several other UDP services by default. Which two of the following are among them? (Choose two.)

- [x] DNS (port 53)
- [x] TFTP (port 69)
- [ ] HTTP (port 80)
- [ ] SSH (port 22)
- [ ] SNMP (port 161)

### Explanation
* **Key point:** By default `ip helper-address` relays **eight** UDP services, including Time (37), TACACS (49), **DNS (53)**, DHCP/BOOTP (67/68), **TFTP (69)**, and NetBIOS name/datagram (137/138).
* **Why not the others:** HTTP, SSH, and SNMP are not in the default forwarded list.

---

## Question 25: Match the DHCPv6 method
Match each DHCPv6 method to its RA flag setting and address source.

| Method | RA flags / address source |
| :--- | :--- |
| **SLAAC** | **M=0, O=0 — host builds its own address, no server** |
| **Stateless DHCPv6** | **M=0, O=1 — host builds own address; server provides DNS/other info** |
| **Stateful DHCPv6** | **M=1 — server assigns the address and tracks state** |

### Explanation
* **Key point:** Read the flags as a checklist: **M=1 → stateful** (server gives the address); **M=0/O=1 → stateless** (host self-assigns, server gives extras); **M=0/O=0 → SLAAC** (no server).

---

## Question 26: Lease command and default
Which statement about the DHCPv4 lease is correct?

- [ ] The default lease time is one hour and cannot be changed.
- [x] The default lease time is one day, and it can be changed with the `lease` command inside the pool.
- [ ] Leases are only used in automatic allocation.
- [ ] The lease command is entered in global configuration mode.

### Explanation
* **Key point:** The **default lease is one day**, configurable per pool with `lease` (e.g. `lease 3` for three days).
* **Why not the others:** Leasing belongs to **dynamic** allocation (automatic allocation is permanent); the `lease` command is entered inside the pool, not globally.
