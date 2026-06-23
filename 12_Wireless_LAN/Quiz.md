# Chapter 12 Quiz — Wireless LAN (WLAN)

> Exam-style questions. Answers are marked and explained. Cover the explanations and test yourself first.

## Question 1: Wireless network types
A company needs short-range, low-power connectivity between a headset and a phone over a distance of a few meters. Which type of wireless network and standard fits this need?

- [ ] WLAN based on the 802.11 standard
- [x] WPAN based on the 802.15 standard
- [ ] WMAN based on the 802.16 standard
- [ ] WWAN using cellular 4G/5G

### Explanation
* **Key point:** A **WPAN** (Wireless Personal-Area Network) covers a very short range (~6–9 m) using low-power transmitters, and is based on the **802.15** standard (Bluetooth, ZigBee).
* **Why not the others:** 802.11 = WLAN (up to ~100 m), 802.16 = WMAN/WiMAX (metropolitan), cellular = WWAN (national/global).

---

## Question 2: WiMAX classification
WiMAX (IEEE 802.16) provides high-speed wireless broadband over distances up to about 50 km using a network of towers. Which category does WiMAX belong to?

- [ ] WPAN
- [ ] WLAN
- [ ] WMAN
- [x] WWAN

### Explanation
* **Key point:** **WiMAX** is an IEEE **802.16 WWAN** standard delivering coverage over an extensive geographic area, similar to cell towers.
* **Why not the others:** WPAN/WLAN are short-to-medium range; WMAN serves a city, but WiMAX as described (national-scale, tower network) is classified as WWAN.

---

## Question 3: 802.11 dual-band standards
Which two 802.11 standards can operate in both the 2.4 GHz and 5 GHz frequency bands? (Choose two.)

- [ ] 802.11a
- [ ] 802.11b
- [x] 802.11n
- [ ] 802.11ac
- [x] 802.11ax

### Explanation
* **Key point:** **802.11n** (Wi-Fi 4) and **802.11ax** (Wi-Fi 6) are dual-band, operating on both 2.4 GHz and 5 GHz.
* **Why not the others:** **802.11a and 802.11ac** are 5 GHz only; **802.11b** is 2.4 GHz only (as is 802.11g).

---

## Question 4: Maximum data rates
Match the 802.11 standard with its approximate maximum theoretical data rate.

| Standard | Max data rate |
| :--- | :--- |
| **802.11b** | **11 Mbps** |
| **802.11g** | **54 Mbps** |
| **802.11n** | **600 Mbps** |
| **802.11ax** | **~10 Gbps** |

### Explanation
* **Key point:** Speeds climb across generations: 802.11b (11 Mbps) → 802.11a/g (54 Mbps) → 802.11n (600 Mbps) → 802.11ac (~1.3 Gbps+) → 802.11ax (~10 Gbps).

---

## Question 5: Wireless router functions
A home user connects devices with a single wireless router. Which three functions does this all-in-one device combine? (Choose three.)

- [x] An access point providing 802.11 wireless access
- [x] A multiport Ethernet switch for wired devices
- [x] A router that acts as the default gateway to the internet
- [ ] A RADIUS authentication server
- [ ] A WLAN controller managing lightweight APs

### Explanation
* **Key point:** A home wireless router bundles an **AP**, a **switch** (4-port full-duplex 10/100/1000), and a **router** (default gateway).
* **Why not the others:** RADIUS servers and WLCs are separate enterprise components, not part of a consumer wireless router.

---

## Question 6: Autonomous vs. controller-based APs
A large enterprise is deploying dozens of access points and wants them centrally managed with no per-device initial configuration. Which solution should they choose?

- [ ] Autonomous APs, each configured independently
- [x] Controller-based lightweight APs managed by a WLC
- [ ] Home wireless routers in mixed mode
- [ ] Standalone APs with directional antennas

### Explanation
* **Key point:** **Controller-based (lightweight) APs** need no initial config and are automatically managed by a **WLAN controller (WLC)** — ideal when many APs are required.
* **Why not the others:** Autonomous APs hold their entire configuration locally and must be managed individually — fine for a couple of APs, not dozens.

---

## Question 7: CAPWAP
Which statement correctly describes CAPWAP?

- [ ] It is a Layer 2 framing method used between wireless clients and an AP.
- [x] It tunnels AP-to-WLC traffic over UDP and adds DTLS encryption to LWAPP.
- [ ] It is the encryption protocol used by WPA2.
- [ ] It replaces 802.1X for enterprise client authentication.

### Explanation
* **Key point:** **CAPWAP** lets a WLC manage multiple APs; it encapsulates/forwards WLAN client traffic between AP and WLC over **UDP** (IPv4 or IPv6), and is based on **LWAPP** plus **DTLS** security.
* **Why not the others:** WPA2 uses AES/CCMP; 802.1X handles client authentication — both are unrelated to CAPWAP's AP↔WLC role.

---

## Question 8: SSID vs. BSSID
Refer to a network with two APs sharing one wireless network name. Which statements about identifiers are correct? (Choose two.)

- [x] The SSID identifies the ESS (the network name clients see).
- [x] The BSSID is the MAC address of an individual AP, identifying one BSS.
- [ ] The BSSID is the network name shared across all APs.
- [ ] Each BSS within an ESS must use a different SSID.
- [ ] The SSID is the Layer 2 MAC address of the AP.

### Explanation
* **Key point:** The **SSID** is the human-readable network name and identifies the **ESS**; the **BSSID** is the **AP's MAC address** and uniquely identifies a single **BSS**.
* **Why not the others:** Multiple BSSs in one ESS *share* the same SSID (that's what enables roaming); the MAC-based identifier is the BSSID, not the SSID.

---

## Question 9: Roaming
Within a single ESS, what allows a mobile client to move from one AP's coverage area to another's without losing connectivity?

- [ ] The APs are configured in ad hoc mode.
- [x] The BSSs are joined by a common distribution system, and the client roams between them using the same SSID.
- [ ] Each AP uses a different SSID so the client picks the strongest.
- [ ] The client switches from CSMA/CA to CSMA/CD.

### Explanation
* **Key point:** An **ESS** is two or more BSSs joined by a wired **distribution system (DS)**, all sharing one SSID — this lets clients **roam** seamlessly between coverage areas.
* **Why not the others:** Ad hoc mode has no APs; differing SSIDs would break seamless roaming; the access method never changes to CSMA/CD on wireless.

---

## Question 10: Association process order
What is the correct order of the steps a wireless client takes to join an AP?

- [ ] Associate → Authenticate → Discover
- [ ] Authenticate → Associate → Discover
- [x] Discover → Authenticate → Associate
- [ ] Discover → Associate → Authenticate

### Explanation
* **Key point:** A client first **discovers** the AP, then **authenticates**, then **associates**.
* **Why not the others:** You cannot authenticate or associate with an AP you have not yet discovered, and authentication precedes association.

---

## Question 11: Passive vs. active discovery
An AP is configured with SSID cloaking and does not broadcast beacon frames. How must a client discover this network?

- [ ] Passively, by listening for beacon frames
- [x] Actively, by broadcasting a probe request that includes the known SSID
- [ ] By sending an RTS frame to all channels
- [ ] By performing a CAPWAP handshake with the WLC

### Explanation
* **Key point:** With no beacons, the client must use **active mode** — it broadcasts a **probe request** containing the SSID it already knows, and the AP replies with a probe response.
* **Why not the others:** Passive mode relies on beacons, which a cloaked AP does not send; RTS and CAPWAP are unrelated to discovery.

---

## Question 12: CSMA/CA rationale
Why do WLANs use CSMA/CA rather than the CSMA/CD method used on legacy wired Ethernet?

- [ ] Wireless links are full-duplex and can detect collisions instantly.
- [x] Wireless is half-duplex, so a station cannot detect a collision while it transmits; it must avoid collisions instead.
- [ ] CSMA/CD is only used on fiber-optic media.
- [ ] CSMA/CA provides faster speeds than CSMA/CD.

### Explanation
* **Key point:** WLANs are **half-duplex** — a station cannot transmit and listen for a collision at the same time, so it cannot *detect* collisions. CSMA/CA **avoids** them up front using RTS/CTS and ACKs.
* **Why not the others:** Wireless is not full-duplex; the choice is about collision detection feasibility, not raw speed or media type.

---

## Question 13: CSMA/CA mechanics
In the CSMA/CA process, what does a wireless client do if it does NOT receive a CTS message after sending an RTS?

- [ ] It immediately transmits the data anyway.
- [ ] It switches to a directional antenna.
- [x] It waits a random amount of time, then restarts the process.
- [ ] It sends the data to the WLC for buffering.

### Explanation
* **Key point:** No **CTS** means access was not granted, so the client **waits a random time** before retrying — this randomness helps avoid repeated collisions.
* **Why not the others:** It does not transmit without a CTS; antenna type and WLC buffering are irrelevant to the access handshake.

---

## Question 14: Acknowledgements
On a WLAN, how does a transmitting client know a collision likely occurred?

- [ ] The AP sends an explicit collision notification frame.
- [x] It does not receive an acknowledgement (ACK) for the transmitted frame.
- [ ] The signal strength on the channel drops to zero.
- [ ] The SSID disappears from the beacon.

### Explanation
* **Key point:** Every WLAN transmission is **acknowledged**. If the client gets **no ACK**, it assumes a collision occurred and restarts the process.
* **Why not the others:** There is no dedicated collision frame in CSMA/CA; absence of an ACK is the signal.

---

## Question 15: Rogue AP vs. evil twin
An attacker sets up a wireless access point in a café broadcasting the same SSID as the café's legitimate Wi-Fi, hoping nearby users connect to it. What type of attack is this?

- [ ] A denial-of-service (DoS) jamming attack
- [ ] A MAC spoofing attack
- [x] An evil twin attack
- [ ] An RTS/CTS flooding attack

### Explanation
* **Key point:** An **evil twin** is a rogue AP configured with the **same SSID** as a legitimate AP, common at open-Wi-Fi venues; clients near it see a stronger signal and associate, letting the attacker capture traffic.
* **Why not the others:** A generic rogue AP need not duplicate an SSID; DoS aims to disrupt service, not impersonate; MAC spoofing changes a hardware address.

---

## Question 16: WLAN DoS sources
Which of the following can cause a wireless denial-of-service condition? (Choose three.)

- [x] Improperly configured wireless devices
- [x] A malicious user deliberately jamming the wireless medium
- [x] Accidental interference from microwave ovens or cordless phones
- [ ] Enabling WPA3 encryption on the AP
- [ ] Assigning non-overlapping channels 1, 6, and 11

### Explanation
* **Key point:** Wireless DoS can result from **misconfiguration**, **deliberate jamming**, and **accidental interference** (microwaves, cordless phones, baby monitors). The 2.4 GHz band is especially interference-prone.
* **Why not the others:** Strong encryption and proper channel planning *improve* reliability, not degrade it.

---

## Question 17: Weak protections
Why are SSID cloaking and MAC address filtering considered weak security measures?

- [ ] They disable encryption on the WLAN.
- [x] Hidden SSIDs are easily discovered and MAC addresses are easily spoofed.
- [ ] They require a RADIUS server that most networks lack.
- [ ] They only work with the 5 GHz band.

### Explanation
* **Key point:** SSIDs can be uncovered even when not broadcast, and MAC addresses can be **spoofed**, so neither stops a determined attacker. Real security comes from **authentication + encryption**.
* **Why not the others:** These measures do not control encryption, do not need RADIUS, and are band-independent.

---

## Question 18: WPA vs. WPA2 encryption
Match each Wi-Fi security standard with its encryption/authentication mechanism.

| Standard | Mechanism |
| :--- | :--- |
| **WPA** | **TKIP (with MIC)** |
| **WPA2** | **AES with CCMP** |
| **WPA3-Personal** | **SAE (PSK never exposed)** |

### Explanation
* **Key point:** **WPA** uses **TKIP** (RC4-based, adds a Message Integrity Check); **WPA2** uses **AES with CCMP**; **WPA3-Personal** uses **SAE (Simultaneous Authentication of Equals)** so the pre-shared key is never sent over the air.

---

## Question 19: Personal vs. Enterprise
A WLAN must authenticate each user individually against a central server using the 802.1X standard. Which mode and component are required? (Choose two.)

- [ ] WPA2-Personal with a shared passphrase
- [x] WPA2/WPA3-Enterprise mode
- [x] A RADIUS authentication server using EAP
- [ ] A pre-shared key (PSK) distributed to all users
- [ ] OWE for open networks

### Explanation
* **Key point:** **Enterprise mode** uses **802.1X** with **EAP**, requiring a **RADIUS** server to authenticate each user/device individually.
* **Why not the others:** Personal mode uses a single shared **PSK** for everyone; OWE is for open (passwordless) networks.

---

## Question 20: WPA3 Personal feature
What advantage does WPA3-Personal's SAE provide over WPA2-Personal?

- [ ] It removes the need for any password.
- [x] The pre-shared key is never exposed over the air, defeating offline password-guessing.
- [ ] It allows the SSID to be hidden automatically.
- [ ] It doubles the maximum data rate of the WLAN.

### Explanation
* **Key point:** **SAE (Simultaneous Authentication of Equals)** ensures the **PSK is never transmitted**, so an attacker cannot capture a handshake and brute-force the password offline.
* **Why not the others:** WPA3-Personal still uses a password; SAE has nothing to do with SSID cloaking or data rates.

---

## Question 21: Best security choice
A technician is configuring a new wireless router and the slides advise to "always enable the highest security level supported." If the router supports Open, WEP, WPA, WPA2, and WPA3, which should be selected?

- [ ] Open with a VPN
- [ ] WEP
- [ ] WPA with TKIP
- [x] WPA3

### Explanation
* **Key point:** **WPA3** is the newest and strongest option; the rule is to always pick the highest supported security.
* **Why not the others:** WEP is broken, WPA/TKIP is an outdated interim fix, and Open provides no built-in protection (VPN aside).

---

## Question 22: Wireless router setup order
What should be the FIRST action after logging in to a brand-new wireless router with its factory defaults?

- [ ] Configure the SSID and channel
- [x] Change the default administrative password
- [ ] Enable MAC address filtering
- [ ] Renew the DHCP IP address

### Explanation
* **Key point:** The basic setup begins with **changing the default admin password** so an attacker cannot log in with well-known factory credentials.
* **Why not the others:** SSID/channel and security settings come after securing administrative access; renewing the IP follows changing the DHCP addresses.

---

## Question 23: 2.4 GHz channel planning
Three APs are deployed in overlapping areas on the 2.4 GHz band. Which channel assignment minimizes interference between them?

- [ ] Channels 1, 2, 3
- [ ] Channels 5, 6, 7
- [x] Channels 1, 6, 11
- [ ] Channels 9, 10, 11

### Explanation
* **Key point:** In the 2.4 GHz band the only set of **non-overlapping** channels is **1, 6, and 11**, so adjacent APs on these channels do not interfere.
* **Why not the others:** The other sets contain overlapping channels, which causes co-channel interference.
