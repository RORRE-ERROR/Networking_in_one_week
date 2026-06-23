# Chapter 1 — Network Communication

> **Day 1 of 7** · Estimated study time: ~3 hours
> *This chapter is the "big picture" of networking: what the pieces are, what we call them, how networks are sized and connected to the internet, and what makes a network actually good (reliable). Almost no math here — it's vocabulary and concepts, but they show up constantly later, so learn them properly now.*

## 🎯 What you'll be able to do after this chapter
- Name the three categories of network components (devices, media, services) and sort real-world gadgets into **end devices** vs **intermediary devices**.
- Tell the difference between a **physical** and a **logical** topology diagram.
- Classify a network as **LAN, WAN, internet, intranet, or extranet**, and recognise a **SOHO** network.
- Compare the common ways homes and businesses connect to the internet (cable, DSL, fiber, cellular, satellite, dial-up; leased line, Metro Ethernet, business DSL).
- Explain the four characteristics of a reliable network: **fault tolerance, scalability, QoS, security**.
- Describe modern network trends (BYOD, online collaboration, cloud, smart home, powerline networking) and common security threats.

---

## 1. Introduction — why networks matter

Intuition first: a **network** is just two or more devices connected so they can share information. That's it. The "internet" you use every day is simply an enormous number of these networks joined together.

A few framing ideas from the slides:
- **Internet of Everything (IoE)** — the idea that not just computers, but *people, processes, data, and things* (fridges, cars, sensors) are all becoming connected and useful together.
- The **cloud** lets you store files and reach them from anywhere — your data lives on someone else's servers on the internet, not only on your own laptop.

The key vocabulary to lock in right away:
- A **host** (also called an **end device**) is any device that participates *directly* in network communication — it can send and receive messages. Your phone, laptop, and a web server are all hosts.
- A **client** is an end device that *requests* information (e.g., your browser asking for a web page).
- A **server** is a computer running software that *provides* information or services (web pages, email, files). Each service needs its own server software — a web server runs web server software, a mail server runs mail server software.

> 💡 **Exam tip:** In a **peer-to-peer (P2P)** network, a single host can act as **client and server at the same time** (e.g., sharing a printer while also opening files from another PC). The exam likes to contrast this with the dedicated client/server model.

---

## 2. Network Components

Cisco splits the network infrastructure into **three categories**:

| Category | What it is | Examples |
| --- | --- | --- |
| **Devices** | The physical hardware | PCs, switches, routers |
| **Media** | The channel the data travels over | Copper cable, fiber, wireless |
| **Services** | The applications people use | Email hosting, web hosting |

Devices and media are the *physical* (hardware) elements. **Services** are software things provided by servers and reached by clients across the network.

The devices themselves split into two important groups. **Learn this division cold — it is the single most-tested item in this chapter.**

### 2a. End devices (hosts)
These form the **interface between users and the network** — where a message starts or ends (its source or destination).
- Computers (workstations, laptops, file servers, web servers)
- Network printers
- VoIP phones
- TelePresence / video endpoints
- Security cameras
- Mobile handheld devices (smartphones, tablets, barcode scanners, wireless card readers)

### 2b. Intermediary devices
These **interconnect end devices** and work behind the scenes to keep data flowing. They connect individual hosts to the network and can join multiple networks into an **internetwork**.
- **Network access:** switches and wireless access points (APs)
- **Internetworking:** routers
- **Security:** firewalls

> 💡 **Exam tip:** A *server* is an **end device**, not an intermediary device, even though it "serves" the network. The test loves to slip a server (or printer, or IP phone) into a list and ask you to pick the intermediary devices — those are switches, APs, routers, and firewalls.

### 2c. Network media
The **medium** is the physical channel the message travels over from source to destination. Three common types:
- **Metallic wires** inside cables (copper — carries *electrical* signals)
- **Glass or plastic fibers** — fiber-optic cable (carries *pulses of light*)
- **Wireless** transmission (carries *radio waves* / microwaves)

> 💡 **Exam tip:** Fiber's headline advantages: **longer distances**, **higher bandwidth**, and **immunity to electromagnetic interference (EMI/RFI)** because it uses light, not electricity. Copper is limited to ~100 m for Ethernet.

---

## 3. Network Representations (diagrams)

Intuition: a network can have hundreds of devices, so we draw maps. A **topology diagram** is a visual map of how the network is connected, using standard icons. Being able to read these is "critical to visualizing the organization and operation of a network."

There are **two types**:

| Diagram type | Shows | Think of it as... |
| --- | --- | --- |
| **Physical topology** | Physical *location* of devices, the ports/interfaces used, and cable runs | "Where is the cable actually plugged in, in which room?" |
| **Logical topology** | The devices, ports, and the **IP addressing scheme** | "What are the addresses and how does traffic logically flow?" |

> 💡 **Exam tip:** If the answer mentions **IP addresses / addressing scheme**, it's a **logical** topology. If it mentions **cable installation, room location, or physical ports**, it's a **physical** topology.

A few standard terms you'll see on diagrams:
- **NIC (Network Interface Card)** — connects a host to the network media.
- **Port / Interface** — the connector or socket where media plugs in.
- **Physical port** is a hardware connector; an **interface** is the port specifically used for networking (often used interchangeably at this level).

---

## 4. Types of Networks

Networks range from two PCs in a bedroom to millions of devices worldwide. They differ by: **area covered, number of users, services offered, and who is responsible** for administering them.

### The two main infrastructure types

| | **LAN — Local Area Network** | **WAN — Wide Area Network** |
| --- | --- | --- |
| Area | Small geographic area (home, school, one office, campus) | Large geographic area (between cities, countries, continents) |
| Administered by | Usually **one** organization/individual | Usually **multiple service providers (ISPs)** |
| Speed | **High-speed** bandwidth internally | Typically **slower** links between LANs |
| Job | Connects end devices in a limited area | Connects LANs together over distance |

> 💡 **Exam tip:** Memorize the three contrasts — **size, ownership, speed**. LAN = small/one owner/fast; WAN = large/multiple providers/slower. A WAN's purpose is to **connect LANs** over distance.

### The "net" family
- **Internet** — a worldwide collection of interconnected networks that cooperate using common standards. (A "network of networks.") No single owner.
- **Intranet** — a *private* connection of LANs/WANs belonging to **one organization**, accessible only by its members/employees/authorized users.
- **Extranet** — gives **secure, controlled access to outsiders** who need some company data (suppliers, contractors, partner doctors), but only to a *subset* of information.

> 💡 **Exam tip:** Remember the audience: **intranet = insiders only; extranet = trusted outsiders given limited access.** Example: a hospital giving outside doctors a booking system = extranet.

### SOHO
A **SOHO (Small Office / Home Office)** network lets people work from home or a small remote office — used by self-employed workers to advertise, sell, order supplies, and communicate with customers.

> 💡 **Exam tip:** "A network for people who work from home or a small remote office" is the textbook definition of a **SOHO network**.

---

## 5. Internet Connections

Home users, remote workers, and small offices connect to an **ISP (Internet Service Provider)** to reach the internet. Options vary by ISP and location.

### Home / small office options

| Connection | Key facts |
| --- | --- |
| **Cable** | Runs over the same coax that delivers cable TV. High bandwidth, high availability, **always-on**. |
| **DSL** (Digital Subscriber Line) | Runs over the **telephone line**. High bandwidth, always-on. Home users usually get **ADSL** — *Asymmetric*, meaning **download is faster than upload**. |
| **Cellular** | Uses the cell-phone network (3G / 4G / 5G). Good for mobility. |
| **Satellite** | Useful where nothing else reaches. Needs a **clear line of sight** to the satellite. |
| **Dial-up telephone** | Cheap, uses an ordinary phone line + modem. **Very low bandwidth** — okay for light/mobile use, not large transfers. |

> 💡 **Exam tip:** **ADSL = Asymmetric = download faster than upload** (typical home use). Contrast with business **SDSL = Symmetric = upload and download at the same high speed**. The word *symmetric* is the giveaway.

### Business connections
Businesses often need **higher, dedicated bandwidth and managed services**.

| Connection | Key facts |
| --- | --- |
| **Dedicated leased line** | Reserved (private) circuit through the provider's network connecting separated offices; rented monthly/yearly. |
| **Metro Ethernet** | Extends LAN (Ethernet) technology into the WAN. |
| **Business DSL** | Various formats; popular choice **SDSL (Symmetric DSL)** — equal high-speed upload and download. |

A **converged network** carries multiple services — data, voice, and video — over **one** network instead of separate networks for each.

> 💡 **Exam tip:** "Convergence" = data + voice + video on **one** network. This is *why* QoS (next section) becomes necessary.

---

## 6. Reliable Network — the four characteristics

**Network architecture** = the technologies, services, and **protocols** (rules) that move data across the network. A network design must meet user expectations on **four** characteristics. Expect at least one question on these.

1. **Fault tolerance** — *limits the impact of a failure* so the fewest devices are affected, and allows **quick recovery**. It depends on **multiple paths** between source and destination; if one path fails, traffic instantly uses another. Having those multiple paths is called **redundancy**.
2. **Scalability** — can **expand quickly** to support new users and applications **without degrading** performance for existing users. Achieved by following **accepted standards and protocols**.
3. **Quality of Service (QoS)** — manages **congestion** and ensures reliable delivery as data/voice/video converge. When demand for bandwidth exceeds what's available, you get congestion; a **QoS policy** lets the router **prioritize** traffic (e.g., give **voice** priority over a file download).
4. **Security** — protecting the infrastructure and the data on it. Two angles:
   - **Network security** — physically securing devices and preventing unauthorized access to their management software.
   - **Information security** — protecting the data *in transit* (in the packets) and *at rest* (stored on devices).

> 💡 **Exam tip:** Match the keyword to the characteristic — **redundancy / multiple paths → fault tolerance**; **grow without slowdown → scalability**; **prioritize voice during congestion → QoS**; **prevent unauthorized access → security**.

### The three security goals (CIA)
The slides call these the three primary security requirements — known broadly as the **CIA triad**:
- **Confidentiality** — only the intended, authorized recipients can read the data.
- **Integrity** — assurance the information was **not altered** in transit from origin to destination.
- **Availability** — assurance of **timely and reliable access** to data/services for authorized users.

> 💡 **Exam tip:** Don't mix these up — *altered data* attacks **integrity**; *eavesdropping/reading data* attacks **confidentiality**; a *denial-of-service attack* attacks **availability**.

---

## 7. Network Trends

- **BYOD (Bring Your Own Device)** — *any device, any ownership, used anywhere.* Lets users use personal devices to access the business/campus network.
- **Online collaboration** — working with others on a joint project; tools like **Cisco Webex Teams** (instant messages, file/image/video sharing).
- **Video communication** — video conferencing as a tool for local and global communication.
- **Cloud computing** — store/back up files and run apps (word processing, photo editing) on servers over the internet. **Four types of clouds:** **Public, Private, Hybrid, Community.**
- **Smart home** — everyday appliances become "smart"/automated and connect with other devices.
- **Powerline networking** — uses **existing electrical wiring** + a powerline adapter to connect a device to the LAN at any electrical outlet; no new data cables needed.
- **Wireless ISP (WISP)** — common in rural areas; an ISP that delivers internet access over **wireless** (radio) links instead of running cable to each home.

> 💡 **Exam tip:** **Four cloud types = Public, Private, Hybrid, Community.** And **powerline networking = data over existing electrical wiring** (it does *not* replace high-bandwidth needs, just adds convenient connectivity where there's an outlet).

---

## 8. Network Security threats & defenses

**External threats** the slides list:
- **Viruses, worms, Trojan horses** — malicious software/code running on a device.
- **Spyware and adware** — secretly collect information about the user.
- **Zero-day attacks** — exploit a vulnerability on the **first day** it becomes known (before a patch exists).
- **Threat actor attacks** — a malicious person attacks devices/resources.
- **Denial-of-Service (DoS)** — slow or crash apps/processes on a device (attacks **availability**).
- **Data interception and theft** — capture private info crossing the network.
- **Identity theft** — steal a user's **login credentials** to reach private data.

**Defenses — home / small office (basic):**
- **Antivirus / antispyware** software on end devices.
- **Firewall filtering** to block unauthorized traffic in and out.

**Defenses — larger / corporate networks (add):**
- **Dedicated firewall systems** — filter large traffic volumes with more granularity.
- **Access Control Lists (ACLs)** — filter access/forwarding based on IP addresses and applications.
- **Intrusion Prevention Systems (IPS)** — catch fast-spreading threats like zero-day attacks.
- **VPNs (Virtual Private Networks)** — give remote workers **secure** access into the organization.

> 💡 **Exam tip:** Know which defenses are "home/small office" (antivirus, basic firewall) vs "added for large networks" (dedicated firewalls, ACLs, IPS, VPN). And remember **zero-day = day the vulnerability becomes known**, before a fix is available.

---

## 🧪 Hands-on (Packet Tracer)
- **1.5.5 Packet Tracer — Network Representation:** Explore a network in Packet Tracer; identify end devices vs intermediary devices and the media connecting them, and read the topology the way the diagrams in Section 3 describe.
- **1.5.7 Packet Tracer — Network Representation:** Practice recognizing logical vs physical layout — locate devices, their ports/interfaces, and the addressing scheme so you can connect "icon on a diagram" to "real device in the network."

---

## 🧠 Key terms to memorize

| Term | Meaning in one line |
| --- | --- |
| **Host / end device** | Source or destination of a message; sends and receives (PC, phone, server, printer). |
| **Intermediary device** | Connects/forwards between hosts: switch, AP, router, firewall. |
| **Media** | Physical channel: copper, fiber, or wireless. |
| **Server / Client** | Server provides a service; client requests it. P2P = both at once. |
| **Topology diagram** | Visual map of how a network connects. |
| **Physical topology** | Location of devices, ports, cable runs. |
| **Logical topology** | Devices, ports, and **IP addressing scheme**. |
| **LAN** | Small area, one owner, high speed. |
| **WAN** | Large area, multiple providers, slower; connects LANs. |
| **Internet / Intranet / Extranet** | Public worldwide / private internal / limited access for trusted outsiders. |
| **SOHO** | Small office / home office network. |
| **ISP** | Internet Service Provider. |
| **ADSL / SDSL** | Asymmetric (download faster) / Symmetric (equal speeds). |
| **Converged network** | Data + voice + video on one network. |
| **Fault tolerance** | Limits failure impact via redundancy (multiple paths). |
| **Scalability** | Grows without degrading existing service. |
| **QoS** | Prioritizes traffic to manage congestion. |
| **CIA triad** | Confidentiality, Integrity, Availability. |
| **BYOD** | Bring Your Own Device. |
| **Cloud types** | Public, Private, Hybrid, Community. |
| **Powerline networking** | Networking over existing electrical wiring. |
| **Zero-day attack** | Exploit on the first day a vulnerability is known. |

---

## ⚡ Exam tips & traps (recap)
- **End vs intermediary:** servers, printers, IP phones, cameras = **end devices**. Switches, APs, routers, firewalls = **intermediary devices**.
- **Logical topology = IP addressing scheme; physical topology = cable/room/port locations.**
- **LAN vs WAN:** size, ownership (one vs many providers), speed. WANs **connect LANs**.
- **Intranet = insiders; Extranet = trusted outsiders with limited access.**
- **ADSL = download > upload; SDSL = equal (business).** "Asymmetric" vs "Symmetric" is the clue.
- **Redundancy = multiple paths = fault tolerance.** Don't confuse with scalability.
- **QoS prioritizes real-time traffic (voice/video) during congestion.**
- **CIA:** altered data → integrity; eavesdropping → confidentiality; DoS → availability.
- **Four cloud types: Public, Private, Hybrid, Community.**
- **Zero-day = vulnerability exploited the first day it's known** (no patch yet).
- **Convergence (one network for data/voice/video) is the reason QoS exists.**

---

## ✅ Quick self-check
1. List the three categories of network components, and give one example of each.
2. A switch, a laptop, a router, a web server, a firewall, and a printer are on a list. Which are **intermediary** devices?
3. You see a diagram labeled with each device's IP address. Is it a physical or logical topology — and why?
4. A hospital lets outside doctors log in to book appointments for their patients. Internet, intranet, or extranet?
5. Name the four characteristics of a reliable network, and say which one provides "multiple paths so traffic survives a link failure."

*(Answers are in `Quiz.md`.)*
