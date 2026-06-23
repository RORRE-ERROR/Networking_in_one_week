# 🧪 Graded Lab Guide — WIA1005

Walkthroughs for the **graded practical labs** in your `Lecture1/` folder (`WIA1005-LT-01.pdf`, `WIA1005-LT-02.pdf`, and the `WIA1005-L2` IPv6 Packet Tracer activity). The theory behind each lives in the chapter Notes — this guide turns it into the exact steps you configure.

> In every task, **X = your rack number**, **Z = your PC number**. Substitute your own values.

---

## Lab Task 1 — Basic Connectivity *(Week 4 · 5%)*
**Goal:** connect PCs + a switch, address them, and prove they can ping within and across racks. *(Theory: Chapters 2 & 5.)*

**Addressing**
| Device | IP Address | Subnet Mask |
|---|---|---|
| SW (switch) | `10.26.1.23X` (X = rack 1–8) | `255.255.255.0` |
| PC | `10.26.1.Z` (Z = your PC number) | `255.255.255.0` |

All devices share network `10.26.1.0/24`, so everything is one subnet → no router needed; pings work directly once addresses are set.

**Steps**
1. Cable PCs to the switch per the topology (straight-through, PC↔switch).
2. On each PC: set IP `10.26.1.Z` + mask `255.255.255.0` (gateway not needed within one subnet).
3. Give the switch a management IP on its SVI:
   ```
   Switch> enable
   Switch# configure terminal
   Switch(config)# hostname SW
   SW(config)# interface vlan 1
   SW(config-if)# ip address 10.26.1.23X 255.255.255.0
   SW(config-if)# no shutdown
   SW(config-if)# end
   SW# copy running-config startup-config
   ```
4. **Verify:** `ping 10.26.1.Z` between PCs and to the switch — same rack *and* other racks (all are in `10.26.1.0/24`, so cross-rack pings succeed once switches are interconnected).

✅ **Pass criteria:** PC1 can ping PC2 and SW in its own rack *and* in other racks.

---

## Lab Task 2 — Dual Stack IPv4 + IPv6 *(Week 6 · 5%)*
**Goal:** configure three subnets with **both** IPv4 and IPv6 on PC, switch, and router, and verify connectivity. *(Theory: Chapters 5, 6 & 11.)*

**Addressing** (X = your rack number)
| Net | IPv4 Network | IPv6 Network |
|---|---|---|
| N1 | `172.20.X.0/26` | `2001:X:X:1::/64` |
| N2 | `172.20.X.64/27` | `2001:X:X:2::/64` |
| N3 | `172.20.X.96/28` | `2001:X:X:3::/64` |

**IPv4 host-assignment rule:** **first** usable host → PC, **second** → switch, **last** usable host → router.
**IPv6 interface-ID rule:** Router `::100`, Switch `::200`, PC `::X` (rack number).

**Worked example for N1 = `172.20.X.0/26`** (block size 64 → hosts .1–.62, broadcast .63):
| Role | IPv4 | IPv6 |
|---|---|---|
| PC (first host) | `172.20.X.1 /26` | `2001:X:X:1::X` |
| Switch (second host) | `172.20.X.2 /26` | `2001:X:X:1::200` |
| Router (last host) | `172.20.X.62 /26` | `2001:X:X:1::100` |

> Apply the same first/second/last logic to N2 (`/27`: hosts .65–.94, last .94, broadcast .95) and N3 (`/28`: hosts .97–.110, last .110, broadcast .111).

**Router config (per LAN interface, dual-stack)**
```
Router(config)# ipv6 unicast-routing
Router(config)# interface g0/0
Router(config-if)# ip address 172.20.X.62 255.255.255.192        # N1 router = last host
Router(config-if)# ipv6 address 2001:X:X:1::100/64
Router(config-if)# ipv6 address fe80::100 link-local
Router(config-if)# no shutdown
```
Repeat on the other interfaces with the N2/N3 addresses (and their masks `/27`=`255.255.255.224`, `/28`=`255.255.255.240`).

**PC config:** set the IPv4 address/mask + the router's IPv4 as **default gateway**, and the IPv6 address `2001:X:X:n::X/64` + IPv6 gateway = the router's link-local or `::100`.

**Verify**
```
show ip interface brief          # IPv4 up/up
show ipv6 interface brief        # IPv6 addresses present
ping <other-subnet IPv4>         # cross-subnet (routed) reachability
ping <other-subnet IPv6>
```
✅ **Pass criteria:** every device can ping every other device over **both** IPv4 and IPv6.

---

## Lab — Configure IPv6 Addressing (`WIA1005-L2` Packet Tracer) *(Theory: Chapter 6)*
**Goal:** configure IPv6 on a router, servers, and clients, then verify. This mirrors ITN lab **12.6.6**.

**Addressing table**
| Device | Interface | IPv6 Address / Prefix | Gateway |
|---|---|---|---|
| R1 | G0/0 | `2001:db8:1:1::1/64` (+ `fe80::1` link-local) | — |
| R1 | G0/1 | `2001:db8:1:2::1/64` (+ `fe80::1`) | — |
| R1 | S0/0/0 | `2001:db8:1:a001::2/64` (+ `fe80::1`) | — |
| Sales / Billing / Accounting | NIC | `2001:db8:1:1::2/3/4` | `fe80::1` |
| Design / Engineering / CAD | NIC | `2001:db8:1:2::2/3/4` | `fe80::1` |
| ISP | S0/0/0 | `2001:db8:1:a001::1` | `fe80::1` |

**Part 1 — Router**
```
R1> enable
R1# configure terminal
R1(config)# ipv6 unicast-routing                       # enable IPv6 forwarding (REQUIRED first)
R1(config)# interface g0/0
R1(config-if)# ipv6 address 2001:db8:1:1::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown
```
Repeat for G0/1 (`2001:db8:1:2::1/64`) and S0/0/0 (`2001:db8:1:a001::2/64`), each with `fe80::1 link-local`.

**Parts 2 & 3 — Servers and Clients:** on each end device's IPv6 config, enter the address from the table with `/64` and set the **default gateway to `fe80::1`** (the router's link-local address).

**Part 4 — Verify**
```
show ipv6 interface brief        # confirm global + link-local addresses, up/up
show ipv6 route                  # connected /64s present
ping 2001:db8:1:2::2             # client-to-client across subnets
```
✅ **Pass criteria:** all hosts reach each other; `show ipv6 interface brief` shows both a global (`2001:db8…`) and a link-local (`fe80::1`) address on each router interface.

---

### Common lab pitfalls (exam-relevant)
- Forgot **`ipv6 unicast-routing`** → router won't forward IPv6 between subnets.
- Forgot **`no shutdown`** → interface stays down, route never appears.
- Wrong **mask on the interesting octet** (/26 vs /27 vs /28) → wrong "last host" address.
- Used the **network or broadcast address** as a host → invalid.
- IPv6 client gateway must be the router's **link-local** (`fe80::…`), not its global address.
