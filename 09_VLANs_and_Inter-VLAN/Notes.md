# Chapter 9 — VLANs and Inter-VLAN Communication

> **Day 5 of 7** · Estimated study time: ~3 hours
> *How to split one physical switched network into many separate logical networks (VLANs), let switches share them over a single link (trunking), and then let those separated networks talk to each other again through a router or Layer 3 switch (inter-VLAN routing).*

## 🎯 What you'll be able to do after this chapter
- Explain what a VLAN is and why we use one (segmentation, security, smaller broadcast domains).
- Identify the standard VLAN types: default, data, native, management, and voice.
- Configure VLANs, assign access ports, and build an 802.1Q trunk between switches.
- Verify your work with `show vlan brief` and `show interfaces trunk`.
- Compare the three ways to do inter-VLAN routing: legacy, router-on-a-stick, and Layer 3 switch SVIs.
- Configure router-on-a-stick using subinterfaces with `encapsulation dot1q`.
- Describe VLAN hopping attacks (switch spoofing and double-tagging) and the commands that stop them.

---

## 1. What is a VLAN, and why bother?

Imagine one office switch with 24 ports. By default, **every** device plugged into that switch is in the same network — one big **broadcast domain**. When any PC sends a broadcast (an ARP request, for example), *every* other device has to receive and process it. Sales, Engineering, and the security cameras are all mixed together. That is messy, slow, and insecure.

A **VLAN (Virtual Local Area Network)** lets you take that one physical switch and carve it into several *logical* networks. Ports 1–8 can be "Sales," ports 9–16 "Engineering," and so on. To the devices, it feels like they are on separate switches — even though they share the same hardware.

The key idea to lock in early:

> **One VLAN = one broadcast domain = (typically) one IP subnet.**

A VLAN is based on **logical connections**, not physical location. A user in Building A and a user in Building B can be on the *same* VLAN, and two users sitting side by side can be on *different* VLANs.

### Benefits (the slides list these — the exam loves them)
- **Security** — Groups with sensitive data are separated from everyone else, reducing the chance of a breach.
- **Cost reduction** — Use existing bandwidth and uplinks more efficiently; fewer hardware upgrades needed.
- **Better performance** — Smaller broadcast domains mean less unnecessary traffic, so the network runs faster.
- **Shrink broadcast domains** — Fewer devices share each broadcast domain.
- **Improved IT staff efficiency** — Users with similar needs share a VLAN, making management easier.
- **Simpler project/application management** — Group users and devices by function or department.

> 💡 **Exam tip:** If a question asks for the *main reason* VLANs improve performance, the answer is **they reduce the size of broadcast domains**. Don't be tempted by "they increase bandwidth" — VLANs don't add bandwidth; they reduce wasted traffic.

---

## 2. Types of VLAN

There are five named VLAN roles you must recognize. Memorize what each one *does*, not just its name.

| VLAN type | What it carries / does |
|---|---|
| **Data VLAN** | Carries user-generated traffic (PCs, laptops). Also called a *user VLAN*. A real network has many of these. |
| **Default VLAN** | What every port belongs to out of the box. On Cisco switches this is **VLAN 1**. It cannot be renamed or deleted. |
| **Native VLAN** | The VLAN that **untagged** traffic on an 802.1Q trunk is placed into. Default is VLAN 1. |
| **Management VLAN** | A VLAN used for managing the switch (SSH, Telnet, HTTPS, SNMP). You give its SVI an IP address. Default is VLAN 1. |
| **Voice VLAN** | A separate VLAN for VoIP traffic, with guaranteed bandwidth and priority so calls stay clear (delay under 150 ms). |

A few critical facts about **VLAN 1**:
- All switch ports start in VLAN 1 after the first boot.
- By default, **all Layer 2 control traffic** (CDP, STP, DTP, VTP) is associated with VLAN 1.
- VLAN 1 is, by default, *all three* of: the default VLAN, the native VLAN, and the management VLAN.
- VLAN 1 **cannot be renamed or deleted**.

> 💡 **Exam tip:** Security best practice is to **move** the native VLAN and the management VLAN *off* VLAN 1 to unused VLANs. The fact that VLAN 1 is the default for everything is exactly why attackers target it.

### VLAN ID ranges
| Range | Name | Notes |
|---|---|---|
| **1–1005** | Normal range | IDs 1002–1005 reserved for Token Ring/FDDI. IDs 1 and 1002–1005 are auto-created and can't be removed. Stored in **vlan.dat** in flash. Learned/stored by VTP. |
| **1006–4094** | Extended range | For service providers. Saved in the **running-config**, not vlan.dat. Requires **VTP transparent mode**. |

> 💡 **Exam tip:** Normal-range VLAN config lives in **`vlan.dat`** in flash — *not* in the running-config. So erasing the startup-config does **not** delete your VLANs; you must delete `vlan.dat` separately.

---

## 3. VLAN Trunking and 802.1Q

Here is the problem trunking solves. Suppose VLAN 10 (Sales) exists on Switch 1 *and* Switch 2. A Sales PC on Switch 1 needs to reach a Sales PC on Switch 2. The link between the two switches must carry VLAN 10 traffic — but maybe also VLAN 20 and VLAN 30. How does the receiving switch know which VLAN a frame belongs to?

The answer is a **trunk**.

> A **trunk** is a point-to-point link between two devices that carries traffic for **more than one VLAN**.

By contrast, an **access port** carries traffic for exactly **one** VLAN and connects to an end device (a PC, printer, or AP).

### How tagging works (802.1Q)
The IEEE **802.1Q** standard solves the "which VLAN?" problem by inserting a small **tag** into the Ethernet frame.

1. A PC on VLAN 10 sends a normal, untagged frame into an access port.
2. The switch sees the port is in VLAN 10, **inserts a 4-byte 802.1Q tag** recording "VLAN 10," **recalculates the FCS**, and sends the tagged frame out the trunk.
3. The far switch reads the tag, knows it's VLAN 10, **removes the tag**, and forwards it out its own VLAN 10 access ports.

The 4-byte tag contains:

| Field | Size | Purpose |
|---|---|---|
| **Type (TPID)** | 2 bytes | Tag Protocol ID, set to **0x8100** for Ethernet — tells the switch "a tag follows." |
| **User priority** | 3 bits | Class of Service (CoS) for QoS. |
| **CFI** | 1 bit | Canonical Format Identifier (legacy Token Ring compatibility). |
| **VLAN ID (VID)** | 12 bits | The actual VLAN number. 12 bits → up to **4096** VLANs. |

> 💡 **Exam tip:** Remember the magic numbers: TPID = **0x8100**, VID = **12 bits** = **4096** possible VLANs. The tag is **4 bytes** inserted *inside* the original frame (between Source MAC and EtherType).

### The native VLAN and untagged traffic
On an 802.1Q trunk, most frames are tagged — but the **native VLAN** is the exception. Traffic for the native VLAN crosses the trunk **untagged**.

- Untagged frames arriving on the trunk are placed into the native VLAN (its **PVID**). Example: if VLAN 99 is the native VLAN, the PVID is 99, and all untagged traffic goes to VLAN 99.
- Control traffic on the native VLAN **should not be tagged**. If an 802.1Q port receives a *tagged* frame whose VLAN ID equals the native VLAN, it **drops the frame**.

> 💡 **Exam tip:** A classic trap — the **native VLAN must match on both ends** of a trunk. A mismatch causes a CDP error and can leak traffic between VLANs. Best practice: set the native VLAN to a dedicated, unused VLAN and don't put any hosts on it.

---

## 4. DTP — Dynamic Trunking Protocol

A switch port doesn't have to be told "you are a trunk" manually — it can *negotiate*. **DTP** is the **Cisco-proprietary** protocol that two Cisco switches use to negotiate whether a link becomes a trunk. It works **point-to-point only**.

| Mode | Behavior |
|---|---|
| `dynamic auto` | **Default** on many switches. Will become a trunk *only if the other side asks*. Auto + Auto = access (stays an access link!). |
| `dynamic desirable` | Actively *tries* to make the link a trunk. |
| `trunk` (`switchport mode trunk`) | Forces the port to be a permanent trunk. |
| `access` (`switchport mode access`) | Forces the port to be a permanent access port (never trunks). |

To force a trunk to a device that **doesn't speak DTP**, turn negotiation off:
```
S1(config-if)# switchport mode trunk
S1(config-if)# switchport nonegotiate
```

> 💡 **Exam tip:** `dynamic auto` on **both** sides means **NO trunk forms** (both wait for the other to ask) — the link stays in access mode. This is a favorite "why isn't my trunk up?" question.

### DTP Negotiation Matrix (memorize the outcomes)
The result of a link depends on the mode at *both* ends. Read row (local) against column (neighbor):

| Local ↓ \ Neighbor → | Dynamic Auto | Dynamic Desirable | Trunk | Access |
|---|---|---|---|---|
| **Dynamic Auto** | Access | Trunk | Trunk | Access |
| **Dynamic Desirable** | Trunk | Trunk | Trunk | Access |
| **Trunk** | Trunk | Trunk | Trunk | ⚠️ Limited connectivity (mismatch) |
| **Access** | Access | Access | ⚠️ Limited connectivity (mismatch) | Access |

> 💡 **Exam tip:** Two patterns to lock in: (1) **Auto + Auto → Access** (no trunk). (2) **Trunk on one side + Access on the other → "limited connectivity"** (a misconfiguration, *not* a clean trunk). Anything involving **Desirable** with Auto/Desirable/Trunk → **Trunk**.

---

## 5. VLAN Configuration

### Create a VLAN and name it
```
S1(config)# vlan 10
S1(config-vlan)# name Sales
S1(config-vlan)# vlan 20
S1(config-vlan)# name Engineering
S1(config-vlan)# exit
```

### Assign an access port to a VLAN
```
S1(config)# interface fastEthernet 0/1
S1(config-if)# switchport mode access
S1(config-if)# switchport access vlan 10
S1(config-if)# exit
```
Assign a whole **range** of ports at once:
```
S1(config)# interface range fastEthernet 0/1 - 8
S1(config-if-range)# switchport mode access
S1(config-if-range)# switchport access vlan 10
```

### Voice + data on one port (IP phone with a PC behind it)
```
S1(config-if)# switchport mode access
S1(config-if)# switchport access vlan 20        ! data VLAN for the PC
S1(config-if)# switchport voice vlan 150         ! voice VLAN for the phone
```

### Configure an 802.1Q trunk port
```
S1(config)# interface gigabitEthernet 0/1
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk native vlan 99        ! move native VLAN off VLAN 1
S1(config-if)# switchport trunk allowed vlan 10,20,99 ! only allow these VLANs
S1(config-if)# end
```
*(On switches that support multiple trunk encapsulations you would first enter `switchport trunk encapsulation dot1q`; on most CCNA-era switches 802.1Q is the only option and the command isn't needed.)*

### Removing things
- Change a port's VLAN membership back to default: `no switchport access vlan`
- Delete a VLAN: `no vlan 10`

### Verify your work
```
S1# show vlan brief          ! VLAN-to-port assignments and membership type
S1# show interfaces trunk    ! which ports are trunking, native VLAN, allowed VLANs
S1# show interfaces fa0/1 switchport   ! mode + access/voice VLAN of one port
```

> 💡 **Exam tip:** `show vlan brief` shows **access** port assignments — trunk ports do **not** appear under a single VLAN there. To inspect trunks, use **`show interfaces trunk`**. Mixing these two up is a common exam trap.

---

## 6. Inter-VLAN Communication

We deliberately separated VLANs into different broadcast domains. But Sales sometimes *does* need to reach a server on another VLAN. Since each VLAN is a separate subnet, moving traffic between them is **routing** — and that needs a Layer 3 device (a router or a multilayer/Layer 3 switch).

> **Inter-VLAN routing** = forwarding traffic from one VLAN to another using a Layer 3 device.

There are three methods. Know all three and *especially* how they compare.

### Method 1 — Legacy inter-VLAN routing
The original approach: a router with **one physical interface per VLAN**. Each switch port connecting to the router is an access port in one VLAN; each router interface sits on a different subnet and acts as that VLAN's default gateway.

- ✅ Simple to understand.
- ❌ **Does not scale** — a router only has so many physical ports. One VLAN = one cable = one router interface. Wasteful.

### Method 2 — Router-on-a-stick (ROAS)
A single physical router interface carries **all** the VLANs over **one trunk link** to the switch. Inside the router, you create **subinterfaces** — software-based virtual interfaces, one per VLAN — each with its own IP address and `encapsulation dot1q` VLAN assignment.

How it works:
1. The router interface operates as an **802.1Q trunk**; the switch port it connects to is in **trunk mode**.
2. Tagged frames arrive; the router reads the tag and routes between subinterfaces.
3. The router sends the routed frame back out the *same* physical interface, re-tagged for the destination VLAN.

- ✅ Only needs one physical interface — far more scalable than legacy.
- ❌ The single link can become a bottleneck under heavy traffic; still less scalable than a Layer 3 switch.

### Method 3 — Layer 3 (multilayer) switch with SVIs
A **multilayer switch** does both Layer 2 switching *and* Layer 3 routing inside one box. You enable `ip routing` and create a **Switch Virtual Interface (SVI)** for each VLAN. The SVI is a virtual interface (no dedicated physical port) that acts as that VLAN's default gateway.

- Routing happens **internally** in hardware — no traffic squeezing down a single trunk for re-tagging.
- Supports both **SVIs** and **routed ports** (a physical port with `no switchport`, acting like a router port — but routed ports do **not** support subinterfaces).
- ✅ **Most scalable** inter-VLAN method; fastest (hardware switching).
- ❌ More expensive hardware; doesn't connect to WAN interface types a router would.

> 💡 **Exam tip:** "Which method is the **most scalable**?" → **Layer 3 switch (SVIs)**. "Which uses **one router interface and subinterfaces**?" → **router-on-a-stick**. "Which uses **one physical interface per VLAN**?" → **legacy**.

### The three methods compared

| Feature | Legacy | Router-on-a-stick | Layer 3 switch (SVI) |
|---|---|---|---|
| Device | Router | Router | Multilayer switch |
| Physical links to switch | One **per VLAN** | **One** (a trunk) | None extra (internal) |
| Gateway per VLAN | Physical router interface | **Subinterface** (`encapsulation dot1q`) | **SVI** (`interface vlan X`) |
| Switch port type | Access (one per VLAN) | **Trunk** | n/a (internal) |
| Scalability | Poor (limited ports) | Moderate | **Best** |
| Speed | Router-speed | Router-speed, shared trunk | **Hardware (fastest)** |
| Typical use | Obsolete / legacy | Small networks | Campus / enterprise |

---

## 7. Inter-VLAN Configuration commands

### Router-on-a-stick (subinterfaces with `encapsulation dot1q`)
On the **router** (single physical interface G0/0/1 carries VLANs 10, 20, 99):
```
R1(config)# interface gigabitEthernet 0/0/1
R1(config-if)# no shutdown
R1(config-if)# exit
!
R1(config)# interface gigabitEthernet 0/0/1.10     ! subinterface number usually = VLAN
R1(config-subif)# encapsulation dot1q 10            ! tag this subif's traffic as VLAN 10
R1(config-subif)# ip address 192.168.10.1 255.255.255.0
R1(config-subif)# exit
!
R1(config)# interface gigabitEthernet 0/0/1.20
R1(config-subif)# encapsulation dot1q 20
R1(config-subif)# ip address 192.168.20.1 255.255.255.0
R1(config-subif)# exit
!
R1(config)# interface gigabitEthernet 0/0/1.99
R1(config-subif)# encapsulation dot1q 99 native     ! 'native' for the native VLAN subif
R1(config-subif)# ip address 192.168.99.1 255.255.255.0
```
On the **switch**, the port facing the router must be a trunk:
```
S1(config)# interface gigabitEthernet 0/1
S1(config-if)# switchport mode trunk
```
Each subinterface IP becomes the **default gateway** for the PCs in that VLAN.

> 💡 **Exam tip:** Three things break ROAS most often: (1) the physical interface is left `shutdown` — you must `no shutdown` it; (2) the `encapsulation dot1q` VLAN number doesn't match the VLAN; (3) the switch port is left in access mode instead of **trunk**.

### Layer 3 switch SVIs
```
S1(config)# ip routing                       ! enable Layer 3 routing (essential!)
S1(config)# vlan 10
S1(config)# vlan 20
S1(config)# interface vlan 10
S1(config-if)# ip address 192.168.10.1 255.255.255.0
S1(config-if)# no shutdown
S1(config)# interface vlan 20
S1(config-if)# ip address 192.168.20.1 255.255.255.0
S1(config-if)# no shutdown
```
A **routed port** (physical port acting like a router interface) instead uses:
```
S1(config)# interface gigabitEthernet 0/1
S1(config-if)# no switchport               ! turn off Layer 2 → make it a routed port
S1(config-if)# ip address 10.0.0.1 255.255.255.252
```

> 💡 **Exam tip:** On a Layer 3 switch, inter-VLAN routing won't work until you run **`ip routing`** in global config. A missing `ip routing` is the #1 "SVIs are up but VLANs can't reach each other" cause. On a 2960, you may also need `sdm prefer lanbase-routing` to enable routing, then reload.

---

## 8. VLAN Attacks (VLAN Hopping)

A **VLAN hopping** attack lets traffic from one VLAN be seen by another VLAN **without a router** — defeating the whole point of VLAN separation. There are two flavors.

### a) Switch spoofing
The attacker's host **pretends to be a switch**. It sends fake 802.1Q and DTP signals to the connected switch port. If that port is in `dynamic auto`/`dynamic desirable`, it happily forms a **trunk** with the attacker. Now the attacker's host is on a trunk and can send/receive traffic on **every VLAN** — effectively hopping between them.

**Why it works:** ports left at the default DTP setting will negotiate a trunk with anything that asks.

### b) Double-tagging
The attacker crafts a frame with **two** 802.1Q tags:
1. The **outer** tag matches the **native VLAN** of the trunk.
2. The **inner** tag is the **target** (victim) VLAN.

What happens:
1. The first switch sees the outer tag = native VLAN, **strips it** (native VLAN is untagged on the trunk), and forwards the frame — still carrying the **inner** tag — across the trunk.
2. The next switch reads the **inner** tag and delivers the frame into the target VLAN.

Key properties: the attack is **unidirectional** (no reply path), and it only works when the **attacker is in the same VLAN as the native VLAN of the trunk**.

### Mitigations (memorize this list)
- **Disable trunking on all access ports** — force them to access mode: `switchport mode access`.
- **Disable auto trunking** so trunks must be enabled manually: `switchport nonegotiate` on real trunks.
- **Make sure the native VLAN is used only for trunk links** — and set it to an **unused** VLAN that has no hosts.

> 💡 **Exam tip:** Double-tagging is defeated specifically by **changing the native VLAN to an unused VLAN** that no access ports use, so an attacker can never sit in the native VLAN. Switch spoofing is defeated by **turning off DTP/auto-trunking** (`switchport mode access` + `switchport nonegotiate`).

---

## 🧪 Hands-on (Packet Tracer)

| Lab | What it teaches |
|---|---|
| **SRWE 3.2.8 Investigate a VLAN Implementation** | See how VLANs separate traffic; observe broadcast domains. |
| **3.3.12 VLAN Configuration** | Create VLANs, name them, assign access ports, verify with `show vlan brief`. |
| **3.4.5 Configure Trunks** | Build 802.1Q trunks, set native and allowed VLANs, verify with `show interfaces trunk`. |
| **3.5.5 Configure DTP** | Practice trunk negotiation modes (auto/desirable/trunk/nonegotiate). |
| **3.6.1 Implement VLANs and Trunking** | Put VLANs + trunking together end-to-end. |
| **4.2.7 Configure Router-on-a-Stick Inter-VLAN Routing** | Subinterfaces with `encapsulation dot1q`; router as gateway for multiple VLANs. |
| **4.3.8 Configure Layer 3 Switching and Inter-VLAN Routing** | Enable `ip routing`, create SVIs, use routed ports. |
| **4.4.8 Troubleshoot Inter-VLAN Routing** | Find and fix common faults (missing `ip routing`, wrong encapsulation, access vs trunk). |

---

## 🧠 Key terms to memorize

| Term | Meaning in one line |
|---|---|
| **VLAN** | A logical broadcast domain that segments a switched network. |
| **Broadcast domain** | The set of devices that receive each other's broadcasts; one per VLAN. |
| **Access port** | Carries traffic for exactly one VLAN; connects an end device. |
| **Trunk** | Point-to-point link carrying multiple VLANs between devices. |
| **802.1Q** | IEEE standard that inserts a 4-byte VLAN tag into a frame. |
| **Native VLAN** | VLAN whose traffic crosses a trunk untagged (default VLAN 1). |
| **PVID** | Port VLAN ID — the VLAN assigned to untagged frames on a port. |
| **DTP** | Cisco-proprietary protocol that negotiates trunking; default `dynamic auto`. |
| **Default VLAN** | VLAN 1 — all ports start here; can't be renamed/deleted. |
| **Management VLAN** | VLAN (with an SVI + IP) used to manage the switch remotely. |
| **Voice VLAN** | Dedicated VLAN giving VoIP priority and assured bandwidth. |
| **vlan.dat** | File in flash storing normal-range VLAN config. |
| **Inter-VLAN routing** | Routing traffic between VLANs via a Layer 3 device. |
| **Router-on-a-stick** | One router interface + subinterfaces routing many VLANs over a trunk. |
| **Subinterface** | Virtual interface on a physical port; gets its own IP + `encapsulation dot1q`. |
| **SVI** | Switch Virtual Interface — virtual Layer 3 interface for a VLAN on a multilayer switch. |
| **Routed port** | Physical switch port with `no switchport`, acting like a router interface. |
| **VLAN hopping** | Attack that reaches another VLAN without a router (switch spoofing / double-tagging). |

---

## ⚡ Exam tips & traps (recap)
- **One VLAN = one broadcast domain = one subnet.** VLANs improve performance by **shrinking broadcast domains**, not by adding bandwidth.
- **VLAN 1** is the default for default/native/management VLANs and carries Layer 2 control traffic. Move native and management off VLAN 1 for security; VLAN 1 can't be deleted.
- 802.1Q tag = **4 bytes**, TPID **0x8100**, VID **12 bits → 4096 VLANs**. Native VLAN traffic is **untagged**.
- **`dynamic auto` on both ends = no trunk.** Use `switchport mode trunk` + `switchport nonegotiate` to force one.
- **Native VLAN must match on both trunk ends.** A tagged frame matching the native VLAN ID is **dropped**.
- Normal-range VLANs live in **`vlan.dat`** (flash), not the running/startup config.
- `show vlan brief` = access-port assignments; **`show interfaces trunk`** = trunk details. Don't swap them.
- Inter-VLAN scalability ranking: **legacy (worst) < router-on-a-stick < Layer 3 switch (best)**.
- ROAS needs: physical interface **`no shutdown`**, matching **`encapsulation dot1q <vlan>`**, and a **trunk** switch port.
- Layer 3 switch routing requires **`ip routing`**; a routed port needs **`no switchport`** (and does **not** support subinterfaces).
- **Switch spoofing** → stop with `switchport mode access` + `switchport nonegotiate`. **Double-tagging** → stop by setting the native VLAN to an **unused** VLAN with no hosts.

---

## ✅ Quick self-check
1. Two switches connect with both ports in `dynamic auto`. Is the link a trunk or an access link, and why?
2. What three things must be true for router-on-a-stick to route between VLAN 10 and VLAN 20?
3. You configured SVIs for VLANs 10 and 20 on a multilayer switch and they're "up," but the VLANs still can't reach each other. What command did you most likely forget?
4. In a double-tagging attack, which VLAN must the attacker be in, and what single configuration change defeats it?
5. Where is normal-range VLAN configuration stored, and why does erasing the startup-config not remove your VLANs?

*(Answers and full reasoning are in `Quiz.md`.)*
