# Chapter 8 Quiz — Switching Concept

> Exam-style questions. Answers are marked and explained. Cover the explanations and test yourself first.

## Question 1: Campus layer roles
Which campus hierarchy layer represents the network edge and provides network access to end users?

- [x] access layer
- [ ] distribution layer
- [ ] core layer
- [ ] backbone layer

### Explanation
* **Key point:** The **access layer** is the network edge — the place where users plug in and gain access to the network.
* **Why not the others:** The **distribution layer** interfaces between access and core (and adds redundancy/high availability). The **core layer** is the high-speed backbone that aggregates the campus. "Backbone" is just another name for the core, not a separate option.

---

## Question 2: Switch learning
On what does a switch base its decision to **learn** a new entry into its MAC address table?

- [ ] the destination MAC address of the frame
- [x] the source MAC address of the frame and the ingress port
- [ ] the destination IP address of the packet
- [ ] the egress port and the source IP address

### Explanation
* **Key point:** A switch **learns from the SOURCE MAC address** plus the **ingress port** it arrived on. If that source MAC is new, it adds MAC + port to the table; if it already exists, it just refreshes the timer.
* **Why not the others:** The **destination** MAC is used to decide *forwarding*, not learning. Switches operate at Layer 2 and do not look at IP addresses to build the MAC table.

---

## Question 3: Unknown unicast behavior
A switch receives a frame whose destination MAC address is a unicast that is **not** present in its MAC address table. What does the switch do?

- [ ] It drops the frame.
- [ ] It forwards the frame only back out the port it arrived on.
- [x] It floods the frame out all ports except the ingress port.
- [ ] It sends an ARP request for the destination MAC.

### Explanation
* **Key point:** When the destination unicast MAC isn't in the table, the switch performs an **unknown unicast** flood — sending the frame out **every port except the one it arrived on**.
* **Why not the others:** Switches never forward a frame back out its ingress port, and they don't drop unknown unicasts — they flood. ARP is a host/router function, not a switch action here.

---

## Question 4: Default MAC aging timer
By default, how long does a typical Cisco Ethernet switch keep an inactive entry in its MAC address table before aging it out?

- [ ] 30 seconds
- [ ] 1 minute
- [x] 5 minutes
- [ ] 60 minutes

### Explanation
* **Key point:** The default MAC address table aging time is **5 minutes (300 seconds)** of inactivity. Each time a source MAC is seen again, its timer is refreshed.

---

## Question 5: Collision domains and duplex
Which statement about collision domains on a modern switch is correct? (Choose two.)

- [x] When a switch port operates in half-duplex, that segment is its own collision domain.
- [x] When switch ports operate in full-duplex, there are no collision domains.
- [ ] All ports on a switch share a single collision domain.
- [ ] A switch port connected to a hub eliminates the collision domain.
- [ ] Full-duplex operation increases the number of collision domains.

### Explanation
* **Key point:** Switches provide **microsegmentation** — each port is its own collision domain in half-duplex. In **full-duplex**, transmit and receive use separate paths, so collisions cannot occur and there are **no collision domains**.
* **Why not the others:** Hubs (half-duplex, shared media) *create* a collision domain, they don't eliminate one. A single shared collision domain describes a hub, not a switch.

---

## Question 6: Broadcast domains
A network technician interconnects four switches with no VLANs configured. How many broadcast domains exist?

- [ ] four — one per switch
- [x] one
- [ ] eight
- [ ] one per active port

### Explanation
* **Key point:** A collection of interconnected switches forms **one single broadcast domain** (the MAC broadcast domain). Switches break up collision domains but **not** broadcast domains.
* **Why not the others:** To create additional broadcast domains you must use **VLANs** or a **router** (a Layer 3 boundary). Without them, all four switches are one broadcast domain.

---

## Question 7: Forwarding methods — error checking
Which switch forwarding method receives the **entire** frame and performs an FCS (CRC) check before forwarding it?

- [x] store-and-forward
- [ ] cut-through fast-forward
- [ ] cut-through fragment-free
- [ ] flood-and-prune

### Explanation
* **Key point:** **Store-and-forward** buffers the whole frame, runs the **FCS/CRC** error check, and discards corrupt frames before forwarding. It is also **Cisco's default** method and is required for QoS and speed-mismatch buffering.
* **Why not the others:** Both cut-through variants start forwarding early and do **not** discard errored frames. There is no "flood-and-prune" forwarding method.

---

## Question 8: Fragment-free cut-through
How does the **fragment-free** variant of cut-through switching reduce the chance of forwarding a corrupt frame?

- [ ] It reads only the destination MAC address before forwarding.
- [x] It reads the first 64 bytes of the frame before forwarding.
- [ ] It performs a full FCS check on the entire frame.
- [ ] It buffers the frame for 5 minutes.

### Explanation
* **Key point:** **Fragment-free** waits to read the **first 64 bytes** before forwarding. Because most collisions and errors occur within the first 64 bytes, this catches the worst frames — a compromise between fast-forward and store-and-forward.
* **Why not the others:** Reading only the destination MAC describes **fast-forward**. A full FCS check on the entire frame is **store-and-forward**.

---

## Question 9: Frame buffering across speeds
Why do switches use large frame buffers (memory buffering)?

- [ ] to permanently store the MAC address table
- [x] to temporarily store frames so a faster ingress port can feed a slower egress port without dropping frames
- [ ] to encrypt frames before forwarding
- [ ] to convert frames into packets

### Explanation
* **Key point:** Buffers let the switch hold received frames temporarily. This allows traffic arriving on a **faster port (e.g., 1 Gbps)** to be forwarded to a **slower port (e.g., 100 Mbps)** without losing frames.
* **Why not the others:** The MAC table is stored in CAM, not the frame buffer; switches don't encrypt frames or change them into packets (that's Layer 3).

---

## Question 10: Switch boot sequence
Place the switch boot steps in the correct order.

| Order | Step |
| :--- | :--- |
| **1** | POST runs from ROM (tests CPU, DRAM, flash file system) |
| **2** | Boot loader runs and performs low-level CPU initialization |
| **3** | Boot loader initializes the flash file system |
| **4** | Boot loader locates and loads the IOS image into memory |
| **5** | IOS initializes interfaces using the startup-config in NVRAM |

### Explanation
* **Key point:** The order is **POST → boot loader → flash file system → load IOS → IOS reads startup-config**. POST and the boot loader live in **ROM**; the IOS image is in **flash**; the **startup-config** is in **NVRAM**.

---

## Question 11: Management IP configuration
Refer to the configuration. A switch must be managed remotely from a different subnet. Which two commands are required to make this work? (Choose two.)

- [x] `interface vlan 99` followed by `ip address 192.168.1.2 255.255.255.0`
- [ ] `interface FastEthernet0/1` followed by `ip address 192.168.1.2 255.255.255.0`
- [x] `ip default-gateway 192.168.1.1`
- [ ] `ip route 0.0.0.0 0.0.0.0 192.168.1.1`
- [ ] `ip address dhcp` on the console line

### Explanation
* **Key point:** A switch's management IP goes on a **Switch Virtual Interface (SVI)**, not a physical port. To reach managers on **other subnets**, the switch also needs **`ip default-gateway`**.
* **Why not the others:** Layer 2 access ports cannot take an IP address. `ip route` is a Layer 3 routing command used on routers (or a multilayer switch in routing mode), not the standard way to set a basic switch's gateway.

---

## Question 12: SVI management VLAN best practice
Why is it considered a best practice to use a VLAN other than VLAN 1 for switch management?

- [ ] VLAN 1 cannot have an IP address assigned.
- [x] VLAN 1 is the default on all ports, so using a different VLAN for management improves security.
- [ ] SSH only works on VLANs numbered above 1.
- [ ] VLAN 1 does not support the SVI interface.

### Explanation
* **Key point:** By default all ports are in **VLAN 1**, which makes it a predictable target. Moving management to a separate VLAN reduces the attack surface — a security best practice.
* **Why not the others:** VLAN 1 *can* have an SVI and an IP; SSH is not restricted by VLAN number. The recommendation is about security, not capability.

---

## Question 13: Auto-MDIX
What is the effect of enabling auto-MDIX on a switch port?

- [ ] It automatically negotiates the IP address of the connected device.
- [x] It automatically detects and adjusts for the required cable type (straight-through vs crossover).
- [ ] It forces the port into full-duplex mode.
- [ ] It encrypts all traffic on the port.

### Explanation
* **Key point:** **Auto-MDIX** lets the interface automatically detect the required cable connection, so a straight-through-vs-crossover mismatch no longer breaks the link.
* **Why not the others:** Auto-MDIX deals with cabling polarity only — not IP negotiation, duplex forcing, or encryption.

---

## Question 14: SSH vs Telnet
Which two statements correctly compare SSH and Telnet? (Choose two.)

- [x] SSH provides strong encryption of authentication and data; Telnet sends them in plaintext.
- [x] SSH uses TCP port 22 and Telnet uses TCP port 23.
- [ ] Telnet uses TCP port 22 and SSH uses TCP port 23.
- [ ] Both protocols encrypt the session by default.
- [ ] Telnet is preferred for remote management because it is more secure.

### Explanation
* **Key point:** **SSH (TCP 22)** encrypts the entire session; **Telnet (TCP 23)** transmits usernames, passwords, and data in **plaintext**. SSH is the secure, preferred choice.
* **Why not the others:** The ports are not swapped, Telnet has no encryption, and Telnet is the *insecure* option.

---

## Question 15: SSH prerequisites
A network administrator enters `crypto key generate rsa` but the command is refused. Which two configurations are prerequisites for generating the RSA keys? (Choose two.)

- [x] a configured hostname
- [x] a configured IP domain name
- [ ] a configured default gateway
- [ ] an enabled HTTP server
- [ ] a configured banner motd

### Explanation
* **Key point:** SSH key generation requires both a **hostname** and an **IP domain name** — together they form the fully-qualified name used to label the RSA key pair. The minimum modulus is **1024 bits**.
* **Why not the others:** A default gateway, HTTP server, and login banner are unrelated to RSA key generation.

---

## Question 16: Restricting vty lines to SSH
Which command, applied under the vty lines, ensures the switch accepts **only** SSH connections and rejects Telnet?

- [ ] `login local`
- [ ] `ip ssh version 2`
- [x] `transport input ssh`
- [ ] `service password-encryption`

### Explanation
* **Key point:** **`transport input ssh`** on the vty lines permits SSH only and blocks Telnet. (`login local` tells the lines to authenticate against the local username database, but it does not disable Telnet.)
* **Why not the others:** `ip ssh version 2` forces SSHv2 but doesn't block Telnet; `service password-encryption` only obscures stored passwords.

---

## Question 17: MAC flooding attack
How does a MAC address (CAM) flooding attack allow an attacker to capture traffic intended for other hosts?

- [ ] It changes the switch's default gateway to the attacker's PC.
- [x] It fills the MAC address table with bogus entries, forcing the switch to flood incoming frames out all ports.
- [ ] It assigns the attacker's MAC to the victim's IP via ARP.
- [ ] It disables the switch's SVI so no MACs can be learned.

### Explanation
* **Key point:** A tool like **macof** floods the switch with fake source MACs until the **finite** CAM table is full. The switch can no longer learn legitimate entries, so it treats traffic as **unknown unicast and floods it out all ports** in the VLAN — letting the attacker sniff it.
* **Why not the others:** Gateway changes and ARP-to-IP mapping describe different attacks (e.g., ARP spoofing). Disabling the SVI is not how flooding works.

---

## Question 18: Sticky vs dynamic secure MACs
Match each port-security MAC type to its description.

| MAC type | Description |
| :--- | :--- |
| **Static** | Manually configured on the port and stored in the running-config |
| **Dynamic** | Automatically learned, stored only in the MAC table, and **lost on reboot** |
| **Sticky** | Learned dynamically (or set manually) **and** added to the running-config so it survives a reboot once saved |

### Explanation
* **Key point:** **Dynamic** secure MACs disappear when the switch restarts because they live only in the address table. **Sticky** addresses are learned dynamically *but* written into the **running-config**, so after `copy running-config startup-config` they behave like static entries. **Static** addresses are typed in by the administrator.

---

## Question 19: Default violation mode
A port is configured with port security but no `violation` keyword is specified. A violation then occurs. What happens to the port?

- [ ] Traffic from the offending MAC is dropped but the port stays up with no notification.
- [ ] Traffic is dropped, a syslog message is logged, and the port stays up.
- [x] The port is placed in the err-disabled state and its LED turns off.
- [ ] The maximum MAC count is automatically increased.

### Explanation
* **Key point:** The **default** violation mode is **shutdown**. On a violation the port immediately becomes **err-disabled**, the LED turns off, and the violation counter increments. Recovery requires `shutdown` then `no shutdown`.
* **Why not the others:** "Drop, no notification" describes **protect**; "drop + log, port stays up" describes **restrict**. Neither is the default.

---

## Question 20: Comparing violation modes
Which violation mode drops offending traffic **and** sends a notification, but does **not** disable the port?

- [ ] protect
- [x] restrict
- [ ] shutdown
- [ ] err-disable

### Explanation
* **Key point:** **Restrict** drops packets from unknown sources, **sends a notification** (syslog/SNMP), and **increments the violation counter**, while leaving the port **operational**.
* **Why not the others:** **Protect** drops silently (no notification, no counter). **Shutdown** (the default) err-disables the port. "Err-disable" is a port state, not a violation mode.

---

## Question 21: Enabling port security on a port
Which command must be configured on an interface **before** `switchport port-security` will take effect?

- [ ] `switchport mode trunk`
- [x] `switchport mode access`
- [ ] `no shutdown`
- [ ] `spanning-tree portfast`

### Explanation
* **Key point:** Port security only operates on a **static access** port, so you must first set **`switchport mode access`**. Applying port security to a port left in dynamic/auto mode will not work.
* **Why not the others:** Port security is not configured on trunk ports in the basic CCNA scenario; `no shutdown` and PortFast are unrelated prerequisites.

---

## Question 22: Hardening unused ports
What is a recommended best practice for switch ports that are not in use?

- [ ] Configure them as trunk ports for flexibility.
- [ ] Assign them all to VLAN 1.
- [x] Administratively shut them down.
- [ ] Enable Telnet on them for quick access.

### Explanation
* **Key point:** Unused ports are a potential entry point for attackers, so the best practice is to **administratively shut them down** (e.g., `interface range ... ` then `shutdown`).
* **Why not the others:** Trunking unused ports and dumping them in VLAN 1 *increases* risk; enabling Telnet anywhere is insecure.
