# Chapter 1 Quiz — Network Communication

> Exam-style questions. Answers are marked and explained. Cover the explanations and test yourself first.

## Question 1: Hosts and end devices
Which statement best describes a host on a network?

- [ ] A device that works behind the scenes to forward data between other devices.
- [x] A device that participates directly in network communication and can be the source or destination of a message.
- [ ] A physical channel such as copper, fiber, or wireless over which a message travels.
- [ ] A software application provided by a server and used by clients.

### Explanation
* **Key point:** A **host** (end device) is the source or destination of a message — it sends and receives. Examples: PCs, phones, servers, printers.
* **Why not the others:** "Behind the scenes forwarding" describes an **intermediary device**; the third option is **media**; the fourth is a **service**.

---

## Question 2: Identify intermediary devices
From the following list, which three are intermediary devices? (Choose three.)

- [x] switch
- [ ] web server
- [x] wireless access point
- [ ] network printer
- [x] router
- [ ] IP phone

### Explanation
* **Key point:** Intermediary devices interconnect end devices and move data across the network: **switches, access points, routers, firewalls**.
* **Why not the others:** A web server, network printer, and IP phone are **end devices** — they are sources/destinations of messages, even though a server "serves" the network.

---

## Question 3: Peer-to-peer roles
In a peer-to-peer (P2P) network, what role can a single host play?

- [ ] Only a client.
- [ ] Only a server.
- [x] Both client and server at the same time.
- [ ] Neither; a dedicated device is always required.

### Explanation
* **Key point:** In P2P, one host can **request** information (client) and **provide** information (server) simultaneously — for example, sharing a printer while opening files from another PC.
* **Why not the others:** Being restricted to only one role describes the dedicated **client/server** model, not P2P.

---

## Question 4: Network media
Which option lists the three common types of network media?

- [ ] routers, switches, and firewalls
- [ ] confidentiality, integrity, and availability
- [x] metallic wires in cables, glass or plastic fiber, and wireless transmission
- [ ] LAN, WAN, and the internet

### Explanation
* **Key point:** **Media** is the physical channel a message travels over: **copper (metallic wire)**, **fiber-optic (glass/plastic)**, and **wireless**.
* **Why not the others:** Routers/switches/firewalls are devices; CIA is the security triad; LAN/WAN/internet are network types.

---

## Question 5: Topology diagram types
Refer to a diagram that labels each device with its IP addressing scheme and the ports in use, without showing room locations or cable runs. Which type of topology diagram is this?

- [ ] physical topology
- [x] logical topology
- [ ] converged topology
- [ ] hybrid topology

### Explanation
* **Key point:** A **logical** topology diagram identifies devices, ports, and the **IP addressing scheme** — how traffic logically flows.
* **Why not the others:** A **physical** topology shows physical location of devices, configured ports, and **cable installation**. "Converged" and "hybrid" are not topology-diagram types.

---

## Question 6: LAN vs WAN
Which two statements correctly describe a WAN? (Choose two.)

- [ ] It connects end devices within a single building or campus.
- [x] It connects networks over a wide geographic area such as between cities or countries.
- [ ] It is always administered by a single organization.
- [x] It is usually administered by multiple service providers and provides slower links between LANs.
- [ ] It provides the highest-speed bandwidth to internal end devices.

### Explanation
* **Key point:** A **WAN** spans a wide geographic area, is run by **multiple service providers**, and provides typically **slower** links that **connect LANs**.
* **Why not the others:** Connecting devices in one building/campus, single-organization administration, and high internal bandwidth all describe a **LAN**.

---

## Question 7: The "net" family
A hospital provides a booking system that allows outside doctors to log in and make appointments for their patients. What is this an example of?

- [ ] an intranet
- [x] an extranet
- [ ] the internet
- [ ] a SOHO network

### Explanation
* **Key point:** An **extranet** gives **secure, controlled access to outsiders** (here, external doctors) to a **subset** of an organization's information.
* **Why not the others:** An **intranet** is for the organization's own members only; the **internet** is the public worldwide network; a **SOHO** is a small office/home office network.

---

## Question 8: Matching network types
Match each description to the correct network term.

| Description | Network Term |
| :--- | :--- |
| A private network of LANs/WANs accessible only by an organization's own members | **intranet** |
| A worldwide collection of interconnected networks using common standards | **internet** |
| Controlled access to a subset of data for trusted outside partners | **extranet** |
| A small network for people who work from home or a small remote office | **SOHO** |

### Explanation
* **Key point:** *Intranet* = insiders only; *internet* = public network of networks; *extranet* = trusted outsiders, limited access; *SOHO* = small/home office network.
* **Trap to avoid:** Intranet and extranet differ only by **who** is allowed in — members vs. authorized outsiders.

---

## Question 9: SOHO definition
What term describes the type of network used by people who work from home or from a small remote office?

- [ ] WAN
- [ ] extranet
- [x] SOHO network
- [ ] converged network

### Explanation
* **Key point:** A **SOHO (Small Office / Home Office)** network supports self-employed and remote workers — advertising, selling, ordering supplies, and communicating with customers.
* **Why not the others:** A WAN connects LANs over distance; an extranet grants outsiders limited access; a converged network carries data/voice/video together.

---

## Question 10: ADSL characteristic
A home user connects to an ISP using ADSL. Which statement is true about this connection?

- [ ] It uses the same coaxial cable that delivers cable television.
- [ ] The upload speed is faster than the download speed.
- [x] The download speed is faster than the upload speed, and it runs over a telephone line.
- [ ] It requires a clear line of sight to a satellite.

### Explanation
* **Key point:** **ADSL = Asymmetric DSL**, so **download is faster than upload**, and DSL runs over the **telephone line**. It is always-on with high bandwidth.
* **Why not the others:** Coax-over-TV-cable describes **cable** internet; line of sight describes **satellite**; symmetric (equal) speeds describe **SDSL**.

---

## Question 11: Symmetric business connection
A business needs equal upload and download speeds over a DSL connection. Which option meets this requirement?

- [ ] ADSL
- [ ] dial-up
- [x] SDSL (Symmetric DSL)
- [ ] satellite

### Explanation
* **Key point:** **SDSL (Symmetric DSL)** provides uploads and downloads at the **same high speed**, a popular business choice.
* **Why not the others:** **ADSL** is asymmetric (download > upload); **dial-up** is very low bandwidth; **satellite** is for areas with no other option and needs line of sight.

---

## Question 12: Business internet connection
Which connection type is a reserved (private) circuit within a service provider's network that connects geographically separated offices and is rented at a monthly or yearly rate?

- [ ] broadband cable
- [ ] cellular
- [x] dedicated leased line
- [ ] dial-up telephone

### Explanation
* **Key point:** A **dedicated leased line** is a reserved circuit rented from the provider to privately connect separated offices for voice/data.
* **Why not the others:** Cable and cellular are typically shared, consumer-oriented broadband options; dial-up is low bandwidth and not dedicated.

---

## Question 13: Converged network
What is the defining characteristic of a converged network?

- [ ] It uses multiple separate networks, one each for data, voice, and video.
- [x] It carries multiple services — data, voice, and video — over a single network.
- [ ] It guarantees that no two devices share the same path.
- [ ] It only connects devices within a single LAN.

### Explanation
* **Key point:** A **converged** network carries **data, voice, and video on one network** instead of separate dedicated networks.
* **Why not the others:** The point of convergence is the opposite of "separate networks." It is also why **QoS** is needed — to prioritize real-time traffic when these services compete for bandwidth.

---

## Question 14: Fault tolerance and redundancy
A network is designed with multiple paths between switches so that traffic continues to flow if a single link fails. Which characteristic of a reliable network does this describe, and what is the design technique called?

- [x] fault tolerance, achieved through redundancy
- [ ] scalability, achieved through standards
- [ ] quality of service, achieved through prioritization
- [ ] security, achieved through encryption

### Explanation
* **Key point:** **Fault tolerance** limits the impact of a failure and allows quick recovery; it relies on **multiple paths**, and having those paths is called **redundancy**.
* **Why not the others:** Scalability is about growing without degrading service, QoS is about prioritizing traffic during congestion, and security protects the data and infrastructure.

---

## Question 15: Scalability
Which statement best describes a scalable network?

- [ ] It limits the number of devices affected by a single failure.
- [x] It can expand quickly to support new users and applications without degrading performance for existing users.
- [ ] It gives voice traffic priority over data during congestion.
- [ ] It ensures only authorized recipients can read the data.

### Explanation
* **Key point:** **Scalability** = grow quickly to add users/applications **without degrading** existing service, achieved by following **accepted standards and protocols**.
* **Why not the others:** Those describe fault tolerance, QoS, and confidentiality (security), respectively.

---

## Question 16: QoS purpose
What type of network traffic most requires QoS, and why?

- [ ] file downloads, because they use the most total bandwidth
- [ ] email, because it must be delivered reliably
- [x] real-time traffic such as voice and video, because congestion would disrupt delivery
- [ ] background backups, because they run continuously

### Explanation
* **Key point:** **QoS** manages **congestion** and prioritizes **real-time** traffic (voice/video), giving it priority so it isn't disrupted when bandwidth demand exceeds availability.
* **Why not the others:** File downloads, email, and backups are not time-sensitive; they tolerate delay, so they receive lower priority.

---

## Question 17: The CIA triad
Match each network security requirement to its description.

| Description | Security Requirement |
| :--- | :--- |
| Only intended, authorized recipients can access and read the data | **confidentiality** |
| Assurance that information was not altered in transit from origin to destination | **integrity** |
| Assurance of timely and reliable access to data and services for authorized users | **availability** |

### Explanation
* **Key point:** These are the three primary security requirements (the **CIA triad**): **C**onfidentiality, **I**ntegrity, **A**vailability.
* **Trap to avoid:** *Altering* data attacks **integrity**; *reading* data attacks **confidentiality**; a *denial-of-service* attack attacks **availability**.

---

## Question 18: Network trends — cloud types
What are the four primary types of clouds?

- [ ] Local, Remote, Edge, and Core
- [x] Public, Private, Hybrid, and Community
- [ ] Wired, Wireless, Powerline, and Satellite
- [ ] LAN, WAN, Intranet, and Extranet

### Explanation
* **Key point:** The four primary cloud types are **Public, Private, Hybrid, and Community**.
* **Why not the others:** The other lists mix up media types, network types, or invented categories.

---

## Question 19: Powerline networking
Which statement correctly describes powerline networking for a home network?

- [ ] It transmits data as pulses of light through fiber-optic cable.
- [x] It uses existing electrical wiring and a powerline adapter so devices can connect to the LAN at an electrical outlet.
- [ ] It requires a clear line of sight to a satellite.
- [ ] It is a wireless technology that replaces all cabling with radio waves.

### Explanation
* **Key point:** **Powerline networking** uses **existing electrical wiring** plus a standard powerline adapter — connect wherever there's an outlet, with **no data cables** to install.
* **Why not the others:** Fiber uses light; satellite needs line of sight; powerline is not the same as Wi-Fi.

---

## Question 20: BYOD
Which phrase best captures the concept of BYOD (Bring Your Own Device)?

- [ ] Only company-owned devices may access the network.
- [x] Any device, with any ownership, used anywhere to access the network.
- [ ] A dedicated firewall placed at the network edge.
- [ ] A private circuit rented from a service provider.

### Explanation
* **Key point:** **BYOD** = "any device, any ownership, used anywhere," letting end users access the business/campus network with their personal tools.
* **Why not the others:** Restricting to company devices is the opposite of BYOD; the others describe a firewall and a leased line.

---

## Question 21: Zero-day attack
What is a zero-day attack?

- [ ] An attack that steals a user's login credentials to access private data.
- [ ] Software that secretly collects information about a user.
- [x] An attack that exploits a vulnerability on the first day it becomes known, before a fix is available.
- [ ] An attack that slows or crashes applications on a network device.

### Explanation
* **Key point:** A **zero-day** attack occurs on the **first day a vulnerability becomes known** — before a patch exists, which is what makes it dangerous.
* **Why not the others:** Stealing credentials is **identity theft**; secretly collecting info is **spyware**; slowing/crashing services is a **denial-of-service** attack.

---

## Question 22: Defenses for larger networks
Which three security measures are typically added for larger or corporate networks beyond the basic antivirus and firewall used at home? (Choose three.)

- [x] Access Control Lists (ACLs)
- [ ] antivirus software on each end device
- [x] Intrusion Prevention Systems (IPS)
- [x] Virtual Private Networks (VPNs)
- [ ] a single home-router firewall as the only protection

### Explanation
* **Key point:** Larger networks add **ACLs** (filter by IP/application), **IPS** (catch fast-spreading/zero-day threats), and **VPNs** (secure remote-worker access), often with **dedicated firewall systems**.
* **Why not the others:** Antivirus on end devices and a basic firewall are the **home/small office** baseline, not the corporate-specific additions.

---

## Question 23: Three categories of components
A network infrastructure is described as containing three categories of components. Which option lists all three correctly?

- [ ] hosts, intermediary devices, and clients
- [ ] LAN, WAN, and internet
- [x] devices, media, and services
- [ ] confidentiality, integrity, and availability

### Explanation
* **Key point:** The infrastructure has three categories: **devices** and **media** (the physical hardware) and **services** (applications like email/web hosting provided by servers).
* **Why not the others:** Hosts/intermediary/clients are sub-categories of devices; LAN/WAN/internet are network types; CIA is the security triad.
