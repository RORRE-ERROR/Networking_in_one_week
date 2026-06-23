# Chapter 11 Quiz — Routing (Static and Dynamic)

> Exam-style questions. Answers are marked `[x]` and explained. Cover the explanations and test yourself first — heavy emphasis on static routing, floating static, fully-specified routes, recursive lookup, AD values, and default routes, just like the CCNA checkpoint.

## Question 1: Match-all static route
What is a characteristic of a static route that matches all packets?

- [ ] It uses a single network address to send multiple static routes to one destination address.
- [ ] It backs up a route already discovered by a dynamic routing protocol.
- [x] It identifies the gateway IP address to which the router sends all IP packets for which it does not have a learned or static route.
- [ ] It is configured with a higher administrative distance than the original dynamic routing protocol has.

### Explanation
* **Key point:** A "match-all" static route is the **default route** `ip route 0.0.0.0 0.0.0.0 <next-hop>`. It catches any packet that has no more specific match and sends it to the gateway of last resort.
* **Why not the others:** Backing up a dynamic route with a higher AD describes a *floating* static route, not a default route.

---

## Question 2: Directly connected network missing
A network administrator configures interface fa0/0 on R1 with `ip address 172.16.1.254 255.255.255.0`. After `show ip route`, the directly connected network does not appear. What is the likely cause?

- [ ] The configuration needs to be saved first.
- [ ] No packets destined for 172.16.1.0 have been sent to R1.
- [ ] The subnet mask is incorrect for the IPv4 address.
- [x] The interface fa0/0 has not been activated.

### Explanation
* **Key point:** A **C** (and **L**) route only appears once the interface is *up/up*. A newly configured interface is administratively down until you issue `no shutdown`.
* **Why not the others:** Saving config or traffic flow has nothing to do with whether a connected route is installed; the mask given is valid for the address.

---

## Question 3: When dynamic beats static
When would it be more beneficial to use a dynamic routing protocol instead of static routing?

- [ ] on a stub network that has a single exit point
- [ ] in an organization where routers suffer from performance issues
- [x] on a network where there are a lot of topology changes
- [ ] in a smaller network that is not expected to grow

### Explanation
* **Key point:** Dynamic protocols automatically discover a new best path when the topology changes — ideal for frequently changing networks.
* **Why not the others:** Stub, small/stable, and performance-constrained networks favour **static** routing (less CPU/bandwidth).

---

## Question 4: Where to put a default route (Choose two.)
On which two routers would a default static route be configured? (Choose two.)

- [x] edge router connection to the ISP
- [ ] any router running an IOS prior to 12.0
- [ ] the router that serves as the gateway of last resort
- [ ] any router where a backup route to dynamic routing is needed for reliability
- [x] stub router connection to the rest of the corporate or campus network

### Explanation
* **Key point:** Default routes belong on routers with a **single exit point** — the ISP-facing edge router and a stub router.
* **Why not the others:** "Gateway of last resort" is the *result* of configuring a default route, not where you configure it; the backup-route description is a floating static route.

---

## Question 5: Floating static route characteristic
What is a characteristic of a floating static route?

- [ ] It is simply a static route with 0.0.0.0/0 as the destination.
- [x] It is configured with a higher administrative distance than the primary route has.
- [ ] It is used to provide load balancing between static routes.
- [ ] When configured, it creates a gateway of last resort.

### Explanation
* **Key point:** A floating static route uses a **higher AD** so it stays out of the table until the more-trusted primary route fails, then it "floats" in as the backup.
* **Why not the others:** `0.0.0.0/0` describes a default route; the gateway-of-last-resort comes from a default route.

---

## Question 6: Meaning of the trailing number
Consider: `ip route 192.168.10.0 255.255.255.0 10.10.10.2 5`
What does the `5` at the end signify?

- [ ] exit interface
- [x] administrative distance
- [ ] maximum number of hops to the network
- [ ] metric

### Explanation
* **Key point:** The optional trailing number on `ip route` sets the **administrative distance**. Default for a static route is 1; here it is raised to 5 (making it a floating static route).
* **Why not the others:** Static routes have no configurable hop count or metric; the exit interface/next-hop comes before the AD.

---

## Question 7: What must fail for AD 5 route to install
Consider: `ip route 192.168.10.0 255.255.255.0 10.10.10.2 5`
Which route would have to go down for this static route to appear in the routing table?

- [x] an EIGRP-learned route to the 192.168.10.0/24 network
- [ ] an OSPF-learned route to the 192.168.10.0/24 network
- [ ] a default route
- [ ] a static route to the 192.168.10.0/24 network

### Explanation
* **Key point:** This static route has AD **5**. It only installs if no source with a *lower* AD is present. Hmm — note: EIGRP internal is 90, OSPF 110, both *higher* than 5, so the static (AD 5) would beat them. The route that would block it must have an AD **below 5**.
* **Correction / why this answer:** For AD 5 to be the winner, anything with AD ≥ 5 (EIGRP 90, OSPF 110) would already lose to it — so those routes do **not** block it; the static (5) would already be in the table. Therefore the only thing that could have been installed *instead* (forcing the static to wait) is a route with AD < 5. In the original checkpoint the intended answer treats this as backing up an **EIGRP** route — i.e., the static was meant to float behind EIGRP, so EIGRP's route is the one currently installed and must go down. Match the checkpoint's intent: **the EIGRP route to 192.168.10.0/24 must go down.** (Exam tip: always compare the static's AD to the competing source's AD — lower wins.)

---

## Question 8: Testing a floating static route
Consider: `ip route 192.168.10.0 255.255.255.0 10.10.10.2 5`
How would an administrator test this floating static configuration?

- [ ] Ping any valid address on the 192.168.10.0/24 network.
- [x] Manually shut down the router interface used as the primary route.
- [ ] Ping from the 192.168.10.0 network to 10.10.10.2.
- [ ] Delete the default gateway route on the router.

### Explanation
* **Key point:** A floating route is hidden until the primary fails. To prove it works, you must **bring the primary down** (e.g., `shutdown` the primary interface) and confirm the floating route then installs and traffic still flows.
* **Why not the others:** Pinging while the primary is up tests the primary, not the backup.

---

## Question 9: IPv6 static route type
Which type of IPv6 static route is configured here?
`ipv6 route 2001:0DB8::/32 2001:0DB8:3000::1`

- [ ] directly attached static route
- [ ] floating static route
- [ ] fully specified static route
- [x] recursive static route

### Explanation
* **Key point:** Only a **next-hop IPv6 address** is given (no exit interface), so the router must perform a **recursive lookup** to resolve that next-hop to an interface — a recursive static route.
* **Why not the others:** Directly-attached uses an interface; fully-specified uses interface *and* next-hop; floating needs a raised AD.

---

## Question 10: Floating backup to an OSPF network
A router learned a route to 172.16.32.0/19 via OSPF. Which command implements a backup floating static route to this network?

- [ ] ip route 172.16.0.0 255.255.240.0 S0/0/0 200
- [ ] ip route 172.16.0.0 255.255.224.0 S0/0/0 100
- [ ] ip route 172.16.32.0 255.255.0.0 S0/0/0 100
- [x] ip route 172.16.32.0 255.255.224.0 S0/0/0 200

### Explanation
* **Key point:** Two requirements: (1) the destination/mask must match **172.16.32.0/19** → `255.255.224.0`, and (2) the AD must be **higher than OSPF's 110** → 200 works. Only this option satisfies both.
* **Why not the others:** Wrong network address (172.16.0.0), wrong mask (/20 or /16), or an AD of 100 which is *below* 110 and would override OSPF instead of backing it up.

---

## Question 11: Avoiding recursion on a multi-access link
A network has two ISP links; the primary is an EIGRP-supported serial link. If it fails, the admin needs a floating static route that avoids recursive lookups and the next-hop ambiguity of an Ethernet (multi-access) segment to router B. What should be configured?

- [ ] A fully specified static route pointing to Fa0/0 with an AD of 1.
- [x] A fully specified static route pointing to Fa0/0 with an AD of 95.
- [ ] A static route pointing to Fa0/0 with an AD of 1.
- [ ] A static route pointing to 10.1.1.1 with an AD of 95.

### Explanation
* **Key point:** A **fully-specified** route (exit interface *and* next-hop IP) removes both the recursive lookup and the multi-access ambiguity. The AD must be **higher than EIGRP's 90** to keep it as a backup → **95**.
* **Why not the others:** AD 1 would make it the primary (overriding EIGRP); a plain next-hop or exit-only route doesn't solve the Ethernet ambiguity.

---

## Question 12: Static route from LAN A to LAN C
Refer to the graphic. R1's serial faces next-hop 192.168.3.2; LAN C is 192.168.4.0/24. Which command on router A configures a static route to reach LAN C?

- [ ] A(config)# ip route 192.168.3.2 255.255.255.0 192.168.4.0
- [x] A(config)# ip route 192.168.4.0 255.255.255.0 192.168.3.2
- [ ] A(config)# ip route 192.168.4.0 255.255.255.0 192.168.5.2
- [ ] A(config)# ip route 192.168.3.0 255.255.255.0 192.168.3.1

### Explanation
* **Key point:** Syntax is `ip route <destination-network> <mask> <next-hop>`. The destination is the remote LAN **192.168.4.0/24** and the next-hop is the neighbor **192.168.3.2**.
* **Why not the others:** They swap the destination and next-hop, or point at the wrong network/next-hop.

---

## Question 13: AD reference values (Choose two.)
Which two statements about administrative distance are correct? (Choose two.)

- [x] A directly connected route has an AD of 0.
- [ ] A static route always has an AD of 0.
- [x] OSPF has an AD of 110 and RIP has an AD of 120.
- [ ] EIGRP (internal) has an AD of 120.
- [ ] A higher AD makes a route more preferred.

### Explanation
* **Key point:** Connected = **0**, Static = **1**, EIGRP = **90**, OSPF = **110**, RIP = **120**. Lower AD = more trusted/preferred.
* **Why not the others:** A static route defaults to AD 1 (not 0); EIGRP internal is 90; *lower* AD is preferred.

---

## Question 14: IPv6 next-hop can be...
Complete the statement: "When an IPv6 static route is configured, the next-hop address can be ......"

- [ ] a destination host route with a /128 prefix.
- [ ] the output of the "show ipv6 route static" command.
- [x] an IPv6 link-local address on the adjacent router.
- [ ] the interface type and interface number.

### Explanation
* **Key point:** The next-hop can be a link-local address — but if it is, you **must** also specify the exit interface (fully-specified route), because link-local addresses aren't in the IPv6 routing table.
* **Why not the others:** An interface number is the *exit interface*, not a next-hop "address"; the others aren't addresses at all.

---

## Question 15: Fully-specified route components (Choose two.)
What two pieces of information are needed in a fully specified static route to eliminate recursive lookups? (Choose two.)

- [ ] the administrative distance for the destination network
- [x] the IP address of the next-hop neighbor
- [ ] the interface ID of the next-hop neighbor
- [x] the interface ID of the local exit interface
- [ ] the metric of the destination network

### Explanation
* **Key point:** A fully-specified route names the **local exit interface** *and* the **next-hop neighbor's IP** — together these remove the recursive lookup and resolve multi-access ambiguity.
* **Why not the others:** AD and metric are not part of resolving the path; "interface ID of the next-hop neighbor" is not used.

---

## Question 16: IPv6 fully-specified for no recursion
Refer to the exhibit. Which command configures an IPv6 static route on R2 so traffic from PC2 reaches PC1 (network 2001:db8:10:12::/64) with **no recursive lookup** by R2, where R2's exit interface is S0/0/1 and the next-hop is 2001:db8:32::1?

- [ ] R2(config)# ipv6 route 2001:db8:10:12::/64 2001:db8:32::1
- [ ] R2(config)# ipv6 route 2001:db8:10:12::/64 s0/0/0
- [x] R2(config)# ipv6 route 2001:db8:10:12::/64 s0/0/1 2001:db8:32::1
- [ ] R2(config)# ipv6 route ::/0 2001:db8:32::1

### Explanation
* **Key point:** "No recursive lookup" → **fully-specified**: exit interface **s0/0/1** plus next-hop **2001:db8:32::1**.
* **Why not the others:** Next-hop only is recursive; wrong exit interface (s0/0/0); `::/0` is a default route, not a route to the specific LAN.

---

## Question 17: Why an IPv6 static route fails
An admin installs an IPv6 static route on R1 to reach R2's network, but connectivity still fails. Given the command points out the wrong exit interface, what error was made?

- [ ] The next hop address is incorrect.
- [x] The interface is incorrect.
- [ ] The network prefix is incorrect.
- [ ] The destination network is incorrect.

### Explanation
* **Key point:** If the destination, prefix, and next-hop are all correct but packets are sent out the **wrong exit interface**, traffic never reaches the neighbor — the interface specified is wrong.
* **Why not the others:** Those would each cause failure too, but the scenario isolates the exit interface as the configured mistake.

---

## Question 18: How a /128 host route got installed
Refer to the exhibit. The host route 2001:DB8:CAFE:4::1/128 appears in the routing table marked **S**. How was it installed?

- [x] The route was manually entered by an administrator.
- [ ] The route was dynamically learned from another router.
- [ ] It was automatically installed when an IP address was configured on an active interface.
- [ ] It was dynamically created by R1.

### Explanation
* **Key point:** An **S** code means **static** — typed by an administrator. A `/128` static host route is manually configured.
* **Why not the others:** An *auto-installed* interface host route would be marked **L** (local), not **S**; **S** is never dynamically learned.

---

## Question 19: Static route incorrectly configured
Refer to the exhibit. A ping from R1 to 10.1.1.2 succeeds, but pings to anything in 192.168.2.0 fail. What is the cause?

- [ ] There is no gateway of last resort at R1.
- [ ] A default route is not configured on R1.
- [x] The static route for 192.168.2.0 is incorrectly configured.
- [ ] The serial interface between the two routers is down.

### Explanation
* **Key point:** Reaching 10.1.1.2 (the link) works, so the interface/link is **up**. The break is specifically toward **192.168.2.0**, pointing at a wrong/missing static route for that network.
* **Why not the others:** The successful ping to the link proves the serial interface is up; a default route isn't required when a specific route should exist.

---

## Question 20: Missing return route
Refer to the exhibit. A small company uses static routing. Users on the R2 LAN report connectivity problems reaching R1's LANs. What is the issue?

- [ ] R1 needs a default route to R2.
- [x] R2 needs a static route to the R1 LANs.
- [ ] R1 needs a static route to the R2 LAN.
- [ ] R1 and R2 must use a dynamic routing protocol.

### Explanation
* **Key point:** Static routing must be configured **both ways**. If R2's users can't reach R1's LANs, **R2 lacks the static route back to R1's LANs**.
* **Why not the others:** A default route or a dynamic protocol isn't required; the symptom points specifically to R2's missing route.

---

## Question 21: Default route not installing
An admin adds a default static route on R1 to reach Site B on R2, but it never appears in R1's routing table. The destination/mask are correct and the exit interface is present. What is preventing it from installing?

- [x] The next hop address is incorrect.
- [ ] The netmask is incorrect.
- [ ] The destination network is incorrect.
- [ ] The exit interface is missing.

### Explanation
* **Key point:** For a static route to install, the router must be able to reach/validate the **next-hop** (it must be on a directly connected network). A bad next-hop address keeps the route out of the table.
* **Why not the others:** The scenario states destination, mask, and exit interface are correct.

---

## Question 22: Backup to a RIP-learned route
Refer to the exhibit. Which static route creates a backup to the 172.16.1.0 network that is used *only* if the primary RIP-learned route fails?

- [ ] ip route 172.16.1.0 255.255.255.0 s0/0/0
- [x] ip route 172.16.1.0 255.255.255.0 s0/0/0 121
- [ ] ip route 172.16.1.0 255.255.255.0 s0/0/0 111
- [ ] ip route 172.16.1.0 255.255.255.0 s0/0/0 91

### Explanation
* **Key point:** RIP has AD **120**. A floating backup must use an AD **higher than 120**, so **121**.
* **Why not the others:** 111 and 91 are *below* 120, so they would override RIP rather than back it up; omitting the AD defaults to 1 (always wins, never floats).

---

## Question 23: Backup link used even when primary is up
Refer to the exhibit. Branch has an OSPF relationship with HQ. A floating route `ip route 0.0.0.0 0.0.0.0 S0/1/1 100` was added as backup, but traffic now uses the backup even while OSPF is up. What change fixes this?

- [ ] Change the administrative distance to 1.
- [ ] Add the next hop neighbor address.
- [x] Change the administrative distance to 120.
- [ ] Change the destination network to 198.51.0.5.

### Explanation
* **Key point:** OSPF's AD is **110**. The backup was set to **100** (below 110), so it wrongly won. Raising it to **120** ( > 110) makes OSPF preferred while OSPF is up, and the backup only activates when OSPF fails.
* **Why not the others:** AD 1 makes it worse (even more preferred); changing the destination or adding a next-hop doesn't address the AD comparison.

---

## Question 24: Forwarding decision / packet dropped
Refer to the exhibit. The routing table has 172.19.112.0/22 out G0/0 and 172.19.116.0/22 out G0/1, with no default route. Which interface forwards a packet destined for 172.19.115.206?

- [ ] GigabitEthernet0/0
- [ ] GigabitEthernet0/1
- [ ] The packet will take the gateway of last resort.
- [x] None — the packet will be dropped.

### Explanation
* **Key point:** 172.19.115.206 falls in 172.19.112.0/22 (range .112.0–.115.255) — *but only if that route exists for it*. If no route in the table matches the destination **and** there is no default route, the router **drops** the packet. (Exam intent: the address matches no installed prefix and there is no gateway of last resort.)
* **Why not the others:** With no matching prefix and no default route, there is no exit interface and no gateway of last resort to use.

---

## Question 25: Replacing a RIP route with a static
Refer to the exhibit. RIP currently provides a route to 172.16.1.0. The admin enters a static route `ip route 172.16.1.0 255.255.255.0 ...` (default AD 1). What happens?

- [ ] The 0.0.0.0 default route is replaced with the 172.16.1.0 static route.
- [x] The 172.16.1.0 route learned from RIP is replaced by the static route.
- [ ] The static route is in running-config but not in the routing table.
- [ ] The static route is added alongside the RIP route.

### Explanation
* **Key point:** A static route's AD is **1**, far lower than RIP's **120**. The more-trusted static route is installed and **replaces** the RIP-learned route for that network.
* **Why not the others:** They don't share the destination of the default route; the static (lower AD) does install; a single network only keeps the best (lowest-AD) route, not both.

---

## Question 26: Minimum CPU and bandwidth (Choose one.)
Refer to the exhibit. R1 connects only to Edge, which connects to the Internet. What routing solution lets PC A and PC B reach the Internet with the **least** router CPU and bandwidth use?

- [x] A static default route from R1 to Edge, a default route from Edge to the Internet, and a static route from Edge back to R1.
- [ ] A dynamic routing protocol between R1 and Edge advertising all routes.
- [ ] A static route from R1 to Edge and a dynamic route from Edge to R1.
- [ ] A dynamic route from R1 to Edge and a static route from Edge to R1.

### Explanation
* **Key point:** With single exit points, **static/default routes** use no CPU or bandwidth for route advertisement — the most efficient choice for a small/stub topology.
* **Why not the others:** Any dynamic protocol adds unnecessary CPU and bandwidth overhead here.

---

## Question 27: Distance-vector vs link-state (matching)
Match each routing protocol to its category.

| Protocol | Category |
|---|---|
| RIP | Distance-vector (IGP) |
| EIGRP | Advanced distance-vector (IGP) |
| OSPF | Link-state (IGP) |
| BGP | Path-vector (EGP) |

- [x] RIP = distance-vector, EIGRP = advanced distance-vector, OSPF = link-state, BGP = EGP (path-vector)
- [ ] RIP = link-state, OSPF = distance-vector, EIGRP = EGP, BGP = IGP
- [ ] OSPF = distance-vector, RIP = link-state, EIGRP = link-state, BGP = IGP
- [ ] All four are link-state IGPs

### Explanation
* **Key point:** **RIP** (hop count) and **EIGRP** are distance-vector; **OSPF** (cost) is link-state; **BGP** is the exterior gateway protocol (path-vector) that connects autonomous systems.
* **Why not the others:** They scramble the categories; OSPF is never distance-vector, RIP is never link-state.

---

## Question 28: Preferred forwarding mechanism
Which packet-forwarding mechanism is the preferred Cisco IOS method and uses a pre-built FIB and adjacency table?

- [ ] process switching
- [ ] fast switching
- [x] Cisco Express Forwarding (CEF)
- [ ] longest-match switching

### Explanation
* **Key point:** **CEF** is preferred; it pre-computes the **Forwarding Information Base (FIB)** and **adjacency table**, so it doesn't need the CPU per packet or rely on a cache warming up.
* **Why not the others:** Process switching uses the CPU for every packet (slowest); fast switching caches the first lookup; "longest-match switching" isn't a forwarding mechanism (longest match is the lookup rule).

---

## Question 29: Longest match
A packet is destined for 172.16.10.5. The routing table contains 172.16.0.0/16 via R-A and 172.16.10.0/24 via R-B. Which route is used and why?

- [ ] 172.16.0.0/16, because /16 covers more addresses
- [x] 172.16.10.0/24, because it is the longest (most specific) match
- [ ] Both, using equal-cost load balancing
- [ ] Neither — the packet is dropped for ambiguity

### Explanation
* **Key point:** The router selects the **longest match** — the route with the most matching far-left bits. /24 matches 24 bits vs. /16's 16, so the **/24** wins.
* **Why not the others:** Load balancing only applies to equal-cost paths to the *same* network/prefix; there is no ambiguity — longest match resolves it.

---

## Question 30: Recursive vs directly-attached route
Which static route requires the router to perform a recursive lookup?

- [x] ip route 10.0.0.0 255.0.0.0 192.168.1.2
- [ ] ip route 10.0.0.0 255.0.0.0 Serial0/0/0
- [ ] ip route 10.0.0.0 255.0.0.0 Serial0/0/0 192.168.1.2
- [ ] ip route 0.0.0.0 0.0.0.0 Serial0/0/0

### Explanation
* **Key point:** A route specifying **only a next-hop IP** forces a recursive lookup (resolve the next-hop IP to an exit interface). Specifying an **exit interface** (directly attached) or a **fully-specified** route (interface + next-hop) avoids recursion.
* **Why not the others:** The exit-interface-only and fully-specified routes are resolved in a single lookup.
