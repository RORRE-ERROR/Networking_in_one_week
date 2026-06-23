# ⭐ Review Before the Exam — WIA1005 Networking (All Chapters)

> **Read this last**, the day or two before your exam, *after* you've worked through the chapter Notes and Quizzes. It condenses all 13 chapters into the highest-yield facts, the must-memorize tables, and the traps that catch people out. If a line here surprises you, go back and re-read that chapter's `Notes.md`.

---

## 🧭 How to use this sheet
1. Read the **Must-Memorize Tables** below until you can reproduce them on scratch paper.
2. Skim each **Chapter Rapid-Recap** — these are your exam tips, collected.
3. Do the **Final Mixed Self-Test** at the bottom (cover the answers).
4. Re-take any chapter `Quiz.md` you scored below ~80% on.

---

## 📌 Must-Memorize Tables

### A. Well-known port numbers
| Port | Protocol | Transport |
|---|---|---|
| 20 / 21 | FTP (data / control) | TCP |
| 22 | SSH | TCP |
| 23 | Telnet | TCP |
| 25 | SMTP | TCP |
| 53 | DNS | **UDP** (TCP for zone transfer) |
| 67 / 68 | DHCP (server / client) | UDP |
| 69 | TFTP | UDP |
| 80 | HTTP | TCP |
| 110 | POP3 | TCP |
| 143 | IMAP | TCP |
| 443 | HTTPS | TCP |
| 546 / 547 | DHCPv6 (client / server) | UDP |

### B. Administrative Distance (lower = more trusted)
| Source | AD |
|---|---|
| Directly connected | 0 |
| Static route | 1 |
| EIGRP (internal) | 90 |
| OSPF | 110 |
| RIP | 120 |
| Unusable / unknown | 255 |

> Path-selection order: **Longest prefix match first** → if tie, **lowest AD** → if tie, **lowest metric**. A **floating static** route is just a static route with a *higher* AD so it stays out of the table until the primary fails.

### C. IPv4 subnet mask reference (the numbers that show up constantly)
| Prefix | Mask | Block size | Usable hosts |
|---|---|---|---|
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | **2** (point-to-point) |

**Formulas:** `#subnets = 2^(borrowed bits)` · `#usable hosts = 2^(host bits) − 2` · `block size = 256 − mask octet`.

### D. IPv6 address types
| Address / Prefix | Type |
|---|---|
| `2000::/3` | Global unicast |
| `FC00::/7` | Unique local |
| `FE80::/10` | **Link-local** |
| `FF00::/8` | Multicast |
| `FF02::1` | All-nodes multicast |
| `FF02::2` | All-routers multicast |
| `FF02::1:2` | All-DHCPv6-servers (SOLICIT) |
| `::1` | Loopback |
| `::` | Unspecified |

### E. OSI ↔ TCP/IP mapping
| OSI (7) | TCP/IP (4) | PDU |
|---|---|---|
| Application / Presentation / Session | **Application** | Data |
| Transport | **Transport** | Segment |
| Network | **Internet** | Packet |
| Data Link / Physical | **Network Access** | Frame / Bits |

> TCP/IP has **no** separate Session or Presentation layer. Remember PDUs by layer: **D**ata–**S**egment–**P**acket–**F**rame–**B**its.

### F. Cable type selection
| Cable | Connects | Example |
|---|---|---|
| Straight-through | **unlike** devices | PC↔switch, switch↔router |
| Crossover | **like** devices | switch↔switch, router↔router, PC↔PC |
| Rollover (console) | PC ↔ console port | management |

> **Auto-MDIX** makes straight-vs-crossover irrelevant on modern gear.

---

## 📚 Chapter Rapid-Recaps (your collected exam tips)

### Ch 1 — Network Communication
- End devices (PC, server, phone, printer, camera) vs intermediary devices (switch, router, AP, firewall).
- **Logical topology = IP scheme; physical topology = cabling/locations.**
- LAN (one org, small, fast) vs WAN (many providers, large); WANs connect LANs. Intranet = insiders; Extranet = trusted outsiders.
- **ADSL** download > upload; **SDSL** equal (business).
- Four characteristics of a reliable network: **fault tolerance (redundancy), scalability, QoS, security.** CIA = Confidentiality / Integrity / Availability.
- Trends: BYOD, collaboration, cloud (Public/Private/Hybrid/Community), smart home, powerline, WISP.

### Ch 2 — Protocols & Network Operating Systems
- OSI↔TCP/IP mapping (table E). **Encapsulation** adds headers going down; de-encapsulation strips them going up.
- On a **remote** send: destination **IP = final host** (unchanged end-to-end); destination **MAC = next hop / default gateway** (changes every hop).
- Standards: ISO made OSI; IEEE = 802.x; IETF = RFCs; IANA = numbers; ISOC promotes open development.
- Access: console/AUX = out-of-band; Telnet/SSH = in-band. **SSH encrypts; Telnet is plaintext.**
- Modes & prompts: `>` user EXEC, `#` privileged EXEC, `(config)#` global, `(config-if)#` interface.
- **`enable secret`** (encrypted) > `enable password`; `service password-encryption` hides the rest. **Save:** `copy running-config startup-config` (RAM volatile → NVRAM persists).

### Ch 3 — Network Access & Ethernet
- **Goodput ≤ Throughput ≤ Bandwidth.** Attenuation (distance) ≠ crosstalk (pair leakage) ≠ EMI/RFI.
- Fiber: **single-mode = laser, long-haul**; **multimode = LED, ≤ ~550 m**; immune to EMI/RFI.
- **LLC** (upper) identifies the L3 protocol; **MAC** (lower) does framing/addressing/media access.
- **CSMA/CD = wired half-duplex; CSMA/CA = wireless.** Full-duplex = collision-free.
- MAC = 48 bits / 12 hex; broadcast = `FF-FF-FF-FF-FF-FF`.
- **Store-and-forward** (Cisco default) checks the **FCS**; **cut-through** forwards after the dest MAC (fragment-free = first 64 bytes).
- **Switches break collision domains; routers break broadcast domains.**
- **ARP request = broadcast, reply = unicast.** IPv6 uses **ICMPv6 / NDP** instead of ARP.

### Ch 4 — Network & Transport Layer
- IP is **connectionless, best-effort, media independent**; reliability is **TCP's** job.
- IPv6 header: fewer fields, fixed length, **no checksum**, TTL → **Hop Limit**.
- 3-way handshake: **SYN → SYN/ACK → ACK**; teardown uses **FIN**. TCP header = 20 B, UDP = 8 B.
- "Reliable, ordered, every byte" → **TCP** (web, email, file). "Fast, loss-tolerant, delay-sensitive" → **UDP** (VoIP, streaming, DNS).
- `ping`/`traceroute` use **ICMP**; traceroute relies on **Time Exceeded** (TTL/Hop-Limit = 0).

### Ch 5 — IPv4 & Subnetting *(highest-yield — practice daily)*
- `#subnets = 2^borrowed`, `#hosts = 2^hostbits − 2`, `block size = 256 − mask octet`.
- Network = lowest address (host bits all 0); broadcast = one below the next subnet.
- An address equal to a network or broadcast value is **not** a usable host.
- Private: 10/8, 172.16–172.31, 192.168/16. APIPA `169.254.x.x` = DHCP failed. Loopback `127.x`.
- **VLSM: assign the largest subnet first**, never overlap, serial links get **/30**.
- First-octet classes: A 1–126, B 128–191, C 192–223, D 224–239, E 240–255 (127 = loopback).

### Ch 6 — IPv6
- 128 bits = 8 hextets. Compression: drop **leading** zeros; `::` = one run of all-zero hextets, **once only**.
- `FFFE` in the middle of the interface ID ⇒ built by **EUI-64** (from MAC, U/L bit flipped).
- **SLAAC** = no server tracks addresses (uses RS/RA). **Stateful DHCPv6** = server assigns + tracks. **RA flags: M=1 stateful, M=0/O=1 stateless, M=0/O=0 SLAAC.**
- IPv6 has **no broadcast** — multicast + anycast replace it. Enable routing with **`ipv6 unicast-routing`**.
- /48 site → **65,536** /64 subnets; subnet on nibble (4-bit) boundaries.

### Ch 7 — Application Layer
- Presentation = format/encrypt/compress; Session = maintain dialogs.
- **SMTP sends; POP3 downloads & deletes; IMAP keeps copies on server.** DNS records: A, AAAA, MX, NS, CNAME.
- DHCP = **DORA**. IOS ping symbols: **`!` success, `.` timeout, `U` unreachable.**
- Begin troubleshooting at the **last responsive hop**. `show cdp neighbors` = L2 check; `show ip route` = path to a network.

### Ch 8 — Switching
- Learns from **source MAC**, forwards by **destination MAC**; default age = 5 min. Unknown unicast/broadcast/multicast = **flood**.
- Switch = many collision domains, **one broadcast domain**.
- Management IP on an **SVI** + `ip default-gateway`. **SSH needs hostname + domain name** before RSA keys (≥1024-bit); `transport input ssh` + `login local`.
- Port security needs `switchport mode access` first. **Default violation = shutdown (err-disabled).** Sticky MACs saved to running-config; dynamic lost on reboot.

### Ch 9 — VLANs & Inter-VLAN
- **One VLAN = one broadcast domain = one subnet.** VLAN 1 is default; move native/management off it.
- 802.1Q tag = 4 bytes; native VLAN is **untagged**; native VLAN must **match** on both trunk ends.
- `dynamic auto` on both ends = **no trunk**; force with `switchport mode trunk`.
- Inter-VLAN ranking: legacy < **router-on-a-stick** (`encapsulation dot1q`) < **L3 switch** (`ip routing`, best).
- VLAN hopping: switch-spoofing → set ports to access + `nonegotiate`; double-tagging → native VLAN = unused.

### Ch 10 — DHCP
- **DORA = B-U-B-U** (Discover broadcast, Offer unicast, Request broadcast, Ack unicast).
- `ip dhcp excluded-address` = global; `network`/`default-router`/`dns-server` = inside pool.
- **`ip helper-address`** on the client-facing interface forwards DHCP across the router (relay).
- DHCPv6 RA flags: M=1 stateful, M=0/O=1 stateless, else SLAAC. Verify with `show ip dhcp binding`.
- DHCP snooping: ports untrusted by default; trust the server/uplink only.

### Ch 11 — Routing
- AD table (B). **Longest match wins** the path. Floating static = higher AD; AD goes at the **end** of `ip route`.
- **Fully-specified route = exit interface + next-hop IP** (avoids recursion). IPv6 **link-local next-hop must be fully specified**.
- Static routing must be configured **both directions**. No route + no default = packet **dropped** (default shows as **S\*** = gateway of last resort).
- **OSPF = link-state; RIP/EIGRP = distance-vector; BGP = EGP.** CEF (FIB + adjacency) is fastest; process switching slowest.

### Ch 12 — Wireless LAN
- Standards: WPAN 802.15, WLAN 802.11, WWAN cellular/802.16. **Dual-band = n & ax**; a/ac = 5 GHz, b/g = 2.4 GHz.
- SSID names the network (ESS); BSSID = AP's MAC. Beacon = passive (AP advertises); probe = active (client asks).
- Wi-Fi uses **CSMA/CA** (half-duplex, can't detect collisions) → RTS/CTS + ACK.
- Security: **WPA→TKIP, WPA2→AES/CCMP, WPA3-Personal→SAE**; Personal = PSK, Enterprise = RADIUS/802.1X. 2.4 GHz non-overlapping channels: **1, 6, 11.** First step on a new AP: change default admin password.

### Ch 13 — Build & Secure a Small Network
- Real-time priority traffic = **Voice & Video** (QoS). Redundancy = no single point of failure.
- **Worm** = self-replicating (best mitigation = **patch/update**); **virus** = attaches to a program; **Trojan** = user must run it. DDoS = many coordinated sources (botnet).
- SSH needs hostname + domain name + local user + RSA keys; `transport input ssh` kills Telnet. `exec-timeout` secures console & VTY.
- `show cdp neighbors detail` = neighbor IP/IOS; `terminal monitor` = see logs on a remote session.

---

## 🛠️ One-page IOS command reference

```bash
# --- Modes ---
enable                         # user EXEC > → privileged EXEC #
configure terminal             # → global config (config)#
exit / end                     # back one level / straight to privileged

# --- Basic device setup ---
hostname R1
enable secret class            # encrypted privileged-mode password
service password-encryption    # encrypt remaining plaintext passwords
banner motd #Authorized only#
line console 0
 password cisco
 login
 exec-timeout 5 0
line vty 0 4
 login local
 transport input ssh
copy running-config startup-config   # SAVE (RAM → NVRAM)

# --- Interface / addressing ---
interface g0/0
 ip address 192.168.1.1 255.255.255.0
 ipv6 address 2001:db8:acad:1::1/64
 no shutdown
ipv6 unicast-routing           # (global) enable IPv6 routing

# --- SSH ---
ip domain-name example.com
crypto key generate rsa        # choose ≥1024-bit modulus
username admin secret cisco

# --- Switch SVI management ---
interface vlan 1
 ip address 192.168.1.2 255.255.255.0
 no shutdown
ip default-gateway 192.168.1.1

# --- Port security ---
interface f0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 2
 switchport port-security mac-address sticky
 switchport port-security violation shutdown

# --- VLANs & trunk ---
vlan 10
 name SALES
interface f0/2
 switchport mode access
 switchport access vlan 10
interface f0/24
 switchport mode trunk
 switchport trunk native vlan 99

# --- Router-on-a-stick ---
interface g0/0.10
 encapsulation dot1q 10
 ip address 192.168.10.1 255.255.255.0

# --- Static & default routes ---
ip route 192.168.2.0 255.255.255.0 10.0.0.2        # next-hop
ip route 192.168.2.0 255.255.255.0 g0/0 10.0.0.2   # fully specified
ip route 0.0.0.0 0.0.0.0 10.0.0.2                  # default route
ip route 192.168.2.0 255.255.255.0 10.0.0.2 100    # floating (AD 100)
ipv6 route 2001:db8:2::/64 g0/0 fe80::2            # IPv6 (link-local → must specify exit if)

# --- DHCP (router as server) ---
ip dhcp excluded-address 192.168.1.1 192.168.1.10
ip dhcp pool LAN
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8
interface g0/1
 ip helper-address 10.0.0.5      # DHCP relay toward server

# --- Verification ---
show running-config
show ip interface brief          # interface status + IP
show ip route                    # routing table
show vlan brief                  # access-port VLAN assignments
show interfaces trunk            # trunk status / native VLAN
show ip dhcp binding             # DHCP leases
show cdp neighbors detail        # neighbor IP + IOS version
show mac address-table           # learned MACs
```

---

## 📝 Final Mixed Self-Test (cover the answers)

1. A `/27` mask gives how many usable hosts per subnet? → **30**
2. Which subnet does `172.16.10.100/26` belong to, and what is its broadcast? → **172.16.10.64**, broadcast **172.16.10.127** (hosts .65–.126)
3. Destination MAC of a frame sent to a host on a *different* network? → **the default gateway's MAC** (destination IP stays the remote host)
4. ARP request and ARP reply delivery types? → **request = broadcast, reply = unicast**
5. Order of the TCP three-way handshake? → **SYN, SYN/ACK, ACK**
6. Port numbers: SSH / DNS / HTTPS / SMTP? → **22 / 53 / 443 / 25**
7. Compress `2001:0db8:0000:0000:0000:ff00:0042:8329`. → **2001:db8::ff00:42:8329**
8. RA flags M=0, O=0 mean which IPv6 config method? → **SLAAC**
9. Which inter-VLAN method uses `encapsulation dot1q` subinterfaces? → **router-on-a-stick**
10. Default port-security violation mode? → **shutdown** (port → err-disabled)
11. AD of OSPF vs RIP vs static? → **110 / 120 / 1** (lower preferred)
12. What makes a static route a *floating* static route? → a **higher AD** than the primary route
13. Two pieces of info in a *fully-specified* static route? → **exit interface + next-hop IP** (avoids recursive lookup)
14. IOS ping output `U` means? → **destination unreachable**
15. Best mitigation for a worm? → **patch/update vulnerable systems**
16. Wi-Fi access method and why (not CSMA/CD)? → **CSMA/CA** — half-duplex, can't detect collisions
17. WPA2 encryption vs WPA3-Personal? → **AES/CCMP** vs **SAE**
18. Command to see which interface reaches a remote network? → **`show ip route`**
19. A host with `169.254.x.x` indicates what? → **DHCP failed** (APIPA)
20. Switch learns addresses from the ___ MAC and forwards by the ___ MAC. → **source / destination**

> Missed more than a couple? Open that chapter's `Notes.md` and `Quiz.md` and drill it. Then come back and run this list again. **You've got this. 🎓**
