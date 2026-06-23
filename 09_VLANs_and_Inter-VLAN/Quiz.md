# Chapter 9 Quiz — VLANs and Inter-VLAN Communication

> Exam-style questions. Answers are marked and explained. Cover the explanations and test yourself first.

## Question 1: Why use VLANs
Which statement best describes how VLANs improve network performance?

- [ ] They increase the total bandwidth available on each switch port.
- [x] They divide one large broadcast domain into several smaller ones, reducing unnecessary traffic.
- [ ] They eliminate the need for any routing in the network.
- [ ] They convert Layer 2 switches into Layer 3 routers.

### Explanation
* **Key point:** A VLAN is a broadcast domain. Splitting a flat Layer 2 network into multiple smaller broadcast domains means fewer devices receive each broadcast, cutting wasted traffic and boosting performance.
* **Why not the others:** VLANs do not add bandwidth, and devices on different VLANs still need a router (or Layer 3 switch) to communicate.

---

## Question 2: VLAN benefits (Choose three.)
Which three are documented benefits of implementing VLANs? (Choose three.)

- [x] Improved security through separation of sensitive groups
- [x] Cost reduction by using existing bandwidth more efficiently
- [x] Shrinking the size of broadcast domains
- [ ] Automatic routing between all VLANs without configuration
- [ ] Elimination of the need for IP subnets

### Explanation
* **Key point:** Security, cost reduction, better performance, smaller broadcast domains, IT staff efficiency, and simpler project management are the listed benefits.
* **Why not the others:** Inter-VLAN traffic still requires a routing device, and each VLAN typically maps to its own IP subnet.

---

## Question 3: VLAN 1 facts
Which statement about VLAN 1 on a Cisco switch is true?

- [ ] VLAN 1 must be manually created before ports can be assigned.
- [ ] VLAN 1 can be renamed but not deleted.
- [x] By default VLAN 1 is the default, native, and management VLAN, and carries Layer 2 control traffic.
- [ ] VLAN 1 is reserved for voice traffic only.

### Explanation
* **Key point:** Out of the box, all ports are in VLAN 1, and VLAN 1 simultaneously serves as the default, native, and management VLAN. Layer 2 control traffic (CDP, STP, DTP, VTP) rides VLAN 1 by default.
* **Why not the others:** VLAN 1 exists automatically and can be **neither renamed nor deleted**.

---

## Question 4: Match VLAN type to purpose
Match each VLAN type to its purpose.

| VLAN type | Purpose |
| :--- | :--- |
| **Data VLAN** | Carries user-generated traffic from PCs and laptops |
| **Native VLAN** | Carries untagged traffic across an 802.1Q trunk |
| **Management VLAN** | Used for SSH/Telnet/SNMP management of the switch via an SVI |
| **Voice VLAN** | Provides assured bandwidth and priority for VoIP |
| **Default VLAN** | VLAN 1, where all ports reside after the initial boot |

### Explanation
* **Key point:** Each VLAN role is defined by the *kind of traffic* it carries. The voice VLAN guarantees low delay (under 150 ms) for calls; the management VLAN needs an SVI with an IP address.

---

## Question 5: Native VLAN behavior
On an 802.1Q trunk, what happens to traffic belonging to the native VLAN?

- [ ] It is tagged with a special priority value.
- [x] It is sent across the trunk untagged.
- [ ] It is dropped by the receiving switch.
- [ ] It is encrypted before transmission.

### Explanation
* **Key point:** The 802.1Q native VLAN carries traffic **untagged**. Untagged frames received on the trunk are placed into the native VLAN (its PVID).
* **Why not the others:** If a *tagged* frame arrives whose VLAN ID equals the native VLAN, the port drops it — but normal native-VLAN traffic is simply untagged, not dropped.

---

## Question 6: 802.1Q tag size and TPID
Which two statements about the 802.1Q VLAN tag are correct? (Choose two.)

- [x] The tag is 4 bytes inserted within the original Ethernet frame header.
- [ ] The tag is appended after the FCS at the end of the frame.
- [x] The TPID (Type) field for Ethernet is set to 0x8100.
- [ ] The VLAN ID field is 8 bits, allowing 256 VLANs.
- [ ] The tag replaces the source MAC address.

### Explanation
* **Key point:** 802.1Q inserts a **4-byte** tag inside the frame (after the source MAC). The TPID is **0x8100**. The VLAN ID is **12 bits**, supporting up to **4096** VLANs.
* **Why not the others:** The switch recalculates the FCS *after* inserting the tag — the tag is not after the FCS, and it does not overwrite the source MAC.

---

## Question 7: VLAN ID ranges
Which statement correctly describes normal-range VLANs?

- [ ] They use IDs 1006–4094 and are stored in the running-config.
- [x] They use IDs 1–1005, are stored in vlan.dat in flash, and can be managed by VTP.
- [ ] They require VTP transparent mode to be configured.
- [ ] They cannot be used in enterprise networks.

### Explanation
* **Key point:** Normal-range VLANs are IDs **1–1005**, stored in **`vlan.dat`** in flash, and VTP can learn/store them.
* **Why not the others:** IDs 1006–4094 are **extended-range** VLANs, stored in the running-config, and they require **VTP transparent mode**.

---

## Question 8: DTP default behavior
Two Cisco switches are connected, and both ports are left at the default DTP setting. What is the result?

- [ ] A trunk forms automatically.
- [x] No trunk forms; the link stays in access mode because both sides are dynamic auto.
- [ ] The link is disabled due to a DTP mismatch.
- [ ] The link becomes a routed port.

### Explanation
* **Key point:** The default is **`dynamic auto`**, which only trunks if the *other* side asks. Auto + auto means neither side initiates, so the link remains an access link.
* **Why not the others:** To guarantee a trunk, set one side to `dynamic desirable` or `trunk`. To force a trunk to a non-DTP device, use `switchport mode trunk` plus `switchport nonegotiate`.

---

## Question 9: Configure an access port
Which command sequence assigns interface Fa0/5 to VLAN 30 as an access port?

- [ ] `switchport trunk vlan 30`
- [x] `switchport mode access` then `switchport access vlan 30`
- [ ] `switchport access vlan 30` then `switchport mode trunk`
- [ ] `vlan 30 access fastethernet 0/5`

### Explanation
* **Key point:** Set the mode to access, then assign the VLAN. If VLAN 30 doesn't yet exist, IOS creates it automatically when you assign it.
* **Why not the others:** `switchport trunk vlan` is not valid for access assignment, and putting the port in trunk mode contradicts an access assignment.

---

## Question 10: Verify VLAN assignments
A technician wants to see which access ports belong to which VLANs. Which command should be used?

- [ ] `show interfaces trunk`
- [x] `show vlan brief`
- [ ] `show ip route`
- [ ] `show running-config interface`

### Explanation
* **Key point:** **`show vlan brief`** lists each VLAN and the access ports assigned to it.
* **Why not the others:** `show interfaces trunk` shows trunk ports, native VLAN, and allowed VLANs — trunk ports do **not** appear under a single VLAN in `show vlan brief`.

---

## Question 11: Force a trunk to a non-DTP device
A Cisco switch connects to a server NIC that does not understand DTP. Which two commands ensure the link becomes and stays a trunk? (Choose two.)

- [x] `switchport mode trunk`
- [ ] `switchport mode dynamic auto`
- [x] `switchport nonegotiate`
- [ ] `switchport mode access`
- [ ] `no switchport`

### Explanation
* **Key point:** `switchport mode trunk` forces trunking; `switchport nonegotiate` disables DTP frames so the switch won't wait to negotiate with a device that can't respond.
* **Why not the others:** `dynamic auto` would never trunk without negotiation; `access` is the opposite; `no switchport` makes a routed port.

---

## Question 12: Inter-VLAN routing requirement
Why can two hosts on different VLANs not communicate by default?

- [ ] Switches block all unicast traffic between ports.
- [x] Each VLAN is a separate broadcast domain/subnet, so a Layer 3 device is needed to route between them.
- [ ] VLANs use incompatible framing formats.
- [ ] The native VLAN drops all inter-VLAN frames.

### Explanation
* **Key point:** A VLAN is a separate broadcast domain and subnet. Moving traffic between subnets is routing, which requires a router or multilayer switch.

---

## Question 13: Identify the inter-VLAN method
A network uses a single physical router interface connected to a switch trunk, with one subinterface per VLAN. Which inter-VLAN routing method is this?

- [ ] Legacy inter-VLAN routing
- [x] Router-on-a-stick
- [ ] Layer 3 switch with SVIs
- [ ] Routed port

### Explanation
* **Key point:** Router-on-a-stick uses **one physical interface** carrying all VLANs over a trunk, with **subinterfaces** (each with `encapsulation dot1q`) acting as VLAN gateways.
* **Why not the others:** Legacy uses one physical interface *per VLAN*; SVIs live on a multilayer switch.

---

## Question 14: Most scalable method
Which inter-VLAN routing method is the most scalable and fastest for a large campus network?

- [ ] Legacy inter-VLAN routing
- [ ] Router-on-a-stick
- [x] Layer 3 (multilayer) switch using SVIs
- [ ] A separate router for each VLAN

### Explanation
* **Key point:** A Layer 3 switch routes **internally in hardware** — packets aren't squeezed down a single trunk for re-tagging — making it the most scalable and fastest option.
* **Why not the others:** Legacy is limited by physical port count; router-on-a-stick shares one trunk link as a potential bottleneck.

---

## Question 15: Router-on-a-stick configuration
Refer to the configuration. Which command correctly assigns subinterface G0/0/1.10 to VLAN 10?

```
R1(config)# interface gigabitEthernet 0/0/1.10
R1(config-subif)# ____________________
R1(config-subif)# ip address 192.168.10.1 255.255.255.0
```

- [ ] `switchport access vlan 10`
- [x] `encapsulation dot1q 10`
- [ ] `switchport trunk allowed vlan 10`
- [ ] `vlan 10`

### Explanation
* **Key point:** On a router subinterface you use **`encapsulation dot1q <vlan-id>`** to tell the router which VLAN's tagged traffic this subinterface handles.
* **Why not the others:** `switchport` commands are switch commands, not valid on a router subinterface.

---

## Question 16: Troubleshoot router-on-a-stick
Refer to the exhibit. Router-on-a-stick is configured with correct subinterface IPs and matching `encapsulation dot1q` values, but no VLANs can reach each other. The switch port toward the router is in access mode. What is the most likely cause?

- [ ] The router needs `ip routing` enabled.
- [x] The switch port facing the router must be configured as a trunk, not an access port.
- [ ] The subinterfaces need `no shutdown` individually.
- [ ] The native VLAN must be VLAN 1.

### Explanation
* **Key point:** Router-on-a-stick requires the switch port to be a **trunk** so tagged frames for every VLAN reach the router. An access port carries only one VLAN, so routing between VLANs fails.
* **Why not the others:** `ip routing` is a multilayer-switch requirement, not a router one. Subinterfaces come up with the physical interface (which must be `no shutdown`), and the native VLAN need not be VLAN 1.

---

## Question 17: Layer 3 switch SVIs
On a multilayer switch with SVIs configured for VLAN 10 and VLAN 20 (both showing "up/up"), hosts still cannot route between the two VLANs. Which command is most likely missing?

- [ ] `switchport mode trunk`
- [ ] `encapsulation dot1q 10`
- [x] `ip routing`
- [ ] `no switchport`

### Explanation
* **Key point:** A Layer 3 switch will not route between VLANs until **`ip routing`** is enabled in global configuration mode, even if the SVIs are up.
* **Why not the others:** Those commands belong to trunks, router subinterfaces, and routed ports respectively — not to enabling inter-VLAN routing on an L3 switch.

---

## Question 18: Routed ports
Which statement about a routed port on a multilayer switch is correct?

- [ ] It is associated with a single VLAN like an access port.
- [x] It is created with `no switchport` and does not support subinterfaces.
- [ ] It requires `encapsulation dot1q` to function.
- [ ] It can only be used on the access layer.

### Explanation
* **Key point:** A **routed port** is made with **`no switchport`**, acts like a router interface (gets an IP directly), is not tied to a VLAN, and — unlike a router — does **not** support subinterfaces.
* **Why not the others:** Routed ports are typically used between core/distribution switches and don't use 802.1Q subinterface encapsulation.

---

## Question 19: VLAN hopping — switch spoofing
How does a switch spoofing VLAN hopping attack succeed?

- [ ] The attacker floods the switch with broadcast frames.
- [x] The attacker's host imitates DTP/802.1Q signaling so the switch forms a trunk with it, exposing all VLANs.
- [ ] The attacker guesses the management VLAN password.
- [ ] The attacker double-encrypts frames to bypass the native VLAN.

### Explanation
* **Key point:** In switch spoofing, the host pretends to be a switch by sending DTP/802.1Q signals. If the port is in `dynamic auto`/`desirable`, it negotiates a trunk, giving the attacker access to **every VLAN**.
* **Why not the others:** Double-encryption and broadcast flooding describe different attacks; switch spoofing exploits **trunk negotiation**.

---

## Question 20: VLAN hopping — double-tagging
Which two statements about a double-tagging VLAN hopping attack are correct? (Choose two.)

- [x] The attacker must be connected to a port in the same VLAN as the trunk's native VLAN.
- [ ] The attack is bidirectional, allowing two-way conversations.
- [x] The first switch strips the outer tag (matching the native VLAN) and forwards the frame with its inner tag.
- [ ] It requires the attacker to know the switch's enable password.
- [ ] It works only over fiber trunk links.

### Explanation
* **Key point:** With two tags, the first switch removes the outer (native-VLAN) tag and forwards the frame still carrying the inner (target-VLAN) tag, which the next switch delivers to the victim VLAN. The attack is **unidirectional** and needs the attacker to be in the **native VLAN**.
* **Why not the others:** There is no return path (no reply), and it does not depend on passwords or media type.

---

## Question 21: VLAN attack mitigation (Choose three.)
Which three actions help prevent VLAN hopping attacks? (Choose three.)

- [x] Disable trunking on all access ports (force `switchport mode access`).
- [x] Disable automatic trunk negotiation so trunks are enabled manually.
- [x] Set the native VLAN to an unused VLAN used only on trunk links.
- [ ] Place all hosts in VLAN 1 for centralized control.
- [ ] Enable `dynamic desirable` on every access port.

### Explanation
* **Key point:** The three recommended guidelines are: disable trunking on access ports, disable auto-trunking (`switchport nonegotiate`), and dedicate the native VLAN to trunk links (an unused VLAN with no hosts).
* **Why not the others:** Putting hosts in VLAN 1 and enabling `dynamic desirable` make hopping *easier*, not harder.

---

## Question 22: Trunk verification command
Which command displays the native VLAN and the list of allowed VLANs on a trunk interface?

- [ ] `show vlan brief`
- [x] `show interfaces trunk`
- [ ] `show mac address-table`
- [ ] `show ip interface brief`

### Explanation
* **Key point:** **`show interfaces trunk`** lists trunking ports, their mode, encapsulation, native VLAN, and allowed/active VLANs.
* **Why not the others:** `show vlan brief` shows access-port-to-VLAN mappings, not trunk details.

---

## Question 23: Native VLAN mismatch
A trunk between S1 and S2 has S1 native VLAN 1 and S2 native VLAN 99. What is the likely consequence?

- [ ] The trunk speed drops to half-duplex.
- [x] A native VLAN mismatch is reported and traffic between the native VLANs can leak or be misforwarded.
- [ ] All VLANs are automatically deleted.
- [ ] The switches reboot to resync.

### Explanation
* **Key point:** The native VLAN must **match on both ends**. A mismatch triggers a CDP error and can cause traffic intended for one native VLAN to land in another — a security and connectivity problem.
* **Why not the others:** It does not change duplex, delete VLANs, or reboot switches.

---

## Question 24: Voice VLAN port
Which command set configures Fa0/2 so an IP phone uses VLAN 150 for voice while a PC behind it uses VLAN 20 for data?

- [ ] `switchport voice vlan 20` and `switchport access vlan 150`
- [x] `switchport access vlan 20` and `switchport voice vlan 150`
- [ ] `switchport trunk native vlan 150`
- [ ] `switchport mode trunk` and `switchport voice vlan 150`

### Explanation
* **Key point:** The data VLAN for the PC is set with `switchport access vlan 20`; the voice VLAN for the phone is set with `switchport voice vlan 150`. This lets one port serve both untagged PC traffic and tagged voice traffic.
* **Why not the others:** The first option swaps the VLANs, and trunk-mode commands are not used for this standard phone+PC port setup.
