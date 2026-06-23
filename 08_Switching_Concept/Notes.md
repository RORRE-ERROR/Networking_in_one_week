# Chapter 8 — Switching Concept

> **Day 5 of 7** · Estimated study time: ~3 hours
> *How a switch actually moves Ethernet frames around a LAN — and how you configure and secure one from the CLI.*

## 🎯 What you'll be able to do after this chapter
- Explain the three campus layers (access, distribution, core) and what an **access switch** does.
- Describe how switches break up **collision domains** and **broadcast domains** (and why microsegmentation matters).
- Walk through how a switch **learns** MAC addresses and **forwards / floods** frames.
- Compare the frame-forwarding methods: **store-and-forward** vs **cut-through** (fast-forward & fragment-free).
- Perform basic switch configuration: boot process, an **SVI management IP**, duplex/speed, auto-MDIX, and the `show` commands to verify it.
- Lock a switch down with **port security** (static / dynamic / sticky MACs, the three violation modes) and **SSH** for secure remote access.

---

## 1. Introduction — the campus hierarchy and the switch's job

A big network isn't one giant flat switch — it's built in **three layers**, like a pyramid. Think of it as a road network: small streets feed into main roads, which feed into the motorway.

| Layer | Plain meaning | Role |
| --- | --- | --- |
| **Access layer** | The "front door" — where users plug in | Provides **network access to end users**. This is the network edge. |
| **Distribution layer** | The middle — connects access to core | Sits **between access and core**; gives **high availability** through redundant switches. |
| **Core layer** | The motorway / **backbone** | **Aggregates** all the campus blocks and ties the campus to the rest of the network. |

> 💡 **Exam tip:** The single most-tested fact here: the **access layer** is where the **user gets network access** (the network edge). The **core** is the high-speed **backbone**. Don't mix them up.

### Ingress vs egress, and why it matters
A switch decides how to forward a frame based on **where it came in** and **where it's going**.

- **Ingress** = the port a frame **enters** on.
- **Egress** = the port a frame **leaves** on.

A LAN switch forwards traffic based on the **ingress port** and the **destination MAC address** of the Ethernet frame.

### The MAC address table (CAM)
A switch keeps a **MAC address table** — a list of *which MAC address lives on which port*. It is stored in **Content Addressable Memory (CAM)**, a special memory built for very fast lookups.

> Analogy: the MAC table is the switch's address book. "Whose MAC is `aabb.cc00.0001`? → that one's on port Fa0/1." CAM lets it look that up almost instantly.

### How a switch learns and forwards — the two-step process
This is one of the most important processes in the whole CCNA. Learn it as **two steps**:

**Step 1 — LEARN (look at the SOURCE MAC):**
1. Every frame entering the switch is inspected for its **source MAC address** and the **ingress port**.
2. If that source MAC is **not** in the table → add it (MAC + port) to the table.
3. If it **is** already there → refresh its timer. By default, an entry stays for **5 minutes** (300 s) of inactivity.

**Step 2 — FORWARD (look at the DESTINATION MAC):**
1. If the destination is a **unicast** MAC **found** in the table → forward out **only that one port**.
2. If the destination is a **unicast not in the table** → flood out **all ports except the ingress port**. This is an **unknown unicast**.
3. If the destination is a **broadcast** (e.g. `ffff.ffff.ffff`) or **multicast** → also **flood** out all ports except the ingress port.

> 💡 **Exam tip:** A switch learns from the **SOURCE** MAC and forwards based on the **DESTINATION** MAC. If asked "what does a switch do with an unknown unicast or a broadcast?" → **flood out all ports except the one it arrived on.**

### Switch hardware form factors
- **Fixed configuration** — ports/features are fixed; you can't expand beyond what it ships with.
- **Modular configuration** — a chassis that accepts swappable **line cards** (choose your port count).
- **Stackable configuration** — switches joined by a special **StackWise** cable, acting as **one larger switch**. A stack can hold **up to 9 switches**, managed by a **single IP address**. One switch is the **stack master**; if it fails, a new master is elected (highest **stack-member priority** wins).

---

## 2. Switching Domain — collisions, broadcasts, and microsegmentation

A "domain" here just means **the area a certain kind of traffic reaches.** Two kinds matter:

- **Collision domain** — the area where two devices' signals can *collide* (only relevant on **half-duplex**/shared media).
- **Broadcast domain** — the area a **broadcast** frame reaches.

### Duplex and collision domains
- Switch ports **auto-negotiate full-duplex** when the device on the other end also supports full-duplex.
- If the other end is **half-duplex** (e.g. an old hub), the port runs **half-duplex** too.
- **Half-duplex port → that segment is its own collision domain.**
- **Full-duplex → there are NO collision domains.** (Send and receive happen on separate wires, so nothing can collide.)

> Intuition: a hub is one big shared road where everyone can crash into each other. A switch gives every port its **own private lane** — this is **microsegmentation**: *each switch port is its own collision domain.*

### Broadcast domains
- A switch **breaks up collision domains** (one per port) but does **NOT** break up broadcast domains by default.
- **A collection of interconnected switches = one single broadcast domain** (the *MAC broadcast domain*) — every device that receives a host's broadcast frames.
- To split broadcast domains you need **VLANs** or a **router** (a Layer 3 boundary).

> 💡 **Exam tip:** Memorize this pair:
> - **Switch** = many collision domains (one per port), but **one** broadcast domain.
> - **Router** = separates **broadcast** domains. **VLANs** also separate broadcast domains.

### Characteristics that make switches fast
- **Fast port speeds** — access: 100 Mbps / 1 Gbps · distribution: up to 10 Gbps · core/data center: 10/40/100 Gbps.
- **Fast internal switching** — a fast internal bus or shared memory for high performance.
- **Large frame buffers** — temporarily hold received frames so a fast ingress port (1 Gbps) can feed a slow egress port (100 Mbps) **without dropping** frames.
- **High port density** — more ports per switch lowers cost (two 48-port switches beat four 24-port switches for 96 ports).

---

## 3. Frame forwarding methods — how the switch decides *when* to forward

When a frame arrives, the switch can either **wait for the whole frame** before forwarding, or **start forwarding early**. That's the store-and-forward vs cut-through trade-off.

### Store-and-forward vs cut-through

| | **Store-and-Forward** | **Cut-Through** |
| --- | --- | --- |
| When it forwards | After the **entire frame** is received | As soon as it reads the **destination MAC** (first 6 bytes after preamble) |
| Error checking | **Yes** — runs the **FCS/CRC** check; drops bad frames | **No** — may forward corrupt/runt frames |
| Latency | Higher (must buffer whole frame) | **Lower** (forwards almost immediately) |
| Required for | **QoS** and speed-mismatch buffering | Lowest-latency environments |
| Cisco default | **Yes (preferred method)** | — |

**Two flavours of cut-through:**
- **Fast-forward** — forwards immediately after reading the **destination MAC**. Lowest latency, highest chance of relaying an errored frame.
- **Fragment-free** — waits to read the **first 64 bytes** before forwarding. Because most collisions/errors happen in the first 64 bytes, this catches the worst frames — a *compromise* between fast-forward and store-and-forward.

> 💡 **Exam tip:** Only **store-and-forward** checks the **FCS** and discards errored frames — and it's **Cisco's default**. **Fragment-free** = the "first **64 bytes**" cut-through variant.

### Memory buffering (how frames wait their turn)
While a frame waits to be forwarded, the switch parks it in a buffer. Two designs:
- **Port-based memory** — frames sit in queues **tied to specific ingress/egress ports**. A frame can be delayed because *its* port's queue is busy, even if other ports are free.
- **Shared memory** — all ports draw from a **common pool**. More flexible; lets large frames go out a port without being limited by one port's queue. Essential when bridging different speeds (1 Gbps → 100 Mbps).

---

## 4. Switch Configuration

### The boot process (in order)
1. **POST** (Power-On Self-Test) runs from **ROM** — tests the **CPU, DRAM**, and the flash file system.
2. **Boot loader** (small program in ROM) runs — does low-level CPU init (registers, memory mapping/size/speed).
3. Boot loader **initializes the flash file system**.
4. Boot loader **locates and loads the IOS** image into memory and hands over control.
5. **IOS** initializes the interfaces using the **startup-config** stored in **NVRAM**.

> 💡 **Exam tip:** Order matters: **POST → boot loader → flash file system → load IOS → IOS reads startup-config.** Startup-config lives in **NVRAM**; the IOS image lives in **flash**; POST and boot loader live in **ROM**.

**Boot loader access (recovery):** console in → unplug power → reconnect and hold the **Mode** button within 15 s while the System LED flashes green → keep holding until System LED goes briefly amber then solid green → release → you get the `switch:` prompt.

**Status LEDs:** System (off = off, **green = normal**, **amber = fault**), RPS, Port Status, Port Duplex, Port Speed, PoE Mode.

### Management IP via the SVI
A switch is a Layer 2 device, so its physical ports don't get IPs. To manage it remotely you give an IP to a **Switch Virtual Interface (SVI)** — a virtual interface tied to a VLAN. You also need a **subnet mask** and a **default gateway**.

By default management is on **VLAN 1**. **Best practice: use a VLAN *other than* VLAN 1** for management.

```cisco
Switch> enable
Switch# configure terminal
Switch(config)# hostname S1
S1(config)# interface vlan 99               ! management SVI
S1(config-if)# ip address 192.168.1.2 255.255.255.0
S1(config-if)# no shutdown
S1(config-if)# exit
S1(config)# ip default-gateway 192.168.1.1  ! needed to reach other subnets
S1(config)# end
S1# copy running-config startup-config       ! save!
```

> To enable IPv6 on a 2960 you must first run `sdm prefer dual-ipv4-and-ipv6 default` and then **reload** the switch.

### Duplex, speed, and auto-MDIX
```cisco
S1(config)# interface FastEthernet0/1
S1(config-if)# duplex full        ! options: auto | full | half
S1(config-if)# speed 100          ! speed in Mbps
S1(config-if)# mdix auto          ! auto-detect straight-through vs crossover
```
- **Auto-MDIX** makes the port auto-detect the cable type, so a crossover-vs-straight-through mismatch no longer matters.
- Mismatched duplex (one side full, one side half) is a classic cause of slow links — keep both ends consistent (or both `auto`).

### Verify with `show` commands
| Command | What it shows |
| --- | --- |
| `show running-config` | Current live configuration |
| `show interfaces` | Status, **duplex & speed**, errors, counters |
| `show ip interface brief` | Quick up/down status + IPs (check the SVI here) |
| `show mac address-table` | Learned MAC-to-port mappings |
| `show vlan` | VLAN membership |
| `show version` | IOS version, uptime |

> 💡 **Exam tip:** To verify a switch's **management IP/SVI**, use `show ip interface brief` (or `show interfaces vlan 99`). To verify **auto-MDIX / duplex / speed** on a port, use `show interfaces`.

---

## 5. Switch Security Management

### Secure remote access — SSH, not Telnet
- **Telnet** (TCP **23**) sends everything — usernames, passwords, data — in **plaintext**. Anyone sniffing the wire can read it.
- **SSH** (TCP **22**) **encrypts** the whole session. Always prefer SSH.

> 💡 **Exam tip:** **SSH = port 22 (encrypted)**, **Telnet = port 23 (plaintext)**. SSH version **2** is preferred.

**Configuring SSH (the steps from the slides):**
```cisco
S1(config)# hostname S1
S1(config)# ip domain-name example.com          ! 1. hostname + 2. domain name
S1(config)# crypto key generate rsa             ! 3. generate RSA keys
  How many bits in the modulus [512]: 1024      !    minimum modulus = 1024 bits
S1(config)# username admin secret Cisco123       ! 4. local user for authentication
S1(config)# ip ssh version 2                     ! force SSH v2
S1(config)# line vty 0 15                         ! 5. configure the vty lines
S1(config-line)# transport input ssh             !    SSH only — block Telnet
S1(config-line)# login local                     !    authenticate against local users
S1(config-line)# end
S1# show ip ssh                                   ! verify SSH settings
```

> 💡 **Exam tip:** SSH needs a **hostname AND a domain name** before `crypto key generate rsa` will work — those two feed the key name. Minimum modulus is **1024 bits**.

### Why we lock down ports — MAC flooding & port security
**The attack:** the MAC table is **finite**. A tool like **macof** floods the switch with thousands of fake source MACs until the table is **full**. Once full, the switch can't learn legitimate entries, so it treats traffic as **unknown unicast and floods everything out all ports** in that VLAN — letting an attacker **sniff traffic** meant for other hosts.

**The defense — port security:** limits **how many MAC addresses** a port will allow. Limit a port to **one** MAC and you control who can connect.

A violation occurs when:
- The max number of MACs is reached and a **new/unknown MAC** appears, **or**
- A MAC learned on **one** secure port shows up on **another** secure port in the **same VLAN**.

### Three ways to learn the secure MAC
| Type | How the MAC is set | Survives reboot? |
| --- | --- | --- |
| **Static** | **Manually** typed in (`switchport port-security mac-address H.H.H`) | Yes (in config) |
| **Dynamic** | **Learned automatically**, stored only in the MAC table | **No** — lost on reboot |
| **Sticky** | Learned dynamically **OR** typed manually, then **written into the running-config** | Yes (if you save) |

> 💡 **Exam tip:** **Sticky** = dynamically learned **but** added to the running-config, so it behaves like a static entry after you `copy run start`. **Dynamic** entries vanish on reboot.

### The three violation modes (memorize this table!)

| Mode | Drops offending traffic? | Sends notification (syslog/SNMP)? | Increments violation counter? | Port goes err-disabled? |
| --- | --- | --- | --- | --- |
| **Protect** | ✅ Yes | ❌ No | ❌ No | ❌ No |
| **Restrict** | ✅ Yes | ✅ Yes | ✅ Yes | ❌ No |
| **Shutdown** *(default)* | ✅ Yes (port off) | ✅ Yes | ✅ Yes | ✅ **Yes** |

> 💡 **Exam tip:** **Shutdown is the DEFAULT.** It puts the port in **err-disabled** state (you must `shutdown` then `no shutdown` to recover). **Protect** is the *quiet* one — drops traffic but gives **no notification** and **no counter**. **Restrict** = drop **+** notify **+** count, but the port **stays up**.

**Configuring port security:**
```cisco
S1(config)# interface FastEthernet0/1
S1(config-if)# switchport mode access                       ! must be access, not dynamic
S1(config-if)# switchport port-security                     ! turn on port security
S1(config-if)# switchport port-security maximum 2           ! allow up to 2 MACs
S1(config-if)# switchport port-security mac-address sticky  ! learn + save MACs
S1(config-if)# switchport port-security violation restrict  ! protect | restrict | shutdown
S1(config-if)# switchport port-security mac-address aaaa.bbbb.cccc   ! a STATIC secure MAC
S1(config-if)# end
S1# show port-security interface FastEthernet0/1            ! verify mode, max, violations
S1# show port-security address                              ! see secure MACs
```

### Port security aging
Removes secure MACs automatically without you deleting them by hand.
- **Absolute** — secure addresses are removed after the set time, no matter what.
- **Inactivity** — removed only if **idle** for the set time.

### Disable unused ports
Any unused port is a back door. Best practice: shut every unused port.
```cisco
S1(config)# interface range FastEthernet0/5 - 24
S1(config-if-range)# shutdown
```

> 💡 **Exam tip:** Port security only works on an **access** port (`switchport mode access`). And a hardening best practice: **administratively shut down all unused ports.**

---

## 🧪 Hands-on (Packet Tracer)
- **SRWE 1.1.7 / 2.5.5 — Basic / Initial Switch Configuration:** hostname, banner, passwords, SVI management IP, save config — the core of Section 4.
- **SRWE 1.3.6 — Configure SSH:** hostname + domain name → RSA keys → user → `transport input ssh` on the vty lines. Practices Section 5's SSH steps.
- **ITN 16.4.6 — Configure Secure Passwords and SSH:** strong password practices plus the full SSH build.
- **ITN 16.5.1 — Secure Network Devices:** ties it together — disabling unused ports, securing access, and verifying with `show` commands.

## 🧠 Key terms to memorize
| Term | Meaning in one line |
| --- | --- |
| Access / Distribution / Core | Network edge (user access) / aggregation + redundancy / high-speed backbone |
| Ingress / Egress | Port a frame **enters** / port it **leaves** |
| MAC address table (CAM) | Fast-lookup memory mapping MAC → port |
| Unknown unicast | Unicast whose dest MAC isn't in the table → flooded out all other ports |
| Collision domain | Area where signals can collide; one per switch port (half-duplex only) |
| Broadcast domain | Area a broadcast reaches; one per switch (split by VLANs/routers) |
| Microsegmentation | Giving each switch port its own collision domain |
| Store-and-forward | Receive whole frame, run FCS check, then forward (Cisco default) |
| Cut-through (fast-forward / fragment-free) | Forward after dest MAC / after first 64 bytes |
| SVI | Virtual VLAN interface that holds the switch's management IP |
| Auto-MDIX | Port auto-detects straight-through vs crossover cabling |
| Port security | Limits valid MACs per port (static/dynamic/sticky) |
| Violation modes | protect (silent) / restrict (notify) / shutdown (err-disable, default) |
| MAC flooding | Filling the CAM table to force the switch to flood and enable sniffing |
| SSH / Telnet | Encrypted remote mgmt TCP 22 / insecure plaintext TCP 23 |

## ⚡ Exam tips & traps (recap)
- Switch **learns from SOURCE MAC**, **forwards by DESTINATION MAC**. Default MAC entry age = **5 minutes**.
- **Unknown unicast, broadcast, multicast → flood** out every port except the ingress port.
- Switch = **many collision domains, ONE broadcast domain**. Routers/VLANs split broadcast domains. **Full-duplex = no collisions.**
- **Store-and-forward** is the only method that checks the **FCS** and it's **Cisco's default**. **Fragment-free** = first **64 bytes**.
- Boot order: **POST → boot loader → flash FS → load IOS → IOS reads startup-config (NVRAM)**.
- Management IP goes on an **SVI**; don't forget **`ip default-gateway`** to reach other subnets. Best practice: management VLAN ≠ VLAN 1.
- **SSH = 22 (encrypted), Telnet = 23 (plaintext).** SSH needs **hostname + domain name** before generating RSA keys (min **1024-bit** modulus). Use **`transport input ssh`** + **`login local`** on the vty lines.
- Port security needs **`switchport mode access`** first. **Default violation mode = shutdown** (port → **err-disabled**). **Protect = no notification.**
- **Sticky** MACs are learned then saved to running-config; **dynamic** MACs are lost on reboot.
- Harden: **disable all unused ports** and prefer a non-VLAN-1 management VLAN.

## ✅ Quick self-check
1. A frame arrives whose destination MAC is **not** in the table. What does the switch do — and what is this called?
2. Your friend says "a switch separates broadcast domains." Correct them in one sentence.
3. Which forwarding method runs the FCS check and discards bad frames, and is it the Cisco default?
4. Put these in boot order: load IOS, POST, boot loader, IOS reads startup-config.
5. You set a port to `violation restrict`. Compared to the default, what behaves differently?
6. Why must you configure a **domain name** before SSH will work?
