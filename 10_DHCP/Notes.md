# Chapter 10 — DHCP

> **Day 6 of 7** · Estimated study time: ~3 hours
> *How devices get their IP settings handed to them automatically — instead of someone typing an address into every laptop, phone, and printer by hand.*

## 🎯 What you'll be able to do after this chapter
- Explain *why* DHCP exists and the difference between automatic, dynamic, and manual (static) allocation.
- Walk through the **DORA** process step by step and say which messages are **broadcast** and which are **unicast**.
- Configure a Cisco router as a DHCPv4 server (pool, gateway, DNS, excluded addresses) and as a DHCPv4 client.
- Set up a **DHCP relay** with `ip helper-address` when the server is on a different subnet.
- Verify leases with `show ip dhcp binding`.
- Tell apart **SLAAC**, **stateless DHCPv6**, and **stateful DHCPv6**, and predict behaviour from the RA **M** and **O** flags.
- Recognise **DHCP starvation** and **DHCP spoofing** attacks and mitigate them with **DHCP snooping**.

---

## 1. Why DHCP exists (the big idea)

Imagine a school with 2,000 laptops. Every device needs four things to talk on the network: an **IP address**, a **subnet mask**, a **default gateway**, and a **DNS server**. Typing those into 2,000 machines by hand — and avoiding duplicates — would be a nightmare.

**DHCP (Dynamic Host Configuration Protocol)** solves this: a device boots up, shouts "anyone got an address for me?", and a DHCP server hands it a complete, ready-to-use configuration. No human involved.

DHCPv4 can hand out addresses three ways:

| Allocation type | What it does | Lease? |
| --- | --- | --- |
| **Manual** | Admin picks a specific address for a specific device (often tied to its MAC). | Permanent (no lease in the dynamic sense) |
| **Automatic** | Server permanently assigns an address from the pool to a device. | No lease — permanent |
| **Dynamic** | Server *leases* an address from the pool for a limited time, then reclaims it. | Yes — time-limited |

> 💡 **Exam tip:** **Dynamic allocation** is the one with a **lease** (a time limit). **Automatic allocation** also picks from the pool but assigns the address **permanently with no lease**. The exam loves to make you separate these two.

---

## 2. The DORA process (DHCPv4 in 4 messages)

A new client and the server exchange four messages. Remember the word **DORA**: **D**iscover → **O**ffer → **R**equest → **A**cknowledge.

The single most-tested detail is **which messages are broadcast and which are unicast**. Here it is as a step list:

1. **DHCPDISCOVER — client → server (BROADCAST).**
   The client has no IP yet, so it doesn't know where any server is. It sends a broadcast at *both* Layer 2 (`FF:FF:FF:FF:FF:FF`) and Layer 3 (`255.255.255.255`) to find any DHCP server on the segment.
   *Source IP `0.0.0.0`, destination IP `255.255.255.255`.*

2. **DHCPOFFER — server → client (UNICAST).**
   A server that hears the Discover reserves an address and offers it. It replies **unicast**, using the server's MAC as source and the **client's MAC** as the Layer 2 destination. (It learned the client's MAC from the Discover.)

3. **DHCPREQUEST — client → server (BROADCAST).**
   The client formally accepts **one** offer. It sends this as a **broadcast** so the chosen server knows it's accepted *and* every other server that made an offer knows to withdraw theirs and put the address back in the pool.

4. **DHCPACK — server → client (UNICAST).**
   The chosen server confirms the lease. Before replying it typically pings the address (to be sure it's free) and creates an ARP entry. It then sends a **unicast DHCPACK**. When the client receives it, it records the settings, does a final ARP check, and starts using the address.

```
   CLIENT                                   SERVER
     |  --- DHCPDISCOVER (broadcast) --->     |   "Any servers out there?"
     |  <---- DHCPOFFER (unicast) -------     |   "Here, take 192.168.10.10"
     |  --- DHCPREQUEST (broadcast) --->      |   "I accept that one"
     |  <----- DHCPACK (unicast) --------     |   "Confirmed. It's yours."
```

**Lease renewal:** when the lease is partway through, the client renews by sending a **DHCPREQUEST directly (unicast) to the server** that gave it the address. If no DHCPACK comes back in time, it falls back to **broadcasting** a DHCPREQUEST so any server can extend it. The server confirms with a DHCPACK.

**Ports:** DHCPv4 runs over **UDP**. Client → server uses **source port 68, destination port 67**. Server → client uses source 67, destination 68.

> 💡 **Exam tip:** Memorise the broadcast/unicast pattern as **B-U-B-U**: Discover = **B**roadcast, Offer = **U**nicast, Request = **B**roadcast, Ack = **U**nicast. And **client uses port 68, server uses port 67** ("68 sends, 67 serves" — or recall it's the *lower* number, 67, that the *server* listens on).

---

## 3. Configuring a Cisco router as a DHCPv4 server

A router can do the server's job. There are three steps, in this order.

**Step 1 — Exclude addresses you don't want handed out.** Routers, servers, and printers usually have static addresses. Exclude them so the pool never leases them out. Do this in *global* config, **before** defining the pool.

**Step 2 — Create the pool** with `ip dhcp pool <name>`. This drops you into DHCP-pool config mode.

**Step 3 — Define the pool's settings:** the `network` (subnet to lease from), the `default-router` (gateway — this is what becomes the client's default gateway), and optional items like `dns-server`, `domain-name`, and `lease`.

```cisco
! Step 1: keep .1–.9 (router/servers) out of the pool
R1(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.9

! Step 2: create the pool
R1(config)# ip dhcp pool LAN-POOL

! Step 3: configure what clients receive
R1(dhcp-config)# network 192.168.10.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.10.1
R1(dhcp-config)# dns-server 8.8.8.8
R1(dhcp-config)# domain-name example.com
R1(dhcp-config)# lease 3        ! 3-day lease (default is 1 day)
```

- The DHCP service is **enabled by default**. Turn it off with `no service dhcp`, back on with `service dhcp`.
- View just the DHCP lines of the config with `show running-config | section dhcp`.

> 💡 **Exam tip:** `ip dhcp excluded-address` is configured in **global config mode**, *not* inside the pool. And `default-router` (not "gateway") is the command that sets the clients' default gateway.

### Router as a DHCPv4 *client*
Sometimes the router itself should *get* its address from an upstream server (e.g. an ISP). On the interface:

```cisco
R1(config)# interface g0/0/1
R1(config-if)# ip address dhcp
R1(config-if)# no shutdown
```

### Verification

```cisco
R1# show ip dhcp binding      ! leases handed out: IP ↔ MAC, lease expiry
R1# show ip dhcp pool         ! pool usage / how many addresses leased
R1# show running-config | section dhcp
```

`show ip dhcp binding` is the key one — it's the **binding table** showing which client (by MAC/identifier) holds which leased IP and when it expires.

---

## 4. DHCP Relay — when the server is on another subnet

**The problem:** DHCPDISCOVER is a **broadcast**, and routers **do not forward broadcasts**. So if clients are on `192.168.10.0/24` but the DHCP server lives on a different subnet (say `192.168.11.0/24`), the Discover never reaches the server and clients get nothing.

**The fix:** make the client-side router a **relay agent** with `ip helper-address`. The relay catches the client's broadcast and forwards it as a **unicast** to the real DHCP server.

**Scenario:** clients on R1's `g0/0/0` interface (`192.168.10.1`), DHCP server at `192.168.11.5` reachable out another interface.

```cisco
R1(config)# interface g0/0/0          ! the interface that RECEIVES client broadcasts
R1(config-if)# ip helper-address 192.168.11.5
```

> ❌ Common mistake: putting `ip helper-address` on the interface facing the *server*. It goes on the interface facing the **clients** — the one where the broadcasts arrive.

**Bonus detail the slides flag:** `ip helper-address` doesn't only forward DHCP. By default it relays **eight** UDP services:

| Port | Service | | Port | Service |
| --- | --- | --- | --- | --- |
| 37 | Time | | 68 | DHCP/BOOTP server |
| 49 | TACACS | | 69 | TFTP |
| 53 | DNS | | 137 | NetBIOS name service |
| 67 | DHCP/BOOTP client | | 138 | NetBIOS datagram service |

---

## 5. DHCPv6 — three flavours and the M/O flags

IPv6 has more than one way to auto-configure an address. The router's **ICMPv6 Router Advertisement (RA)** message carries two flags that *tell the client what to do*:

- **M flag — Managed Address Configuration.** "Get your address from a (stateful) DHCPv6 server."
- **O flag — Other Configuration.** "Get *other* info (like DNS) from a DHCPv6 server."

Combine those flags and you get three methods:

| Method | RA flags | Where the **address** comes from | Where **other info (DNS)** comes from | Server keeps state? |
| --- | --- | --- | --- | --- |
| **SLAAC** | M=0, O=0 | Client builds it itself from the RA prefix (+ EUI-64 or random) | From the RA | No server needed at all |
| **Stateless DHCPv6** | M=0, **O=1** | Client builds it itself via SLAAC | From a **DHCPv6 server** | No (no address records) |
| **Stateful DHCPv6** | **M=1** | From a **DHCPv6 server** | From the DHCPv6 server | **Yes** — server tracks address ↔ client |

One-line intuitions:
- **SLAAC** = "I'll make my own address from the prefix the router gave me." No server. (Relies on **ICMPv6** RA/RS — the A flag in the prefix.)
- **Stateless DHCPv6** = "I'll make my own address, but I need a server just for the *extra* settings like DNS."
- **Stateful DHCPv6** = "Give me everything — address *and* settings — like DHCPv4 does." The server keeps a record (state) of who has what.

Setting the flags on the router:
```cisco
R1(config-if)# ipv6 nd managed-config-flag    ! sets M=1 (stateful)
R1(config-if)# ipv6 nd other-config-flag      ! sets O=1 (stateless extra info)
```

**DHCPv6 message exchange** (UDP; client→server uses **port 547**, server→client uses **port 546**):
1. Client sends **SOLICIT** to multicast `FF02::1:2` (all-DHCPv6-servers, link-local scope — routers don't forward it).
2. Server(s) reply with **ADVERTISE** ("I'm here").
3. The client then asks:
   - **Stateless** client → **INFORMATION-REQUEST** (just wants config params like DNS).
   - **Stateful** client → **REQUEST** (wants an address *and* params).
4. Server sends **REPLY** with the requested info.

**DHCPv6 relay** (server on a different network): on the interface facing the client,
```cisco
R1(config-if)# ipv6 dhcp relay destination 2001:db8:acad:2::5
```

> 💡 **Exam tip:** Read the flags like a checklist. **M=1 → stateful** (server gives the address). **M=0, O=1 → stateless** (client makes the address, server gives DNS). **M=0, O=0 → pure SLAAC** (no server involved). The exam will give you flag values and ask which method is in use.

> 💡 **Exam tip:** Don't mix up the ports. **DHCPv4**: client 68 / server 67. **DHCPv6**: client 546 / server 547.

---

## 6. DHCP attacks and DHCP snooping

DHCP trusts whoever answers — and that trust is the vulnerability. Two classic attacks:

- **DHCP starvation:** the attacker floods the server with fake DHCP requests (each with a spoofed MAC), using tools like *Gobbler*, until the **entire pool is exhausted**. Real clients can't get an address — a **denial-of-service (DoS)**.
- **DHCP spoofing:** the attacker stands up a **rogue (fake) DHCP server**. It answers clients before the real server does and hands out bad settings — most dangerously a **default gateway pointing at the attacker**, so all the victim's traffic flows through the attacker (a man-in-the-middle).

### Mitigation: DHCP snooping
**DHCP snooping** is a *switch* feature that decides whether DHCP messages came from a **trusted** or **untrusted** port.

- **Trusted ports** can pass *all* DHCP messages (including server messages: OFFER/ACK). You trust them, so you point them at your real infrastructure — **uplink/trunk ports and the port connected to the legitimate DHCP server**.
- **Untrusted ports** (all **access ports** by default) may only **source client requests** (DISCOVER/REQUEST). If a device on an untrusted port tries to send a **server** reply (an OFFER/ACK — i.e. acting as a rogue server), the switch **shuts the port down**.
- The switch builds a **DHCP snooping binding table** mapping each untrusted device's **MAC address ↔ leased IP** (plus port/VLAN). This binds the two together and underpins other protections.

> 💡 **Exam tip:** **All interfaces are untrusted by default.** You explicitly *trust* the ports facing the real DHCP server and the uplinks. Untrusted ports can send **requests only**; a rogue server reply on an untrusted port gets the **port disabled**.

Configuration steps:
```cisco
! Step 1: turn snooping on globally (and per VLAN)
S1(config)# ip dhcp snooping
S1(config)# ip dhcp snooping vlan 10

! Step 2: mark the port to the real DHCP server / uplink as trusted
S1(config)# interface g0/1
S1(config-if)# ip dhcp snooping trust

! Step 3: rate-limit DISCOVER messages on untrusted (access) ports
S1(config)# interface range f0/1 - 24
S1(config-if-range)# ip dhcp snooping limit rate 6     ! 6 packets/sec
```

The **rate limit** (step 3) is the specific defence against **starvation** — it caps how many DHCP messages an untrusted port can send per second, so an attacker can't flood the pool.

---

## 🧪 Hands-on (Packet Tracer)
- **SRWE 1.6.1 "Implement a Small Network":** the capstone-style lab where DHCP is commonly configured on the router (or a server). Practise the full pool: `ip dhcp excluded-address`, `ip dhcp pool`, `network`, `default-router`, `dns-server`, then confirm clients pull addresses and check `show ip dhcp binding`.
- **General device-configuration labs:** wherever end devices are set to "DHCP" instead of static, trace the DORA exchange and watch the binding table populate.
- Try moving the DHCP server to a different subnet and fixing connectivity with `ip helper-address` on the client-facing interface — this is the relay scenario made concrete.

## 🧠 Key terms to memorize
| Term | Meaning in one line |
| --- | --- |
| **DHCP** | Protocol that auto-assigns IP + mask + gateway + DNS to clients. |
| **Lease** | Time-limited grant of an address (dynamic allocation). |
| **DORA** | Discover, Offer, Request, Acknowledge — the 4-message handshake. |
| **default-router** | The pool command that sets clients' default gateway. |
| **ip dhcp excluded-address** | Global command reserving addresses the server must not lease. |
| **ip helper-address** | Interface command turning a router into a DHCP relay agent. |
| **show ip dhcp binding** | Displays the lease/binding table (IP ↔ client). |
| **SLAAC** | IPv6 host builds its own address from the RA prefix; no server. |
| **Stateful DHCPv6** | Server assigns the IPv6 address *and* settings (M flag = 1). |
| **Stateless DHCPv6** | Host self-assigns address; server gives only DNS etc. (O flag = 1). |
| **M / O flags** | RA flags telling clients to use a server for the address (M) / other info (O). |
| **DHCP starvation** | Flooding the server to exhaust the address pool (DoS). |
| **DHCP spoofing** | Rogue DHCP server feeding clients a malicious gateway. |
| **DHCP snooping** | Switch feature separating trusted vs untrusted ports for DHCP. |

## ⚡ Exam tips & traps (recap)
- **B-U-B-U:** Discover = broadcast, Offer = unicast, Request = broadcast, Ack = unicast.
- **DHCPv4 ports: client 68, server 67.** **DHCPv6 ports: client 546, server 547.**
- **Dynamic** allocation has a **lease**; **automatic** allocation is **permanent, no lease**.
- `ip dhcp excluded-address` lives in **global** config; `default-router`/`network`/`dns-server` live **inside the pool**.
- `ip helper-address` goes on the **client-facing** interface, points to the **server's IP**, and forwards **8** UDP services (not just DHCP).
- Routers don't forward broadcasts — that's *why* relay exists.
- **M=1 → stateful**, **M=0/O=1 → stateless**, **M=0/O=0 → SLAAC.**
- DHCPv6 SOLICIT goes to multicast **FF02::1:2** (link-local scope, not routed).
- DHCP snooping: **all ports untrusted by default**; trust only uplinks and the real server's port; untrusted ports may source **requests only**; **rate-limit** counters starvation.
- Verify leases with **`show ip dhcp binding`**.

## ✅ Quick self-check
1. In the DORA process, which two messages are **broadcast** and which two are **unicast**? Why is the Request a broadcast?
2. On which interface do you put `ip helper-address`, and what address do you point it at?
3. A router's RA has **M=0, O=1**. How does a client get its IPv6 address, and where does it get DNS?
4. Which DHCP attack exhausts the address pool, and which switch setting specifically counters it?
5. Which command shows you which client currently holds which leased IPv4 address?
