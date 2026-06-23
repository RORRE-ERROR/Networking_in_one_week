# Chapter 13 Quiz — Build and Secure a Small Network

> Exam-style questions. Answers are marked and explained. Cover the explanations and test yourself first.

## Question 1: Defining redundancy
What is an accurate description of redundancy?

- [ ] configuring a switch with proper security to ensure that all traffic forwarded through an interface is filtered
- [ ] designing a network to use multiple virtual devices to ensure that all traffic uses the best path through the internetwork
- [ ] configuring a router with a complete MAC address database to ensure that all frames can be forwarded to the correct destination
- [x] designing a network to use multiple paths between switches to ensure there is no single point of failure

### Explanation
* **Key point:** Redundancy means there is more than one path/device, so a single failure does not take the network down.
* **Why not the others:** The other choices describe filtering, path selection, and frame forwarding — none of which is about eliminating single points of failure.

---

## Question 2: Real-time traffic priority
A network administrator is upgrading a small business network to give high priority to real-time applications traffic. What two types of network services is the administrator trying to accommodate? (Choose two.)

- [x] Voice
- [ ] Instant messaging
- [ ] SNMP
- [x] Video
- [ ] FTP

### Explanation
* **Key point:** Voice and Video are delay/jitter sensitive, so QoS gives them priority delivery.
* **Why not the others:** IM, SNMP, and FTP tolerate delay and are not real-time.

---

## Question 3: Choosing a remote-access protocol
A technician must document the configurations of all network devices in a college, including off-site buildings. Which protocol is best to securely access the devices?

- [ ] FTP
- [x] SSH
- [ ] HTTP
- [ ] Telnet

### Explanation
* **Key point:** SSH encrypts the entire session, including credentials, for secure remote management.
* **Why not the others:** Telnet, FTP, and HTTP send data (and passwords) in plaintext.

---

## Question 4: Completing the SSH config
Which command must be configured on the router to complete an SSH configuration?

- [x] transport input ssh
- [ ] service password-encryption
- [ ] ip domain-name cisco.com
- [ ] enable secret class

### Explanation
* **Key point:** `transport input ssh` on the VTY lines permits SSH (and disables Telnet) — it's the line that actually enables SSH as the access method.
* **Why not the others:** `service password-encryption` and `enable secret` harden the device but aren't SSH-specific; `ip domain-name` is one prerequisite but the question asks for the command that *completes* SSH on the lines.

---

## Question 5: Remaining SSH steps
A network engineer has issued the `login local` and `transport input ssh` line vty commands. What three additional actions complete the SSH configuration? (Choose three.)

- [ ] Set the user privilege levels.
- [x] Create a valid local username and password database.
- [ ] Manually enable SSH after the RSA keys are generated.
- [x] Configure the correct IP domain name.
- [ ] Configure role-based CLI access.
- [x] Generate the asymmetric RSA keys.

### Explanation
* **Key point:** SSH needs a local user (for `login local`), an IP domain name, and generated RSA keys. SSH enables automatically once the keys exist.
* **Why not the others:** Privilege levels and role-based CLI are optional access controls, not required for SSH to work.

---

## Question 6: Showing logs on a remote session
Which command allows log messages to be displayed on remotely connected sessions using Telnet or SSH?

- [x] terminal monitor
- [ ] show running-config
- [ ] logging synchronous
- [ ] debug all

### Explanation
* **Key point:** Log/debug messages are not shown on VTY lines by default; `terminal monitor` enables them for that session.
* **Why not the others:** `logging synchronous` only stops messages interrupting your typing; `debug all` generates messages but doesn't route them to VTY; `show running-config` is unrelated.

---

## Question 7: Verifying Layer 2 after a failed ping
A ping fails from router R1 to a directly connected router R2. The administrator then issues `show cdp neighbors`. Why issue this command after the ping failed?

- [ ] To determine if connectivity can be established from a non-directly connected network.
- [ ] To verify the IP address configured on router R2.
- [x] To verify Layer 2 connectivity.
- [ ] The administrator suspects a virus because the ping did not work.

### Explanation
* **Key point:** CDP is a Layer 2 protocol, so it can confirm the physical/data-link connection is up even when Layer 3 (IP) is misconfigured.
* **Why not the others:** CDP doesn't test remote networks or directly reveal R2's configured IP unless you use `detail`, and a failed ping is not evidence of a virus.

---

## Question 8: Finding neighbor IP and IOS version
A network engineer is troubleshooting interconnected Cisco routers and switches. Which command finds the IP address, host name, and IOS version of neighboring devices?

- [x] show cdp neighbors detail
- [ ] show interfaces
- [ ] show ip route
- [ ] show version

### Explanation
* **Key point:** `show cdp neighbors detail` adds the neighbor's IP address and IOS version to the basic neighbor list.
* **Why not the others:** `show version` shows the *local* device's IOS; `show interfaces` and `show ip route` don't report neighbor identity.

---

## Question 9: Reaching remote networks
Which command lets an administrator determine which interface a router will use to reach remote networks?

- [x] show ip route
- [ ] show protocols
- [ ] show arp
- [ ] show interfaces

### Explanation
* **Key point:** The routing table maps destination networks to the outgoing interface / next hop.
* **Why not the others:** `show protocols` shows L3 status, `show arp` shows MAC-to-IP mappings, `show interfaces` shows interface stats — none gives the path to a remote network.

---

## Question 10: Ping vs tracert
Which statement correctly describes the `ping` and `tracert` commands?

- [ ] Both ping and tracert can show results in a graphical display.
- [ ] Ping shows whether the transmission is successful; tracert does not.
- [x] Tracert shows each hop, while ping shows a destination reply only.
- [ ] Tracert uses IP addresses; ping does not.

### Explanation
* **Key point:** Traceroute reports every hop along the path; ping only confirms the final destination replied.
* **Why not the others:** Both use IP/ICMP, both are text-based, and both report success.

---

## Question 11: IOS ping indicators
Which statement is true about Cisco IOS ping indicators?

- [ ] A combination of '.' and '!' indicates a router along the path had no route and responded with an ICMP unreachable.
- [ ] '.' indicates the ping was successful but the response time was longer than normal.
- [x] 'U' may indicate that a router along the path did not contain a route to the destination and the ping was unsuccessful.
- [ ] '!' indicates the ping was unsuccessful and the device may have DNS issues.

### Explanation
* **Key point:** `U` = an ICMP destination-unreachable was returned (no route). `!` = success, `.` = timeout.
* **Why not the others:** The other statements swap or invent meanings for the indicators.

---

## Question 12: Why use tracert
Why would a network administrator use the tracert utility?

- [ ] to check information about a DNS name in the DNS server
- [x] to identify where a packet was lost or delayed on a network
- [ ] to determine the active TCP connections on a PC
- [ ] to display the IP address, default gateway, and DNS server for a PC

### Explanation
* **Key point:** Traceroute reveals the hop where loss or delay begins along the path.
* **Why not the others:** Those describe `nslookup`, `netstat`, and `ipconfig` respectively.

---

## Question 13: Latency from a baseline
A network engineer is analyzing reports from a recent network baseline. Which situation would depict a possible latency issue?

- [ ] a next-hop timeout from a traceroute
- [ ] a change in the bandwidth according to the show interfaces output
- [x] an increase in host-to-host ping response times
- [ ] a change in the amount of RAM according to the show version output

### Explanation
* **Key point:** Higher ping (round-trip) times versus the baseline indicate growing latency.
* **Why not the others:** A traceroute timeout suggests reachability, not latency; RAM/bandwidth readings aren't latency measures.

---

## Question 14: Protocol analyzer purpose
What is the purpose of a small company using a protocol analyzer to capture traffic on segments being considered for an upgrade?

- [x] to document and analyze network traffic requirements on each network segment
- [ ] to identify the source and destination of local network traffic
- [ ] to establish a baseline for security analysis after the network is upgraded
- [ ] to capture the Internet connection bandwidth requirement

### Explanation
* **Key point:** Capturing traffic per segment (especially at peak) documents the traffic requirements that should drive the upgrade.
* **Why not the others:** It's a planning/capacity exercise, not specifically a security baseline or a single bandwidth measurement.

---

## Question 15: Most effective worm mitigation
What is considered the most effective way to mitigate a worm attack?

- [ ] Ensure that AAA is configured in the network.
- [ ] Ensure that all systems have the most current virus definitions.
- [x] Download security updates from the OS vendor and patch all vulnerable systems.
- [ ] Change system passwords every 30 days.

### Explanation
* **Key point:** Worms exploit known software vulnerabilities, so patching closes the holes they use to self-propagate.
* **Why not the others:** AAA, AV definitions, and password rotation help overall security but don't directly stop a worm exploiting an unpatched flaw.

---

## Question 16: Weak password choice
An administrator uses "admin" as the password on a newly installed router. Which statement applies?

- [ ] It is strong because it contains 10 numbers and special characters.
- [x] It is weak because it is often the default password on new devices.
- [ ] It is strong because it uses a minimum of 10 numbers, letters and special characters.
- [ ] It is strong because it uses a passphrase.

### Explanation
* **Key point:** Common defaults like `admin`, `cisco`, and `password` are the first thing attackers try.
* **Why not the others:** "admin" is short, all lowercase, and dictionary-based — none of the "strong" claims are true.

---

## Question 17: Where exec-timeout helps
On which two interfaces or ports can security be improved by configuring exec-timeouts? (Choose two.)

- [ ] serial interfaces
- [x] console ports
- [x] vty ports
- [ ] loopback interfaces
- [ ] fast Ethernet interfaces

### Explanation
* **Key point:** `exec-timeout` logs out idle *management* sessions, which occur on the console and VTY lines.
* **Why not the others:** Serial, loopback, and Ethernet are data interfaces — you don't open an EXEC session on them.

---

## Question 18: Verifying IPv6 routing
Only employees on IPv6 interfaces cannot reach remote networks. The analyst wants to verify that IPv6 routing has been enabled. What is the best command?

- [ ] copy running-config startup-config
- [x] show running-config
- [ ] show interfaces
- [ ] show ip nat translations
- [ ] show version

### Explanation
* **Key point:** `ipv6 unicast-routing` is a global config line, so `show running-config` confirms whether it's present.
* **Why not the others:** `copy` saves config, `show interfaces`/`show version` don't reveal the routing toggle, and NAT translations are unrelated.

---

## Question 19: Malware types matching
Match each malware type to its correct description.

| Description | Malware type |
| :--- | :--- |
| Standalone software that self-replicates automatically across the network with no user action | **Worm** |
| Malware that inserts a copy of itself into, and becomes part of, another program | **Virus** |
| Harmful software disguised as legitimate that users are tricked into running; can create back doors | **Trojan horse** |

### Explanation
* **Key point:** Worm = self-spreading and standalone; Virus = attaches to a host program; Trojan = relies on user interaction and disguise.
* This three-way distinction is one of the most commonly tested security items.

---

## Question 20: DDoS terminology
A threat actor builds a network of infected hosts and uses a command-and-control program to direct them in a coordinated attack. What term describes the network of infected hosts?

- [ ] a zombie
- [x] a botnet
- [ ] a Trojan
- [ ] a reconnaissance scan

### Explanation
* **Key point:** An individual infected host is a **zombie**; the *network* of zombies is a **botnet**, controlled by a CnC system to launch a DDoS.
* **Why not the others:** "Zombie" is a single host; a Trojan is malware; reconnaissance is a discovery phase, not a host network.

---

## Question 21: Man-in-the-middle
Which description best matches a man-in-the-middle (MITM) attack?

- [ ] The attacker consumes all of a server's resources so legitimate users cannot connect.
- [x] The attacker is positioned between two legitimate parties to read or modify the data passing between them.
- [ ] The attacker tries many password combinations until one works.
- [ ] The attacker uses a compromised host as a base to attack other targets.

### Explanation
* **Key point:** MITM intercepts traffic between two parties, allowing eavesdropping or tampering.
* **Why not the others:** Those describe DoS, a brute-force password attack, and port redirection respectively.

---

## Question 22: Stateful firewall behavior
Which firewall capability ensures that incoming packets are only allowed if they are legitimate responses to requests originated by internal hosts?

- [ ] packet filtering
- [ ] URL filtering
- [x] stateful packet inspection (SPI)
- [ ] application filtering

### Explanation
* **Key point:** SPI tracks connection state, permitting return traffic for internal requests and blocking unsolicited inbound packets.
* **Why not the others:** Packet filtering uses IP/MAC, application filtering uses port numbers, and URL filtering uses website addresses/keywords — none track connection state.
