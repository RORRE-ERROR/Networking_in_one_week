# Chapter 12 — Wireless LAN (WLAN)

> **Day 7 of 7** · Estimated study time: ~3 hours
> *This chapter is about how Wi-Fi actually works — the radio standards, the gear, how a device joins a network, the attacks you should fear, and how to lock it all down with WPA2/WPA3.*

## 🎯 What you'll be able to do after this chapter
- Tell the four wireless network types apart (WPAN, WLAN, WMAN, WWAN) and match each to its scale and standard.
- Compare the 802.11 standards (a/b/g/n/ac/ax) by frequency band and speed.
- Identify WLAN components: wireless NICs, autonomous vs. controller-based APs, WLCs, and antenna types.
- Describe how a client discovers, authenticates, and associates with an AP, and explain CSMA/CA.
- Recognise the main WLAN threats (rogue AP, evil twin, DoS, interception) and the defences.
- Choose the right security (open vs. WEP vs. WPA/WPA2/WPA3, PSK vs. Enterprise/802.1X) and configure a wireless router.

---

## 1. What a WLAN is (and the wireless family)

A **WLAN (Wireless LAN)** is just a LAN where the "cable" is replaced by radio waves. It's what lets your laptop and phone roam around a house, office, or campus without being tethered to an Ethernet jack. Wireless uses the **unlicensed radio spectrum** — anyone can transmit on it, which is exactly why interference and security matter so much.

Wireless networks come in four sizes. Think of them as concentric circles, smallest to largest:

| Type | Scope | Typical range | Standard | Examples |
|---|---|---|---|---|
| **WPAN** (Personal) | Around one person | ~6–9 m | **802.15**, 2.4 GHz | Bluetooth, ZigBee |
| **WLAN** (Local) | Home / office / campus | up to ~100 m | **802.11**, 2.4 / 5 GHz | Wi-Fi |
| **WMAN** (Metropolitan) | A city or district | km-scale | (e.g. 802.16) | Citywide wireless |
| **WWAN** (Wide-area) | National / global | very large | **802.16** (WiMAX), Cellular | WiMAX, 4G/5G, satellite |

- **WiMAX** (IEEE 802.16) is a WWAN technology giving broadband up to ~50 km, using towers like cell towers.
- **Cellular broadband** (4G/5G) carries both voice and data; coverage comes from cell sites on towers.
- **Satellite broadband** uses a dish aimed at a geostationary satellite — expensive, needs clear line of sight, mostly rural.

> 💡 **Exam tip:** Match by scale. **WPAN = 802.15 (Bluetooth)**, **WLAN = 802.11 (Wi-Fi)**, **WWAN = 802.16 (WiMAX) / cellular**. The number after "802." is the giveaway.

---

## 2. The 802.11 standards (a/b/g/n/ac/ax)

The IEEE **802.11** family defines Wi-Fi. The two things the exam cares about most are which **frequency band** each uses and its **maximum speed**. The 2.4 GHz band travels farther and through walls better, but is crowded (microwaves, cordless phones, Bluetooth all share it). The 5 GHz band is faster and cleaner but has shorter range.

| Standard | Year (approx) | Band | Max data rate (theoretical) |
|---|---|---|---|
| **802.11a** | 1999 | 5 GHz | 54 Mbps |
| **802.11b** | 1999 | 2.4 GHz | 11 Mbps |
| **802.11g** | 2003 | 2.4 GHz | 54 Mbps |
| **802.11n** (Wi‑Fi 4) | 2009 | 2.4 **and** 5 GHz | 600 Mbps |
| **802.11ac** (Wi‑Fi 5) | 2013 | 5 GHz | ~1.3 Gbps+ |
| **802.11ax** (Wi‑Fi 6) | 2019 | 2.4 **and** 5 GHz | ~10 Gbps |

- **MIMO** (Multiple Input Multiple Output) uses several antennas (up to 8 Tx/Rx) to boost throughput. It arrived with **802.11n** and continues in ac/ax.
- Modulation matters for trivia: **802.11b uses DSSS**; **802.11a/g/n/ac use OFDM**; **802.11ax uses OFDMA** (a multi-user variant of OFDM). **FHSS** is the old frequency-hopping method.

> 💡 **Exam tip:** Remember which standards are **dual-band**: **n and ax** use *both* 2.4 and 5 GHz. **a and ac are 5 GHz only**; **b and g are 2.4 GHz only**. And the speed order climbs b(11) → a/g(54) → n(600) → ac → ax.

---

## 3. WLAN components

A wireless link needs at least two devices with matching radios: an **end device with a wireless NIC**, and a network device such as a **wireless router** or **access point (AP)**.

### Wireless router (the home all-in-one)
A home **wireless router** is really three devices in one box:
1. An **access point** — provides 802.11 a/b/g/n/ac wireless access.
2. A **switch** — a 4-port full-duplex 10/100/1000 Ethernet switch for wired devices.
3. A **router** — the default gateway out to the internet.

It advertises itself by sending **beacons** containing its **SSID** (the network name). Clients see the SSID, then try to associate and authenticate.

### Access points: autonomous vs. controller-based
| Type | Where config lives | Best for |
|---|---|---|
| **Autonomous AP** | Entirely on the AP itself; each AP configured/managed manually and works independently. A home router is one. | A handful of APs |
| **Controller-based AP** (lightweight AP / **LAP**) | On a central **WLAN controller (WLC)** — APs need no initial config. | Many APs (enterprise) |

- **LAPs** historically used **LWAPP** to talk to the WLC. The modern standard is **CAPWAP** (Control And Provisioning of Wireless Access Points) — it's based on LWAPP but adds **DTLS** encryption, and it tunnels client traffic over **UDP** (IPv4 or IPv6).
- A WLC's ports to the switch can be bundled into a **Link Aggregation Group (LAG)** for redundancy and load balancing.

### Antennas
- **Omnidirectional** — 360° coverage; good for homes, open offices, conference rooms.
- **Directional** — focuses signal one way for stronger reach in that direction (e.g. **Yagi**, **parabolic dish**).

> 💡 **Exam tip:** "No initial configuration needed, managed centrally" = **controller-based / lightweight AP + WLC**. "Configuration resides entirely on the device" = **autonomous AP**. CAPWAP = LWAPP + DTLS security over UDP.

---

## 4. WLAN operation: modes, BSS/ESS, and association

### 802.11 modes
- **Ad hoc mode** — two devices connect peer-to-peer with no AP (Wi-Fi Direct, Bluetooth). Also called an **Independent Basic Service Set (IBSS)**.
- **Infrastructure mode** — clients connect through an AP/wireless router (normal WLAN). The AP connects to the wired **distribution system (DS)**.
- **Tethering / hotspot** — a phone with cellular data shares it as a temporary Wi-Fi hotspot.

### BSS and ESS (the building blocks of infrastructure mode)
- **Basic Service Set (BSS)** — one AP and all the clients associated to it. The AP's **MAC address** identifies the BSS, called the **BSSID**. Move out of the coverage area (BSA) and you lose the connection.
- **Extended Service Set (ESS)** — two or more BSSs joined by a common wired distribution system. Clients can **roam** seamlessly between APs within the same ESS.
- Each **ESS is identified by an SSID** (the name); each **BSS is identified by its BSSID** (the AP's MAC).

> 💡 **Exam tip:** **SSID = the network name (an ESS).** **BSSID = the AP's MAC address (one BSS).** Same SSID can be shared by many APs so you roam without re-typing the password.

### How a client joins: the association process
Three stages, in order:
1. **Discover** the AP (passive or active scanning — see below).
2. **Authenticate** with the AP.
3. **Associate** with the AP.

For association to succeed, client and AP must agree on parameters: **SSID**, **password**, **network mode** (which 802.11 standards — APs can run **Mixed mode** to support several at once), **security mode** (WEP/WPA/WPA2/WPA3 — *always pick the highest supported*), and **channel settings**.

**Passive vs. active discovery:**
- **Passive mode** — the AP periodically broadcasts **beacon frames** (carrying SSID, supported standards, security). Clients listen and pick a network.
- **Active mode** — the client broadcasts a **probe request** (with the SSID it wants); APs reply with a **probe response**. Needed when the AP does *not* broadcast beacons (SSID cloaking). A probe request with no SSID discovers any nearby networks.

> 💡 **Exam tip:** **Beacon = AP advertises (passive). Probe request = client asks (active).** If the SSID is hidden/cloaked, the client *must* use active mode and know the name already.

---

## 5. CSMA/CA — and why it's not CSMA/CD

Here's the intuition: on wired Ethernet a device can *listen while it talks* and hear a collision happen — so it uses **CSMA/CD (Collision Detection)**: detect the crash, then back off. On wireless a device **cannot** hear its own transmission collide (the radio can't transmit and receive on the same channel at once — WLANs are **half-duplex**). So Wi-Fi can't *detect* collisions; it must **avoid** them up front. That's **CSMA/CA (Collision Avoidance)**.

| | **CSMA/CD** | **CSMA/CA** |
|---|---|---|
| Used by | Wired Ethernet (legacy hubs) | Wireless 802.11 |
| Strategy | Detect collisions after they happen | Avoid collisions before sending |
| Duplex | Full-duplex possible | **Half-duplex** |
| Uses RTS/CTS? | No | **Yes** |
| Acknowledgements? | No explicit ACK | **Every frame ACKed** |

**CSMA/CA steps (memorise this sequence):**
1. The client **listens** to the channel to confirm it's idle (no other traffic).
2. It sends a **Ready To Send (RTS)** to the AP requesting access.
3. The AP replies with a **Clear To Send (CTS)** granting access.
4. **No CTS?** The client waits a **random** time, then restarts.
5. After CTS, the client **transmits the data**.
6. The receiver **acknowledges (ACK)** every frame. **No ACK = assume a collision** → restart.

> 💡 **Exam tip:** "Why does Wi-Fi use CSMA/CA instead of CSMA/CD?" → because wireless is **half-duplex and a station cannot detect a collision** while transmitting. RTS/CTS + ACKs are the hallmarks of CSMA/CA.

---

## 6. WLAN threats

A WLAN is open to anyone in radio range — an attacker doesn't need to walk into the building. The main threats:

- **Interception of data** — eavesdroppers reading wireless traffic.
- **Wireless intruders** — unauthorised users associating to the network.
- **Denial of Service (DoS)** — from misconfigured devices, malicious jamming, or **accidental interference** (microwaves, cordless phones, baby monitors). The **2.4 GHz band is more interference-prone than 5 GHz**.
- **Rogue AP** — an AP/wireless router plugged into the corporate network *without authorization*. It can capture MACs and data, give network access, or enable a man-in-the-middle attack. A personal hotspot can act as a rogue AP. Defend with **WLC rogue-AP policies** and spectrum monitoring.
- **Evil twin AP** — a rogue AP configured with the **same SSID** as a legitimate one. Common at airports/cafés with open Wi-Fi. Clients near the evil twin see a stronger signal and connect to it; their traffic is captured and forwarded on.

> 💡 **Exam tip:** **Rogue AP** = unauthorized AP on your network. **Evil twin** = rogue AP *impersonating a real SSID* to lure clients. The "same SSID" detail is the evil-twin signature.

### Weak defences (know that they're weak)
- **SSID cloaking** — stop broadcasting the SSID. But SSIDs are easily discovered anyway.
- **MAC address filtering** — permit/deny by MAC. But MACs are easily **spoofed**.

These help a little but are *not* real security. **Authentication + encryption** is the real answer.

---

## 7. WLAN security: open, WEP, WPA, WPA2, WPA3

**Two authentication models:**
- **Open system authentication** — anyone can connect; security (e.g. a **VPN**) is the client's job. Used by public Wi-Fi.
- **Shared key authentication** — a pre-shared secret authenticates and encrypts traffic (WEP/WPA/WPA2/WPA3).

**Two key-management modes** (for WPA/WPA2/WPA3):
- **Personal (PSK)** — home/small office; everyone uses a **pre-shared key** (password).
- **Enterprise (802.1X)** — requires a **RADIUS** server; the user authenticates via the **802.1X** standard using **EAP**. No shared password.

### Security comparison (high-yield)
| | **WEP** | **WPA** | **WPA2** | **WPA3** |
|---|---|---|---|---|
| Status | **Broken — don't use** | Interim fix | Strong (current baseline) | Strongest (newest) |
| Encryption | RC4 | **TKIP** (still RC4-based) | **AES with CCMP** | AES (Personal); **192-bit** suite (Enterprise) |
| Integrity | weak ICV | **MIC** | CCMP | enhanced |
| Personal auth | shared key | PSK | PSK | **SAE** (PSK never exposed) |
| Enterprise | — | 802.1X/EAP | 802.1X/EAP | 802.1X/EAP + CNSA suite |

**Key details:**
- **WEP** is obsolete and easily cracked.
- **WPA** uses **TKIP** (builds on WEP's RC4 but adds per-packet keying and a **Message Integrity Check, MIC**).
- **WPA2** uses **AES** with **CCMP** — the strong, widely deployed standard.
- **WPA3** adds:
  - **WPA3-Personal**: **SAE (Simultaneous Authentication of Equals)** — the PSK is never sent over the air, so it can't be captured and guessed offline.
  - **WPA3-Enterprise**: 802.1X/EAP with a **192-bit** crypto suite (the **CNSA** suite) for high-security networks.
  - **Open networks**: **OWE (Opportunistic Wireless Encryption)** encrypts traffic even without a password.
  - **IoT onboarding**: **DPP (Device Provisioning Protocol)** uses a hardcoded public key, often a **QR code** on the device.

> 💡 **Exam tip:** Encryption pairing to memorise: **WPA → TKIP**, **WPA2 → AES/CCMP**, **WPA3-Personal → SAE**. **Personal = PSK; Enterprise = RADIUS + 802.1X/EAP.** "Always enable the highest security supported" — so WPA3 > WPA2 > WPA > WEP > Open.

---

## 8. Configuring a wireless router / WLC

**Wireless router basic setup (order matters):**
1. Log in to the router from a web browser.
2. **Change the default admin password**.
3. Log in with the new password.
4. Change the default DHCP IPv4 addresses.
5. Renew the IP address.
6. Log in again at the new IP address.

**Basic wireless setup:**
1. Change the **network mode** (which 802.11 standards).
2. Configure the **SSID**.
3. Configure the **channel** (auto-scan, or set manually to dodge interference).
4. Configure the **security mode** (pick the strongest — WPA2/WPA3).
5. Configure the **passphrase**.

**Channels & overlap (2.4 GHz):** The 2.4 GHz band's channels overlap each other. The classic non-overlapping set is **channels 1, 6, and 11**. Put nearby APs on these to avoid interference.

**On a WLC** the basic WLAN workflow is: **Create the WLAN → Apply/Enable it → Select the interface → Secure it → Verify it's operational → Monitor → View client info.** Enterprise setups add SNMP/RADIUS, VLAN interfaces, DHCP scopes, and a WPA2-Enterprise WLAN.

> 💡 **Exam tip:** First thing on any new wireless router = **change the default admin password**. For 2.4 GHz, the non-overlapping channels are **1, 6, 11**.

---

## 🧪 Hands-on (Packet Tracer)
- **ITN 4.6.5 — Connect a Wired and Wireless LAN:** practises building a small network with both wired switch ports and a wireless router/AP — configuring the SSID, channel, and WPA2 security, then connecting wireless clients. This ties together components (§3), association (§4), and configuration (§8).

## 🧠 Key terms to memorize
| Term | Meaning in one line |
|---|---|
| **SSID** | The wireless network name; identifies an ESS. |
| **BSSID** | The AP's MAC address; identifies one BSS. |
| **BSS / ESS** | One AP's cell / two-or-more BSSs joined by a wired DS. |
| **Beacon** | Frame the AP broadcasts to advertise itself (passive discovery). |
| **Probe request** | Frame a client broadcasts to find an AP (active discovery). |
| **CSMA/CA** | Collision *avoidance* — Wi-Fi's half-duplex access method (RTS/CTS/ACK). |
| **Autonomous AP** | Self-contained AP; all config on the device. |
| **LAP / WLC** | Lightweight AP managed centrally by a Wireless LAN Controller. |
| **CAPWAP** | Protocol (LWAPP + DTLS) tunneling AP↔WLC traffic over UDP. |
| **Rogue AP** | Unauthorized AP attached to the network. |
| **Evil twin** | Rogue AP impersonating a legit SSID. |
| **PSK** | Pre-shared key — Personal-mode password auth. |
| **802.1X / EAP / RADIUS** | Enterprise-mode authentication framework + server. |
| **TKIP / AES-CCMP / SAE** | Encryption: WPA / WPA2 / WPA3-Personal. |

## ⚡ Exam tips & traps (recap)
- Wireless types by standard: **WPAN = 802.15**, **WLAN = 802.11**, **WWAN = 802.16/cellular**.
- **Dual-band standards are n and ax.** a/ac = 5 GHz only; b/g = 2.4 GHz only.
- **SSID names the network (ESS); BSSID = AP's MAC (one BSS).**
- Discovery: **beacon = AP advertises (passive); probe = client asks (active).** Hidden SSID forces active mode.
- Association order: **Discover → Authenticate → Associate.**
- Wi-Fi uses **CSMA/CA** (not CD) because it's **half-duplex and can't detect collisions**; look for **RTS/CTS** and **ACK**.
- **Rogue AP = unauthorized; evil twin = rogue with a duplicate SSID.**
- **SSID cloaking and MAC filtering are weak** (SSIDs are discoverable, MACs spoofable).
- Encryption pairs: **WPA→TKIP, WPA2→AES/CCMP, WPA3-Personal→SAE.** Always choose the highest available.
- **Personal = PSK; Enterprise = RADIUS + 802.1X/EAP.**
- **2.4 GHz non-overlapping channels: 1, 6, 11.** 2.4 GHz suffers more interference than 5 GHz.
- First config step on a new wireless router: **change the default admin password.**

## ✅ Quick self-check
1. Which 802.11 standards operate on *both* 2.4 GHz and 5 GHz?
2. Why does Wi-Fi use CSMA/CA instead of CSMA/CD, and what two frame types does it add?
3. What is the difference between a rogue AP and an evil twin?
4. Match each to its encryption: WPA, WPA2, WPA3-Personal.
5. What is the difference between an SSID and a BSSID?
