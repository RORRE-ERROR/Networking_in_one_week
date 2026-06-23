# Chapter 13 — Build and Secure a Small Network

> **Day 7 of 7** · Estimated study time: ~4 hours
> *This chapter pulls everything together: how to design a small network sensibly, how to troubleshoot it when it breaks, and — most importantly for the exam — how attackers try to get in and how you keep them out.*

## 🎯 What you'll be able to do after this chapter
- Choose the right devices and design choices for a small network (cost, ports, expandability, redundancy).
- Explain why real-time traffic (Voice/Video) needs **QoS** and priority handling.
- Use the core troubleshooting commands — `ping`, `traceroute`, `show cdp neighbors`, `show ip interface brief` — and read their output.
- Describe the major **security threats**: malware (virus/worm/Trojan), reconnaissance, access, and DoS attacks, plus social engineering.
- Name the common **network attacks** (DoS/DDoS, spoofing, MITM, password attacks) and how each works.
- Apply **defense-in-depth** and harden a Cisco device: strong passwords, encrypted passwords, SSH, timeouts, and login local.

---

## 1. Designing a small network

A small network is simple by nature: a handful of devices, usually one site, one WAN link, and one local IT person looking after it. A typical small network has:

- a **router** (the way out to the internet / WAN),
- a **switch** (connects the wired devices),
- a **wireless access point (AP)** for Wi-Fi users,
- and endpoints like an IP phone, a printer, and a server.

The WAN connection is usually a single link — **DSL, cable, or Ethernet** from an ISP.

### Factors to consider when buying devices
Think of buying network gear like buying a car: price is only part of the story — you also care about what it can carry, whether it can be upgraded, and how reliable it is.

| Factor | What it means in plain terms |
|---|---|
| **Cost** | Driven by port count/type, backplane (switching) speed, management features, built-in security, and the cost of cable runs and any redundancy. |
| **Speed & type of ports** | Pick speeds (e.g. Gigabit) and interface types you can grow into, so you don't have to swap the core device later. |
| **Expandability** | **Fixed** (ports are set) vs **modular** (you add interface cards/modules). Modular lets the network grow. |
| **OS features & services** | Does the IOS support the features you need (QoS, security, routing protocols)? |

> 💡 **Exam tip:** "Choosing Layer 2 devices that can accommodate increased speeds" lets a network **evolve without replacing the central device**. That phrasing shows up in scaling questions.

### IP addressing scheme
Plan, document, and maintain an **IP addressing scheme based on device type**. If routers, servers, printers, and PCs each sit in predictable ranges, you can identify a device by its address and troubleshoot far faster.

### Reliability and redundancy
Even small businesses lean heavily on their network — downtime costs money. **Redundancy** removes single points of failure. You get it by:
- installing **duplicate equipment**, and/or
- providing **duplicate links** for critical paths.

> 💡 **Exam tip:** Memorize this definition almost word-for-word — *"designing a network to use multiple paths between switches to ensure there is no single point of failure."* That's the textbook answer for "what is redundancy?"

---

## 2. Small network applications, protocols & scaling

### Remote access: Telnet vs SSH
Network admins need to reach devices and servers remotely. The two options:

- **Telnet** — sends everything, including your password, in **plaintext**. ❌ Insecure.
- **SSH (Secure Shell)** — sets up an **encrypted** remote session. ✅ Always prefer SSH.

Both ends must support it: the network device acts as the **SSH server**, and the admin's machine is the **SSH client**.

### Real-time traffic and QoS
Businesses increasingly run **IP telephony (Voice)** and **streaming/Video** over the same network as ordinary data. The problem: voice and video are sensitive to **delay and jitter** — a late email is fine, a late voice packet sounds like a glitch.

The fix is **QoS (Quality of Service)**: configure devices to give **priority delivery** to real-time traffic so voice/video go first and bulk traffic (file transfers, etc.) waits.

> 💡 **Exam tip:** When a question says "give high priority to real-time applications," the two traffic types are **Voice and Video** — *not* email, FTP, SNMP, or instant messaging.

### Scaling a small network
To grow a network sensibly you need four things documented:

1. **Network documentation** — physical and logical topology.
2. **Device inventory** — list of all devices on/comprising the network.
3. **Budget** — itemized IT budget, including the equipment-purchasing budget.
4. **Traffic analysis** — protocols, apps, and services and their traffic requirements.

To understand traffic flow, use a **protocol analyzer** to capture traffic:
- **during peak utilization** (so you see the worst case), and
- **on different segments/devices** (some traffic stays local to a segment).

Take snapshots over time so you can spot evolving protocol needs and reallocate resources.

> 💡 **Exam tip:** The purpose of running a **protocol analyzer** before an upgrade is *"to document and analyze network traffic requirements on each network segment"* — not to establish a security baseline.

---

## 3. Troubleshooting

Troubleshooting is a method, not guessing: **analyze the problem → determine the cause → implement a plan → resolve → verify**. Gather symptoms first, then work the layers.

### Verifying connectivity

**`ping`** — the quickest connectivity test. It uses **ICMP** and verifies **Layer 3** reachability.

Read the IOS ping indicators:

| Output | Meaning |
|---|---|
| `!` | Success (reply received). |
| `.` | Timeout waiting for a reply. |
| `U` | A router returned **destination unreachable** (no route). |

**`traceroute` / `tracert`** — shows **each hop** along the path. Great for finding **routing loops** and the exact next-hop router.
- ICMP **"time exceeded"** → a router saw the packet and discarded it (TTL/Hop Limit hit 0).
- ICMP **"destination unreachable"** → a router got the packet but couldn't deliver it.

> 💡 **Exam tip:** *Tracert shows each hop; ping shows only the destination reply.* Begin troubleshooting at the **last hop that responded** before timeouts begin.

### Establishing a baseline
A **network baseline** is your "normal." Measure performance at varying times and loads over a period so you know what healthy looks like. Later, a deviation flags a problem.

> 💡 **Exam tip:** A possible **latency** issue shows up as *"an increase in host-to-host ping response times"* compared to the baseline.

### Key `show` and host commands

```
show running-config              ! current live configuration
show ip interface brief          ! quick up/down + IP per interface
show interfaces                  ! detailed interface stats/errors
show ip route                    ! routing table — which interface reaches a network
show protocols                   ! L3 protocol status per interface
show arp                         ! ARP cache on the device
show version                     ! IOS version, uptime, hardware/RAM
show cdp neighbors [detail]      ! directly connected Cisco neighbors
```

On hosts:
```
ipconfig  /  ipconfig /all       ! Windows IP info
ifconfig  (Linux)  /  ifconfig en0 (mac)
arp -a                           ! list devices in the ARP cache
```

> 💡 **Exam tip:** To find *which interface a router uses to reach a remote network* → **`show ip route`**. To verify *IPv6 routing is enabled* → **`show running-config`**.

### CDP — Cisco Discovery Protocol
**CDP** is a Cisco-proprietary **Layer 2** protocol. Devices send periodic CDP advertisements sharing device type, name, and the number/type of interfaces. It's a discovery and troubleshooting tool.

`show cdp neighbors` gives: **Device ID, Port ID (local + remote), Device type (switch/router), Platform**. Add `detail` to also get the neighbor's **IP address and IOS version**.

> 💡 **Exam tip:** If a `ping` between two **directly connected** routers fails, run `show cdp neighbors` to verify **Layer 2 connectivity** — CDP works at L2 even when L3/IP is misconfigured. To get a neighbor's IP and IOS version, use `show cdp neighbors detail`.

### debug and viewing logs remotely
- `debug` commands run in **privileged EXEC** mode and can flood output (`debug all`, `debug ip packet`). Turn it off with **`no debug`**, `undebug`, or `undebug all`.
- Log messages are **not shown on VTY (Telnet/SSH) sessions by default**. To see them remotely:

```
terminal monitor        ! show log messages on this remote session
terminal no monitor     ! stop showing them
```

> 💡 **Exam tip:** The command that lets log/debug messages appear on a remote VTY (SSH/Telnet) session is **`terminal monitor`**.

---

## 4. Security threats

### What attackers are after
- **Information theft** — break in to steal confidential data (can be sold).
- **Data loss / manipulation** — destroy or alter records.
- **Identity theft** — a form of information theft; steal personal info to impersonate someone.
- **Disruption of service** — stop legitimate users from accessing services they're entitled to.

### Vulnerabilities
A **vulnerability** is the degree of weakness in a network or device. Three primary categories:
1. **Technological** (weaknesses in protocols, OS, hardware),
2. **Configuration** (misconfigured devices, weak settings),
3. **Security policy** (no/weak policy, poor enforcement).

### Physical threats (four classes)
| Class | Examples |
|---|---|
| **Hardware** | Physical damage to servers, routers, switches, cabling, workstations. |
| **Environmental** | Temperature extremes (too hot/cold), humidity extremes (too wet/dry). |
| **Electrical** | Voltage spikes, brownouts, electrical noise, total power loss. |
| **Maintenance** | ESD from poor handling, missing spare parts, poor cabling/labeling. |

### Malware (virus / worm / Trojan)
**Malware** = malicious software designed to damage, disrupt, steal, or perform illegitimate actions. The three classic types:

| Type | How it spreads | Needs a host program? | Needs user action? | One-line description |
|---|---|---|---|---|
| **Virus** | Inserts a copy of itself **into another program**; spreads as that program is shared/run. | ✅ Yes (attaches to a host file/program) | Usually yes (run the infected program) | Malware that becomes *part of* another program. |
| **Worm** | **Self-replicating, standalone**; copies itself across the network automatically. | ❌ No | ❌ No (spreads on its own) | Standalone code that replicates by itself with no user help. |
| **Trojan horse** | **Looks legitimate**; tricks the user into running it (email attachment, downloaded file). | Disguised as a normal file | ✅ Yes (user must open/run it) | Harmful software disguised as something useful; can open **back doors**. |

> 💡 **Exam tip:** The single biggest distinction the exam tests: a **worm spreads by itself** (no host, no user), a **virus attaches to another program**, and a **Trojan needs the user to run it** (user interaction).

### Attack categories
- **Reconnaissance** — discover and map systems/services/vulnerabilities. Tools: `nslookup`, `whois` (find IPs), then ping sweeps with `fping`/`gping`.
- **Access** — exploit known weaknesses in authentication, FTP, or web services to gain unauthorized entry. Four sub-types: **password attacks, trust exploitation, port redirection, man-in-the-middle**.
- **DoS** — consume resources so legitimate users can't get service (see §5).

### Social engineering
Attacking the **human**, not the machine — tricking people into giving up credentials or access (phishing, pretexting, tailgating). Mitigated by **policy + employee awareness**, not by a firewall.

---

## 5. Network attacks

### Access-attack details
| Attack | How it works |
|---|---|
| **Password attack** | Guess/crack credentials via **brute-force**, **Trojan horse**, or **packet sniffers** capturing plaintext passwords. |
| **Trust exploitation** | Use unauthorized privileges (an already-trusted relationship) to reach a system. |
| **Port redirection** | Use a **compromised host as a base** to attack other targets through it. |
| **Man-in-the-middle (MITM)** | Attacker sits **between two legitimate parties** to read or modify the data passing between them. |
| **Spoofing** | Forge a source identity (e.g. IP/MAC address) to impersonate a trusted device. |

### DoS and DDoS
- **DoS (Denial of Service)** — the most publicized and one of the hardest to eliminate. It floods/consumes system resources so authorized people can't use a service. Usually from a **single source**.
- **DDoS (Distributed DoS)** — same idea, but from **many coordinated sources**. The attacker infects many hosts (**zombies**); a network of zombies is a **botnet**, driven by a **command-and-control (CnC)** program to launch the attack.

> 💡 **Exam tip:** Remember the chain: infected host = **zombie**, network of zombies = **botnet**, controlled by **CnC**. DDoS = **multiple coordinated sources**; DoS = single source.

---

## 6. Network attack mitigations

### Defense-in-depth
There's no single magic control — you layer them, so if one fails another still protects you. Start by **securing the devices** themselves (routers, switches, servers, hosts), then add network-edge protections.

| Mitigation | What it does |
|---|---|
| **Backups** | Store copies of config + data on removable media kept safely. One of the most effective protections against **data loss**. |
| **Patching / updates** | Keep systems current with vendor security updates — the **most effective way to stop worms**. |
| **AAA** | **Authentication, Authorization, Accounting** — the primary framework for access control on network devices. *Who are you, what can you do, what did you do.* |
| **VPN** | Secure encrypted tunnels for site-to-site and remote users. |
| **ASA Firewall** | **Stateful** firewall: lets internal traffic out and back, blocks externally-initiated connections to inside hosts. |
| **IPS** | Intrusion Prevention System — watches traffic for malware/attack signatures and can stop a recognized threat immediately. |
| **ESA / WSA** | Email Security Appliance filters spam/suspicious email; Web Security Appliance filters malicious sites. |
| **AAA server** | Secure database of who may access/manage network devices. |
| **Endpoint security** | Antivirus + host IPS + network access control on the end devices (hardest because it involves human behavior). |

### Firewall types
| Firewall type | Filters based on |
|---|---|
| **Packet filtering** | Source/destination **IP or MAC** addresses. |
| **Application filtering** | Specific application types, by **port number**. |
| **URL filtering** | Specific **URLs or keywords**. |
| **Stateful packet inspection (SPI)** | Only allows incoming packets that are **legitimate responses** to internal requests; blocks unsolicited packets and can detect DoS. |

### Device hardening checklist (IOS)
This is high-yield exam material — know each command and *why* it's there.

✅ **1. Set a strong enable (privileged) password — use `secret`, not `password`:**
```
enable secret <StrongP@ss>      ! stored as a strong hash, not plaintext
```

✅ **2. Set strong passwords on console and VTY lines.** A good password is long and mixes letters/numbers/symbols — never a default like `admin`, `cisco`, or `password`.

✅ **3. Encrypt all plaintext passwords in the config:**
```
service password-encryption     ! hides remaining plaintext passwords in show run
```

✅ **4. Configure SSH for secure remote management (full sequence):**
```
hostname R1                          ! 1. set a hostname (not "Router")
ip domain-name example.com           ! 2. set a domain name (needed for the RSA key)
username admin secret <StrongP@ss>   ! 3. create a local user (login local needs this)
crypto key generate rsa              ! 4. generate the RSA keys  (1024+ bits)
ip ssh version 2                     ! 5. force SSH v2

line vty 0 4
 login local                         ! 6. authenticate against the local user database
 transport input ssh                 ! 7. allow SSH only — disable Telnet
```
- **Required for SSH:** a **hostname**, an **IP domain name**, a **local username/password**, and **generated RSA keys**.
- `transport input ssh` is the command that **disables Telnet** and permits SSH only.

✅ **5. Set an idle timeout on console and VTY so unattended sessions close:**
```
line console 0
 exec-timeout 5 0                ! log out after 5 min idle
line vty 0 4
 exec-timeout 5 0
```

✅ **6. Disable unused services and shut unused ports/interfaces:**
```
no ip http server                ! turn off services you don't use
interface range gi0/2 - 24
 shutdown                        ! disable unused switch ports
```

✅ **7. Display a legal warning banner (deters and supports prosecution):**
```
banner motd #Authorized access only. Violators will be prosecuted.#
```

> 💡 **Exam tip:** The four things questions love about SSH config: **(1)** create a local username/password, **(2)** set the **IP domain name**, **(3)** **generate the RSA keys**, **(4)** `transport input ssh`. `exec-timeout` improves security on **console ports and VTY ports**. `enable secret` beats `enable password` because it's hashed.

### Troubleshooting & verification command reference
| Goal | Command |
|---|---|
| Test L3 connectivity | `ping <ip>` (extended ping to set source) |
| Trace the path / find where loss occurs | `traceroute` (IOS) / `tracert` (Windows) |
| Verify L2 connectivity to a neighbor | `show cdp neighbors` |
| Get neighbor IP + IOS version | `show cdp neighbors detail` |
| Quick interface status + IPs | `show ip interface brief` |
| Find interface used to reach a network | `show ip route` |
| Verify IPv6 routing enabled | `show running-config` |
| Show log messages on a remote (VTY) session | `terminal monitor` |
| Stop debugging | `no debug` / `undebug all` |
| Host IP details | `ipconfig /all` (Windows), `ifconfig` (Linux) |

---

## 🧪 Hands-on (Packet Tracer)
- **SRWE 1.6.1 Implement a Small Network** — build a working small network end-to-end (router/switch/hosts, addressing, basic config). The design half of this chapter.
- **SRWE 5.1.9 Investigate STP Loop Prevention** — redundancy in action: see how Spanning Tree stops a loop when you add a backup link.
- **ITN 17.8.2 Skills Integration Challenge** — integrate addressing, configuration, and verification — a full small-network build.
- **ITN 17.8.3 Troubleshooting Challenge** — practice the methodology and the `ping`/`traceroute`/`show` commands to find and fix faults.

## 🧠 Key terms to memorize
| Term | Meaning in one line |
|---|---|
| **Redundancy** | Multiple paths/devices so there's no single point of failure. |
| **QoS** | Prioritizes delay-sensitive traffic (Voice/Video). |
| **Baseline** | The network's normal performance, measured over time, to compare against. |
| **CDP** | Cisco L2 protocol that discovers directly connected Cisco neighbors. |
| **SSH** | Encrypted remote CLI access (replaces insecure Telnet). |
| **Virus** | Malware that attaches itself to another program. |
| **Worm** | Standalone malware that self-replicates with no user action. |
| **Trojan horse** | Malware disguised as legitimate; needs the user to run it. |
| **DoS / DDoS** | Resource exhaustion attack; DDoS comes from many coordinated sources. |
| **Botnet / zombie / CnC** | Network of infected hosts (zombies) run by a command-and-control system. |
| **MITM** | Attacker positioned between two parties to read/alter their traffic. |
| **AAA** | Authentication, Authorization, Accounting — access-control framework. |
| **Defense-in-depth** | Layering security controls so one failure isn't fatal. |
| **SPI** | Stateful inspection — only allow replies to internal requests. |

## ⚡ Exam tips & traps (recap)
- **Redundancy** = multiple paths, no single point of failure. Memorize the phrasing.
- Real-time priority traffic = **Voice and Video** (QoS). Not email/FTP/SNMP/IM.
- **`show ip route`** → which interface reaches a remote network; **`show cdp neighbors detail`** → neighbor IP + IOS version; **`show cdp neighbors`** verifies **L2** when a ping fails.
- **`terminal monitor`** shows log/debug messages on a remote VTY session.
- Ping indicators: `!` = success, `.` = timeout, `U` = unreachable. Start troubleshooting at the **last responsive hop**.
- **Worm** = self-replicating, no host/user; **virus** = attaches to a program; **Trojan** = needs user to run it. Best worm mitigation = **patch/update systems**.
- **DDoS** = multiple coordinated sources; zombie → botnet → CnC.
- SSH needs: hostname + **domain name** + **local username/password** + **RSA keys**; `transport input ssh` disables Telnet.
- `exec-timeout` secures **console and VTY** ports. `enable secret` (hashed) > `enable password`. `service password-encryption` hides remaining plaintext passwords.
- A default password like **`admin`** is weak *because it's a common default* — not because of its length.

## ✅ Quick self-check
1. A ping between two directly connected routers fails. Which command verifies Layer 2 connectivity, and why does it still work?
2. List the four configuration items required before SSH will function on a router.
3. In one sentence each, distinguish a virus, a worm, and a Trojan horse.
4. What is the single most effective way to mitigate a worm attack?
5. Which two traffic types should be given high priority via QoS in a small business upgrade?

---

🎉 **You've finished — that's all 7 days and all 13 chapters!** Take a breath: you've gone from cabling and the OSI model all the way to securing a real network. Now consolidate everything in the **Review Before Exam** file before you sit the exam. Good luck — you've got this.
