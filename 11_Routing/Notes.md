# Chapter 11 — Routing (Static and Dynamic)

> **Day 6 of 7** · Estimated study time: ~3 hours
> *This chapter is about how a router decides where to send a packet next — by looking up the destination in its routing table — and the two ways routes get into that table: you type them in by hand (static) or the routers teach each other (dynamic).*

## 🎯 What you'll be able to do after this chapter
- Explain what a router does and how it uses the **longest-match** rule to choose a route.
- Describe the three packet-forwarding methods (process switching, fast switching, CEF) and why CEF is preferred.
- Read a routing table: identify the code (C, L, S, O, D, R), the administrative distance, and the metric.
- Write `ip route` and `ipv6 route` commands for next-hop, exit-interface, fully-specified, default, and floating static routes.
- Recall administrative-distance values (Connected 0, Static 1, EIGRP 90, OSPF 110, RIP 120) and use them to predict which route wins.
- Compare static vs. dynamic routing, and distance-vector vs. link-state protocols, and pick the right one for a scenario.

---

## 1. Introduction — what a router actually does

Think of a router as a **post office at a crossroads**. It has several roads leaving it (interfaces), and each road leads to a different town (IP network). When a letter (packet) arrives, the router reads the destination address and decides which road sends it closest to its destination.

A **router** connects multiple networks. Each interface belongs to a *different* IP network. Its two primary jobs are:

1. **Determine the best path** to forward a packet, based on its routing table.
2. **Forward the packet** toward its destination.

### The longest-match rule
When a packet arrives, the router compares the destination IP against every route in its table and picks the **longest match** — the route whose network prefix has the *greatest number of far-left matching bits* with the destination address.

> Worked example: a packet is destined for `172.16.10.5`. The table has:
> - `172.16.0.0/16` → matches the first 16 bits
> - `172.16.10.0/24` → matches the first 24 bits ✅ **(longest match — this one wins)**
>
> The more specific (longer prefix) route always wins, regardless of metric or AD.

> 💡 **Exam tip:** "Best path = longest match." A /24 always beats a /16 for an address inside both, because /24 matches more bits. AD and metric only break ties *between routes of the same prefix length and the same network*.

### What lives in the routing table
- **Directly connected routes** — networks attached to the router's own active interfaces.
- **Remote routes** — networks on *other* routers; learned either statically (you type them) or dynamically (a protocol learns them).
- **Default route** — a catch-all used when no specific route matches the destination.

---

## 2. Choosing between paths — metric and administrative distance

These two values are the cause of most exam confusion. Keep them separate in your head:

### Metric — "which path is shortest, *within one protocol*"
A **metric** is a number that measures the "distance" to a network. **Lower metric = better path.** Each routing protocol measures distance differently:

| Protocol | Metric |
|---|---|
| **RIP** | Hop count (number of routers crossed) |
| **OSPF** | Cost (based on cumulative bandwidth) |
| **EIGRP** | Bandwidth + delay (and optionally load, reliability) |

When a router has two paths to the same network with **equal metrics**, it uses both at once — this is **equal-cost load balancing**.

### Administrative distance (AD) — "which *source* do I trust most"
If two *different sources* (say OSPF and a static route) both offer a route to the same network, the router can't compare their metrics — they're measured in different units. So it uses **administrative distance**, the "trustworthiness" of the *source*. **Lower AD = more trusted = installed in the table.**

#### Administrative-distance reference table (memorize this)
| Route source | AD |
|---|---|
| Directly connected (C) | **0** |
| Static route (next-hop or exit-interface) | **1** |
| EIGRP summary route | 5 |
| External BGP (eBGP) | 20 |
| **EIGRP (internal)** | **90** |
| IGRP | 100 |
| **OSPF** | **110** |
| IS-IS | 115 |
| **RIP** | **120** |
| External EIGRP | 170 |
| Internal BGP (iBGP) | 200 |
| Unknown / unusable | 255 (never installed) |

> 💡 **Exam tip:** The order from most to least trusted: **Connected (0) → Static (1) → EIGRP (90) → OSPF (110) → RIP (120)**. If you remember those five numbers you can answer most AD questions. A higher AD means the route is *less* preferred — that is exactly how a floating static route works (see §6).

---

## 3. Packet Forwarding — three switching methods

Once the router knows the route, *how* does it physically move the packet? Three mechanisms, from oldest/slowest to modern/fastest:

1. **Process switching** — Every packet goes up to the CPU (control plane), which looks up the destination in the routing table, finds the exit interface, and forwards it. Done for **every single packet** → very slow.
2. **Fast switching** — The first packet is process-switched, but the next-hop info is stored in a **fast-switching cache**. Later packets to the *same* destination reuse the cached entry without bothering the CPU.
3. **Cisco Express Forwarding (CEF)** — The **preferred** modern method. CEF builds two tables *in advance*:
   - **FIB (Forwarding Information Base)** — pre-computed next-hop and interface info for all routes.
   - **Adjacency table** — the Layer 2 (MAC) info needed to build the frame.
   Because everything is pre-computed, CEF doesn't need the CPU per-packet and doesn't depend on traffic "warming up" a cache.

> 💡 **Exam tip:** CEF = preferred, uses **FIB + adjacency table**, pre-computed. Process switching = CPU does it *per packet* = slowest.

### The forwarding decision (what happens to an arriving packet)
1. **Directly connected network?** → forward the packet straight to the destination device.
2. **Remote network?** → forward to the **next-hop router** named in the route entry.
3. **No specific match, but a default route exists?** → send it out the **default route**.
4. **No match and no default route?** → **drop the packet.** ❌

---

## 4. Reading the Routing Table

Each route entry tells you seven things:

1. **Route source / code** — *how* the route was learned (see codes below).
2. **Destination network** — the network/prefix.
3. **Administrative distance** — shown in brackets `[AD/metric]` (left number).
4. **Metric** — the right number in `[AD/metric]`.
5. **Next-hop** — IP of the next router (`via x.x.x.x`).
6. **Route timestamp** — how long ago it was learned.
7. **Outgoing interface** — exit interface toward the destination.

### Common routing-table codes
| Code | Meaning |
|---|---|
| **C** | Directly connected network |
| **L** | Local route — the interface's own IP, always a `/32` (IPv4) or `/128` (IPv6) host route |
| **S** | Static route |
| **S\*** | Static default route (the `*` marks the gateway of last resort) |
| **O** | OSPF |
| **D** | EIGRP (D = "Dual" algorithm) |
| **R** | RIP |
| **B** | BGP |

> Example line: `O 10.2.2.0/24 [110/65] via 192.168.1.1, 00:05:11, Serial0/0/0`
> Read it as: OSPF route to 10.2.2.0/24, AD **110**, metric **65**, reached via next-hop 192.168.1.1, out Serial0/0/0.

> 💡 **Exam tip:** When you put an IP on an active interface and bring it up, the router auto-adds **two** entries: a **C** route for the network and an **L** host route (`/32`) for the interface IP itself. If a directly-connected network is *missing* from the table, the usual cause is the interface is **administratively down** (you forgot `no shutdown`).

---

## 5. Static Routes — typing routes in by hand

A **static route** is one you configure manually. It is *not* updated automatically — if the topology changes, **you** must reconfigure it. Marked with **S** in the table.

**Benefits:** improved security, low resource use (no bandwidth or CPU spent advertising routes), predictable.
**Main disadvantage:** no automatic reconfiguration when the topology changes — it doesn't scale to large/changing networks.

**Best used for:**
- Small networks not expected to grow.
- Reaching a **stub network** (a network with only one way in/out — one neighbor).
- A single **default route** to send everything toward an upstream router/ISP.
- Summarizing several contiguous networks into one route.
- A **backup** (floating) route in case a primary link fails.

### The `ip route` command — fully explained
```
Router(config)# ip route <network> <mask> {next-hop-ip | exit-intf} [AD]
```
| Part | Meaning |
|---|---|
| `network` | The destination network address you want to reach |
| `mask` | The subnet mask of that destination network |
| `next-hop-ip` | IP address of the next router (causes a **recursive lookup**) |
| `exit-intf` | Local interface to send the packet out (**directly attached** route, no recursion) |
| `AD` *(optional)* | Administrative distance; omit it and a static route defaults to **1**. Set it high to make a **floating** route. |

### The three flavours of IPv4 static route

**(a) Next-hop static route** — point at the *neighbor's IP*:
```
R1(config)# ip route 192.168.2.0 255.255.255.0 10.1.1.2
```
The router must do a **recursive lookup**: "to reach 192.168.2.0 I go to 10.1.1.2… now which interface reaches 10.1.1.2?" It looks that up too. Slightly more work.

**(b) Exit-interface (directly attached) static route** — point at *your own interface*:
```
R1(config)# ip route 192.168.2.0 255.255.255.0 Serial0/0/0
```
No recursion needed — the router knows the exit interface immediately. Best on point-to-point serial links.

**(c) Fully-specified static route** — name *both* the exit interface **and** the next-hop IP:
```
R1(config)# ip route 192.168.2.0 255.255.255.0 Serial0/0/0 10.1.1.2
```
This **eliminates the recursive lookup** *and* removes ambiguity on multi-access links (like Ethernet) where many neighbors share one segment. Use it when the exit interface alone isn't enough to know *which* neighbor to send to.

> 💡 **Exam tip:** "Avoid recursive lookups" + "multi-access/Ethernet next-hop concern" → **fully-specified** route (exit-intf *and* next-hop IP). The two pieces of info needed are the **exit interface ID** and the **next-hop IP**.

### Default static route — the catch-all
`0.0.0.0 0.0.0.0` means "any destination". Use it when a router has only one way out (toward a central router or ISP):
```
R1(config)# ip route 0.0.0.0 0.0.0.0 10.1.1.2
       (or)  ip route 0.0.0.0 0.0.0.0 Serial0/0/0
```
This becomes the **gateway of last resort**, shown as `S*` in the table.

> 💡 **Exam tip:** A default route is configured on the **edge/ISP-facing router** and on a **stub router** — the routers with a single exit point. It is NOT configured on a router that has many specific routes and changing topology.

### IPv6 static routes
Same ideas, different command:
```
R1(config)# ipv6 route <ipv6-prefix>/<length> {ipv6-address | exit-intf}
```
- Next-hop (recursive): `ipv6 route 2001:db8:2::/64 2001:db8:1::2`
- Directly attached: `ipv6 route 2001:db8:2::/64 Serial0/0/0`
- Fully specified: `ipv6 route 2001:db8:2::/64 Serial0/0/0 2001:db8:1::2`
- Default IPv6 route: `ipv6 route ::/0 2001:db8:1::2`

> 💡 **Exam tip (IPv6, very common):** If the next-hop you use is an IPv6 **link-local address** (starts `FE80::`), you **must** use a **fully-specified** route (include the exit interface). Link-local addresses are *not* in the IPv6 routing table because they're only unique on one link — so the router can't recurse on them.

### Worked example — wiring up a small network
Topology: `R1 — [10.1.1.0/30] — R2`, R1 LAN `192.168.1.0/24`, R2 LAN `192.168.2.0/24`. R1's serial = `10.1.1.1`, R2's serial = `10.1.1.2`.

```
R1(config)# ip route 192.168.2.0 255.255.255.0 10.1.1.2   ! reach R2's LAN
R2(config)# ip route 192.168.1.0 255.255.255.0 10.1.1.1   ! reach R1's LAN
```
Both directions are needed. A classic exam fault: "Users on R2's LAN can't be reached" → **R1 is missing the static route to R2's LAN** (or vice-versa). Routing must be configured **both ways**.

### Host routes and local routes
A **host route** is a `/32` (IPv4) or `/128` (IPv6) — a single address. Routers add an **L** (local) host route automatically for each configured interface IP. If you see a `/32` or `/128` host route in the table, ask *how it got there*: an **L** route was installed automatically when the interface IP was configured; an **S** `/32` was typed manually.

### Troubleshooting commands
`ping` · `traceroute` · `show ip route` · `show ip interface brief` · `show cdp neighbors detail`

---

## 6. Floating Static Routes — the backup link

A **floating static route** is a backup route configured with a **higher AD** than the primary route, so it "floats" unused until the primary disappears.

How it works: the primary route (say, OSPF AD 110) is more trusted, so it's installed. The floating static sits with a *higher* AD and stays out of the table. If the primary route goes down, the floating static — now the best remaining option — gets installed automatically.

> Worked example: primary route is learned by **OSPF (AD 110)**. To back it up:
> ```
> R1(config)# ip route 172.16.32.0 255.255.224.0 Serial0/0/0 120
> ```
> AD **120 > 110**, so it only activates if the OSPF route fails. (If you set it *lower* than 110, it would wrongly take over even while OSPF is up — a common exam trap.)

> 💡 **Exam tip:** The fix for "backup link is being used even when the primary is up" is to **raise the floating route's AD above the primary's AD**. The AD value goes at the **end** of the `ip route` command. To back up RIP (AD 120) use 121; to back up OSPF (110) use a value > 110; to back up EIGRP (90) use > 90.

---

## 7. Dynamic Routing — routers teach each other

Instead of typing every route, **dynamic routing protocols** let routers **automatically share** information about which networks are reachable and their status. They:
- Discover networks (and neighboring routers).
- Maintain routing tables and pick the **best path** by metric.
- **Automatically find a new best path** when the topology changes — the big advantage over static.

After all routers have exchanged and agreed on their tables, the network has **converged**.

### Main components of a dynamic routing protocol
- **Data structures** — tables/databases kept in RAM.
- **Routing protocol messages** — used to find neighbors and exchange route info.
- **Algorithm** — computes the best path.

### How protocols are classified
**IGP vs EGP — *where* they run:**
- **IGP (Interior Gateway Protocol)** — routing *within* one organization/autonomous system. RIP, EIGRP, OSPF.
- **EGP (Exterior Gateway Protocol)** — routing *between* organizations/autonomous systems. **BGP** (runs the Internet).

**Distance-vector vs link-state — *how* they learn (IGPs):**

| | **Distance-vector** | **Link-state** |
|---|---|---|
| Idea | "Routing by rumor" — tells neighbors its *whole table* | Each router builds a full *map* of the topology |
| Knowledge | Only knows direction + distance to a network | Knows the entire network topology |
| Examples | **RIP**, **EIGRP** (advanced DV) | **OSPF**, IS-IS |
| Updates | Periodic / on change to neighbors | Floods link-state info, then computes shortest path (SPF) |
| Metric | RIP: hop count; EIGRP: bandwidth+delay | OSPF: cost (bandwidth) |
| Scaling | Simpler, smaller networks | Faster convergence, larger networks |

> 💡 **Exam tip:** **OSPF = link-state** (builds a topology map, uses cost). **RIP = distance-vector** (hop count, max 15 hops). **EIGRP = Cisco advanced distance-vector**. **BGP = the EGP / path-vector** that connects autonomous systems on the Internet.

### Static vs dynamic — when to use which
- **Static:** small, stable networks; stub networks; default routes; you want control and minimal overhead.
- **Dynamic:** larger networks, or anywhere the **topology changes often** (links fail, networks added) — the protocol re-routes automatically.

> 💡 **Exam tip:** "Lots of topology changes / growing network" → **dynamic**. "Stub / single exit / small & stable / minimize CPU & bandwidth" → **static (often a default route)**.

---

## 🧪 Hands-on (Packet Tracer)
- **ITN 10.1.4 Configure Initial Router Settings** — hostname, passwords, banner, saving config: the baseline before any routing works.
- **SRWE 1.4.7 Configure Router Interfaces** — assign IPs to interfaces and `no shutdown`; this is what creates the **C** and **L** routes.
- **1.5.10 Verify Directly Connected Networks** — use `show ip route` / `show ip interface brief` to confirm connected networks appear; practice spotting a down interface.
- **1.6.1 Implement a Small Network** — put it together: interfaces + static routes so two LANs can reach each other end-to-end.

## 🧠 Key terms to memorize
| Term | Meaning in one line |
|---|---|
| Longest match | The route with the most matching far-left bits wins |
| Administrative distance (AD) | Trustworthiness of a route *source*; lower wins |
| Metric | Distance within one protocol; lower is the better path |
| Equal-cost load balancing | Using multiple equal-metric paths at once |
| CEF | Preferred forwarding; pre-built FIB + adjacency table |
| FIB | Forwarding Information Base — pre-computed next-hop/interface info |
| Static route (S) | Manually configured route, default AD 1 |
| Recursive lookup | Resolving a next-hop IP to an exit interface in a second lookup |
| Fully-specified route | Static route with both exit interface and next-hop IP |
| Default route | `0.0.0.0/0` (or `::/0`) catch-all; gateway of last resort |
| Floating static route | Backup route with a *higher* AD than the primary |
| Stub network | Network reached by a single route / one neighbor |
| Local (L) route | Auto-added `/32` or `/128` for an interface's own IP |
| Convergence | All routers agree on the topology / routing tables |
| IGP / EGP | Routing inside one AS / between autonomous systems |

## ⚡ Exam tips & traps (recap)
- **AD values to memorize:** Connected **0**, Static **1**, EIGRP **90**, OSPF **110**, RIP **120**. Lower = preferred.
- **Best path = longest match** (most matching bits) — beats AD and metric for choosing the route.
- **Floating static = higher AD than the primary.** Set it just above the primary's AD (RIP→121, OSPF→>110, EIGRP→>90). The AD goes at the **end** of `ip route`.
- **Fully-specified route = exit-interface + next-hop IP** → avoids recursive lookups and multi-access ambiguity.
- **IPv6 link-local next-hop (FE80::) → must be fully specified** (include exit interface).
- A directly connected network missing from the table → interface is **not** `no shutdown`.
- Static routing must be configured in **both directions**; a one-way config breaks return traffic.
- **CEF** is preferred and uses **FIB + adjacency table**; **process switching** is slowest (CPU per packet).
- Default route is for **stub / ISP-edge** routers (single exit). It shows as **S\*** = gateway of last resort.
- No matching route **and** no default route → packet is **dropped**.
- **OSPF = link-state**, **RIP/EIGRP = distance-vector**, **BGP = EGP** between autonomous systems.

## ✅ Quick self-check
1. A packet for `172.16.10.5` matches both `172.16.0.0/16` and `172.16.10.0/24`. Which route is used, and why?
2. OSPF (AD 110) already provides a route to `10.5.0.0/16`. Write a floating static backup out `S0/0/0` to next-hop `10.1.1.2` that only activates if OSPF fails.
3. Why must a static IPv6 route that uses a link-local next-hop also include the exit interface?
4. Your `ip route` for a remote LAN is configured, the interface is up, but the network won't appear/work. Name two likely causes.
5. Classify each: RIP, OSPF, EIGRP, BGP — distance-vector or link-state, and IGP or EGP?
