# Chapter 2 — Network Protocols and Network Operating Systems

> **Day 1 of 7** · Estimated study time: ~3 hours
> *This chapter is about the "rules" that let devices talk (protocols + models), how data is wrapped up and addressed to travel across a network (encapsulation), and how you actually log in to a Cisco switch/router and configure it (Cisco IOS).*

## 🎯 What you'll be able to do after this chapter
- Explain the basic rules of communication: encoding, formatting/encapsulation, size, timing, and delivery options (unicast/multicast/broadcast).
- Describe what a protocol and a protocol suite are, and name the standards organizations behind networking (ISO, IEEE, IETF, IANA, ISOC).
- Map the **OSI model** to the **TCP/IP model** layer-by-layer (this is a top exam item).
- Walk through the **encapsulation** process and name the PDU at each layer (data → segment → packet → frame → bits).
- Explain how a device decides whether the destination is on the **same network** or a **different network**, and which address (MAC vs IP) is used at each layer.
- Access a Cisco device (console, SSH, Telnet, AUX) and navigate IOS modes.
- Run the core IOS configuration commands: hostname, passwords, banners, saving config, and an SVI.

---

## 1. Rules of Communication

Think about a normal human conversation. Before two people can swap information, they silently agree on a bunch of rules: which language, who talks first, how loud, how fast, and "did you hear me?" Computers need the exact same agreements. We call these agreed rules **protocols**.

Every communication starts with a **message** that must go from a **source** to a **destination** over a **channel** (the medium, e.g. a cable or Wi-Fi). The protocols used depend on the source, destination, and channel.

**Protocol requirements** — every protocol must define:
- An identified **sender and receiver**.
- A **common language and grammar** (agreed format).
- The **speed and timing** of delivery.
- **Confirmation / acknowledgment** requirements (do we need a "got it"?).

### The five message rules to memorize
| Rule | One-line meaning | Everyday analogy |
| --- | --- | --- |
| **Message encoding** | Converting information into a form suitable for transmission (and **decoding** back). | Turning thoughts into spoken words (signals/bits on the wire). |
| **Message formatting & encapsulation** | Putting the message into an agreed structure with addressing, like an envelope. | A letter inside an addressed envelope. |
| **Message size** | Big messages are broken into smaller pieces the receiver can handle. | Speaking in short sentences, not one endless run-on. |
| **Message timing** | Flow control, response timeout, and access method. | Pausing so the listener keeps up; waiting your turn. |
| **Message delivery options** | Unicast, multicast, or broadcast. | Talking to one person, a group, or shouting to the whole room. |

**Encoding** = converting information into another acceptable form for transmission. **Decoding** reverses it so the receiver can interpret it.

**Message timing** breaks into three sub-ideas:
1. **Flow control** — how *much* data and how *fast* it can be sent (so the receiver isn't overwhelmed).
2. **Response timeout** — how long a host waits for a reply, and what to do if nothing comes back.
3. **Access method** — determines *when* a device is allowed to send (avoids everyone talking at once).

### Delivery options (high-yield)
| Option | Goes to | Example |
| --- | --- | --- |
| **Unicast** | **One** specific host | Loading a single web page from one server. |
| **Multicast** | A **defined group** of hosts | Streaming video to subscribers; routing protocol updates. |
| **Broadcast** | **All** hosts on the local network | ARP request; old-style discovery. |

> 💡 **Exam tip:** Multicast = a *specific group*, not "everyone." Broadcast = literally everyone on the local segment. If a question says "all hosts on the local network," the answer is broadcast; "a specially defined group of hosts" is multicast.

---

## 2. Network Protocols

A single **network protocol** defines a common format and set of rules for exchanging messages between devices. One message sent over a network usually needs *several* protocols working together — each with its own job and format.

A group of inter-related protocols that together perform a communication function is a **protocol suite**. Protocol suites are implemented in software, hardware, or both. The **TCP/IP suite** is the suite of protocols used to send and receive information over the Internet.

We use a **layered model** to visualize how the protocols interact. A layered model shows what happens *within* a layer and how each layer talks to the layers *above and below* it. The two models you must know:

### The TCP/IP model (4 layers — what the Internet actually runs)
| TCP/IP layer | Job | Example protocols |
| --- | --- | --- |
| **Application** | Provides services to the user's apps | HTTP, HTTPS, FTP, DNS, SMTP, DHCP |
| **Transport** | End-to-end delivery, reliability, segmentation | TCP, UDP |
| **Internet** | Logical addressing & routing between networks | IP (IPv4/IPv6), ICMP |
| **Network Access** | Putting frames onto the physical media | Ethernet, ARP, drivers/NIC |

### The OSI model (7 layers — the reference/teaching model)
| # | OSI layer | Job (one line) | PDU |
| --- | --- | --- | --- |
| 7 | **Application** | Interface to user applications | Data |
| 6 | **Presentation** | Format, encrypt, compress | Data |
| 5 | **Session** | Open/manage/close conversations | Data |
| 4 | **Transport** | End-to-end delivery, ports, reliability | Segment |
| 3 | **Network** | Logical addressing & routing (IP) | Packet |
| 2 | **Data Link** | Framing, MAC addressing on the local link | Frame |
| 1 | **Physical** | Bits as signals on the media | Bits |

**Memory aid (Layer 7 → 1):** *All People Seem To Need Data Processing.*

### OSI ↔ TCP/IP mapping (the high-yield diagram)
```
   OSI (7 layers)            TCP/IP (4 layers)
 7  Application      \
 6  Presentation      >----  Application
 5  Session          /
 4  Transport        ------  Transport
 3  Network          ------  Internet
 2  Data Link        \
 1  Physical          >----  Network Access
```
- OSI **Application + Presentation + Session** all collapse into TCP/IP **Application**.
- OSI **Transport** ↔ TCP/IP **Transport** (1:1).
- OSI **Network** ↔ TCP/IP **Internet** (1:1).
- OSI **Data Link + Physical** collapse into TCP/IP **Network Access**.

> 💡 **Exam tip:** The two 1-to-1 mappings (Transport↔Transport, Network↔Internet) are the easiest marks. The traps are the *groupings*: OSI's top three = TCP/IP Application, and OSI's bottom two = TCP/IP Network Access. Note TCP/IP has **no separate Presentation or Session layer** — those functions live inside its Application layer.

### Standards organizations
Standards organizations are usually **vendor-neutral, non-profit** bodies that develop and promote **open standards** (so any vendor's gear can interoperate).

| Org | Stands for | What it does |
| --- | --- | --- |
| **ISO** | International Organization for Standardization | Created the **OSI** reference model. |
| **IEEE** | Institute of Electrical and Electronics Engineers | LAN/WAN standards — e.g. **802.3** (Ethernet), **802.11** (Wi-Fi). |
| **IETF** | Internet Engineering Task Force | Develops Internet standards (TCP/IP) via **RFC** documents. |
| **IANA** | Internet Assigned Numbers Authority | Allocates IP addresses, port numbers, and protocol numbers. |
| **ISOC** | Internet Society | Promotes open Internet development; oversight/umbrella for the standards process. |

> 💡 **Exam tip:** Easy confusions — **ISO** (the org) is *not* the same as **OSI** (the model ISO created). **IEEE = 802.x numbers** (802.3 Ethernet, 802.11 Wi-Fi). **IETF = RFCs**. **IANA = number assignments** (addresses/ports).

---

## 3. Data Encapsulation

Intuition: you don't mail a whole novel as one giant page. You split it into numbered pages, put each in an addressed envelope, and the post office interleaves your envelopes with everyone else's. Networks do the same.

### Why split data up?
Sending one massive, uninterrupted stream of bits would hog the link and cause big delays for everyone else.
- **Segmentation** = dividing data into smaller, manageable pieces. This also *increases reliability* — if one piece is lost, only that piece is re-sent, not the whole thing.
- **Multiplexing** = interleaving the pieces of many separate conversations on the same link, so multiple conversations share the wire.

Each piece must carry enough addressing/sequencing info to reach the right destination and be **reassembled** into the original message at the other end.

### The encapsulation process
As application data is passed *down* the protocol stack, each layer adds its own control information (a header, and sometimes a trailer). This is **encapsulation**. The form the data takes at any given layer is called a **Protocol Data Unit (PDU)**.

| Layer (TCP/IP) | PDU name | What gets added |
| --- | --- | --- |
| Application | **Data** | The raw application data. |
| Transport | **Segment** | TCP/UDP header (ports, sequence numbers). |
| Internet | **Packet** | IP header (source & destination IP). |
| Network Access | **Frame** | Frame header **and trailer** (source & destination MAC, FCS). |
| Physical | **Bits** | Encoded as signals on the media. |

**Memory aid (top → bottom):** *Do Sergeants Pay For Beer?* → **D**ata, **S**egment, **P**acket, **F**rame, **B**its.

Numbered view of encapsulation (sending host):
1. The application produces **data**.
2. The Transport layer adds a header → **segment**.
3. The Internet layer adds the IP header → **packet**.
4. The Network Access layer adds the frame header + trailer → **frame**.
5. The Physical layer transmits it as **bits** (signals).

**De-encapsulation** is the reverse, at the receiving host: as the data moves *up* the stack, each layer removes (strips) one or more headers until the original data reaches the end-user application.

> 💡 **Exam tip:** Lock these in: **Segment = Transport (Layer 4), Packet = Network/Internet (Layer 3), Frame = Data Link / Network Access (Layer 2).** Many questions are just "what is the PDU at layer X?" The frame is the only PDU with a *trailer* as well as a header.

---

## 4. Data Access (Addressing)

The **Network layer** and **Data Link layer** work together to deliver data from source to destination — but they use **different kinds of addresses**:

- **Network layer (Layer 3) addresses = IP addresses.** Responsible for delivering the IP **packet** from the *original source* to the *final destination*. These addresses **do not change** along the path, whether the destination is on the same network or a remote one.
- **Data Link layer (Layer 2) addresses = MAC addresses.** Responsible for delivering the **frame** from one NIC to another NIC **on the same network (same link)**. These addresses **change at every hop**.

Analogy you'll reuse all course: **IP address = the mailing address on the envelope (start to finish). MAC address = the local courier's "next door" hand-off (changes at each leg of the journey).**

### Case A — Same network (local delivery)
When source and destination are on the **same network**, the frame is sent **directly** to the receiving device. The destination MAC address is the **actual destination host's MAC**.
- Destination IP = the real destination host.
- Destination MAC = the real destination host.

### Case B — Different network (remote delivery via the gateway)
When the destination is on a **different network**, the host can't reach it directly. It must hand the frame to its **default gateway (router)**.
- Destination IP = still the **final destination host** (unchanged).
- Destination MAC = the **router's (default gateway's) MAC**, *not* the final host's.

When the packet reaches the router, the router strips the old frame, looks up the next hop, and builds a **new frame** with new source/destination MAC addresses for the next link. The IP addresses stay the same the whole way.

> 💡 **Exam tip:** Classic "HostA sends to ServerB on another network" question. The packet's **destination IP = ServerB** (final target), but the frame's **destination MAC = the router/default gateway**. The IP is end-to-end; the MAC is hop-to-hop. Students lose marks by putting ServerB's MAC — it's the gateway's MAC.

---

## 5. Network Operating Systems (Cisco IOS)

Infrastructure devices (switches, routers) run a **network operating system**, much like a PC runs Windows/macOS/Linux. On Cisco gear this OS is the **Cisco Internetwork Operating System (IOS)**.

How IOS lives in memory:
- The IOS image file is stored in **flash** — non-volatile (semi-permanent) storage, so it survives a reboot.
- On power-up, IOS is **copied from flash into RAM** and **runs from RAM**.
- The **kernel** talks directly to the hardware; the user talks to the **shell** via a **CLI** (command-line interface) or **GUI**. CCNA focuses on the **CLI**.

### Ways to access the CLI
| Method | In-band / Out-of-band | Needs network configured? | Secure? | Notes |
| --- | --- | --- | --- | --- |
| **Console** | Out-of-band | No | n/a (physical) | Dedicated mgmt port; rollover cable; used for *initial* config and recovery. |
| **AUX** | Out-of-band | No | n/a | Older; remote CLI via a **modem/dial-up** on a router's AUX port. |
| **Telnet** | In-band | **Yes** | ❌ No (plaintext) | Remote CLI over the network via a **virtual** interface (VTY). |
| **SSH** | In-band | **Yes** | ✅ Yes (encrypted) | Like Telnet but **encrypted**, with stronger authentication. **Preferred.** |

Common terminal emulation programs: **PuTTY, Tera Term, SecureCRT, HyperTerminal, OS X Terminal.**

> 💡 **Exam tip:** **Console = out-of-band** (doesn't need any network services up — perfect for a brand-new or broken device). **Telnet vs SSH:** both are *in-band* and need networking configured, but **SSH encrypts** and Telnet sends everything (including passwords) in **clear text**. Always pick SSH for remote management.

### IOS command modes (hierarchical)
You move *up* in privilege with `enable` and *down* with `exit`/`end`. Watch the **prompt** — it tells you exactly where you are.

| Mode | Prompt | What you can do | How to enter |
| --- | --- | --- | --- |
| **User EXEC** | `Switch>` | Limited monitoring only | Default after login |
| **Privileged EXEC** | `Switch#` | Full monitoring, save/reload, debug | `enable` |
| **Global config** | `Switch(config)#` | Device-wide config changes | `configure terminal` |
| **Line config** | `Switch(config-line)#` | Configure console/VTY/AUX lines | `line console 0` / `line vty 0 15` |
| **Interface config** | `Switch(config-if)#` | Configure a specific interface/SVI | `interface vlan 1` |

> 💡 **Exam tip:** The **prompt symbol** is the giveaway: `>` = User EXEC, `#` = Privileged EXEC, `(config)#` = global config, `(config-line)#` / `(config-if)#` = sub-config modes. `enable` goes up to privileged EXEC; `configure terminal` goes into global config; `exit` backs up one level; `end` (or Ctrl+Z) jumps straight back to privileged EXEC.

---

## 6. Basic IOS Commands

### Set a hostname
**Hostnames** let admins identify devices on a network.
```
Switch> enable
Switch# configure terminal
Switch(config)# hostname S1
S1(config)#
```

### Secure the device with passwords
**Passwords** are the primary defense against unauthorized access. The four you must know:

| Password type | Protects | Where it's set |
| --- | --- | --- |
| **Enable password** | Privileged EXEC mode | global config (plaintext — legacy) |
| **Enable secret** | Privileged EXEC mode | global config (**encrypted** — preferred) |
| **Console password** | Console (line con 0) access | line config |
| **VTY password** | Telnet/SSH (line vty) access | line config |

```
! Privileged EXEC — use the encrypted "secret" form
S1(config)# enable secret class

! Console line password + force login
S1(config)# line console 0
S1(config-line)# password cisco
S1(config-line)# login
S1(config-line)# exit

! VTY lines (remote Telnet/SSH) password + force login
S1(config)# line vty 0 15
S1(config-line)# password cisco
S1(config-line)# login
S1(config-line)# exit

! Encrypt all plaintext passwords shown in the config
S1(config)# service password-encryption
```

> 💡 **Exam tip:** `enable secret` is **MD5-encrypted** and overrides `enable password` (plaintext). `service password-encryption` weakly encrypts *other* plaintext passwords (like the line passwords) in the config file. Don't confuse the two: `enable secret` protects privileged EXEC; `service password-encryption` just hides passwords from shoulder-surfers in `show running-config`.

### Banner (legal warning shown at login)
```
S1(config)# banner motd #Authorized Access Only!#
```
The `#` is a **delimiting character** — pick any character not used in the message; the banner text sits between the opening and closing delimiter.

### Save the configuration
There are two configs in memory:
- **running-config** — the *live* config in **RAM** (lost on reboot).
- **startup-config** — the *saved* config in **NVRAM** (loaded on boot).

```
S1# copy running-config startup-config
! short form / older alias:
S1# write memory
```

> 💡 **Exam tip:** If a change "disappears" after a reboot, the cause is almost always that **running-config was never copied to startup-config**. Edits take effect immediately in RAM but are *not* persistent until saved to NVRAM.

### Configure a Switch Virtual Interface (SVI) for remote management
A switch is a Layer 2 device, so it doesn't normally have an IP. To **manage it remotely**, you give the management VLAN interface (the **SVI**, e.g. VLAN 1) an IP address.
```
S1(config)# interface vlan 1
S1(config-if)# ip address 192.168.1.2 255.255.255.0
S1(config-if)# no shutdown
S1(config-if)# exit
S1(config)# ip default-gateway 192.168.1.1
```
- `no shutdown` administratively **enables** the interface (interfaces are often down by default).
- `ip default-gateway` lets the switch reach management traffic on *other* networks.

> 💡 **Exam tip:** A directly-connected interface/network won't appear or work if the interface is **administratively down** — the fix is `no shutdown`. This is a recurring "why isn't connectivity working?" answer.

---

## 🧪 Hands-on (Packet Tracer)
- **2.3.7 Navigate the IOS** — practice moving between command modes (`enable`, `configure terminal`, `exit`, `end`) and reading the prompt.
- **2.5.5 Configure Initial Switch Settings** — set hostname, secure passwords, banner, and save the config.
- **2.7.6 Implement Basic Connectivity** — assign the SVI IP, set the default gateway, and verify devices can ping.
- **2.9.1 Basic Switch and End Device Configuration** — full end-to-end: switch settings + PC IP settings, then test connectivity.
- **3.5.5 Investigate the TCP/IP and OSI Models in Action** — use Simulation mode to watch encapsulation and the PDUs (segment/packet/frame) at each layer as traffic flows.

## 🧠 Key terms to memorize
| Term | Meaning in one line |
| --- | --- |
| **Protocol** | Agreed rules and format for exchanging messages. |
| **Protocol suite** | A group of related protocols that work together (e.g. TCP/IP). |
| **Encoding / Decoding** | Converting info to a transmittable form / back again. |
| **Encapsulation** | Adding headers (and a trailer) as data goes *down* the stack. |
| **De-encapsulation** | Stripping headers as data goes *up* the stack at the receiver. |
| **Segmentation** | Splitting data into smaller pieces for reliable transport. |
| **Multiplexing** | Interleaving many conversations on one link. |
| **PDU** | Protocol Data Unit — the form data takes at a given layer. |
| **Unicast / Multicast / Broadcast** | To one host / a defined group / all hosts on the local network. |
| **MAC address** | Layer 2 physical address; hop-to-hop, changes each link. |
| **IP address** | Layer 3 logical address; end-to-end, stays the same. |
| **Default gateway** | The router a host uses to reach other networks. |
| **IOS** | Cisco's network operating system; runs from RAM, stored in flash. |
| **SVI** | Switch Virtual Interface — a VLAN interface that gives a switch a manageable IP. |
| **running-config / startup-config** | Live config in RAM / saved config in NVRAM. |

## ⚡ Exam tips & traps (recap)
- **OSI 7 ↔ TCP/IP 4:** top 3 (App/Pres/Sess) → Application; Transport↔Transport; Network↔Internet; bottom 2 (Data Link/Physical) → Network Access. TCP/IP has *no* Session or Presentation layer.
- **PDUs:** Data, **S**egment (L4), **P**acket (L3), **F**rame (L2), **B**its (L1). Frame is the only one with a trailer.
- **Addressing on a remote send:** destination **IP = final host**, destination **MAC = default gateway**. IP is end-to-end; MAC changes every hop.
- **Delivery options:** multicast = a *group*, broadcast = *everyone* on the local net, unicast = *one*.
- **Standards orgs:** ISO ≠ OSI (ISO *made* OSI); IEEE = 802.x; IETF = RFCs; IANA = address/port numbers.
- **Access methods:** console & AUX = out-of-band; Telnet & SSH = in-band; **SSH encrypts, Telnet is plaintext** — choose SSH.
- **`enable secret`** (encrypted) beats `enable password` (plaintext); `service password-encryption` hides the *other* passwords.
- **Prompts:** `>` user EXEC, `#` privileged EXEC, `(config)#` global, `(config-if)#`/`(config-line)#` sub-modes.
- **Save or lose it:** `copy running-config startup-config` — RAM is volatile; NVRAM persists.
- **`no shutdown`** enables an interface; "directly connected network missing" usually = interface not activated.

## ✅ Quick self-check
1. Which OSI layers map to the single TCP/IP **Application** layer, and which map to **Network Access**?
2. Name the PDU at the Transport, Network, and Data Link layers.
3. HostA (network 10.1.1.0) sends to ServerB (network 10.2.2.0). What destination IP and destination MAC does HostA put on the data?
4. Which CLI access method is out-of-band and works even with no network services configured? Which remote method should you prefer over Telnet, and why?
5. You configured a hostname and password, then the device rebooted and they were gone. What command did you forget?
