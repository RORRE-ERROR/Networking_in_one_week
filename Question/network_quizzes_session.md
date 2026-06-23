# Network Infrastructure Quiz Compilation

## Question 13
Which method is used to send a ping message specifying the source address for the ping?
* Issue the ping command from within interface configuration mode.
* Issue the ping command without extended commands.
* **Issue the ping command without specifying a destination IP address.**
* Issue the ping command after shutting down un-needed interfaces.

## Question 14
Which command should be used on a Cisco router or switch to allow log messages to be displayed on remotely connected sessions using Telnet or SSH?
* **terminal monitor**
* show running-config
* logging synchronous
* debug all

## Question 15
A network administrator is upgrading a small business network to give high priority to real-time applications traffic. What two types of network services is the network administrator trying to accommodate? (Choose two.)
* **Voice**
* Instant messaging
* SNMP
* **Video**
* FTP

## Question 16
Which command can an administrator execute to determine what interface a router will use to reach remote networks?
* **show ip route**
* show protocols
* show arp
* show interfaces

## Question 17
A network engineer is troubleshooting connectivity issues among interconnected Cisco routers and switches. Which command should the engineer use to find the IP address information, host name, and IOS version of neighboring network devices?
* **show cdp neighbors detail**
* show interfaces
* show ip route
* show version

## Question 18
A network technician issues the C:\> tracert -6 www.cisco.com command on a Windows PC. What is the purpose of the -6 command option?
* **It forces the trace to use IPv6.**
* It sets a 6 milliseconds timeout for each replay.
* It limits the trace to only 6 hops.
* It sends 6 probes within each TTL time period.

## Question 19
Which network service automatically assigns IP addresses to devices on the network?
* Traceroute
* **DHCP**
* Telnet
* DNS

## Question 20
Which statement is true about Cisco IOS ping indicators?
* A combination of '.' and '!' indicates that a router along the path did not have a route to the destination address and responded with an ICMP unreachable message.
* '.' indicates that the ping was successful but the response time was longer than normal.
* **'U' may indicate that a router along the path did not contain a route to the destination address and that the ping was unsuccessful.**
* '!' indicates that the ping was unsuccessful and that the device may have issues finding a DNS server.

## Question 21
What is an accurate description of redundancy?
* configuring a switch with proper security to ensure that all traffic forwarded through an interface is filtered
* designing a network to use multiple virtual devices to ensure that all traffic uses the best path through the internetwork
* configuring a router with a complete MAC address database to ensure that all frames can be forwarded to the correct destination
* **designing a network to use multiple paths between switches to ensure there is no single point of failure**

## Question 22
What is the purpose of a small company using a protocol analyzer utility to capture network traffic on the network segments where the company is considering a network upgrade?
* **to document and analyze network traffic requirements on each network segment**
* to identify the source and destination of local network traffic
* to establish a baseline for security analysis after the network is upgraded
* to capture the Internet connection bandwidth requirement

## Question 23
Refer to the exhibit. Host H3 is having trouble communicating with host H1. The network administrator suspects a problem exists with the H3 workstation and wants to prove that there is no problem with the R2 configuration. What tool could the network administrator use on router R2 to prove that communication exists to host H1 from the interface on R2, which is the interface that H3 uses when communicating with remote networks?
* **an extended ping**
* traceroute
* show cdp neighbors
* telnet

## Question 24
Why would a network administrator use the tracert utility?
* to check information about a DNS name in the DNS server
* **to identify where a packet was lost or delayed on a network**
* to determine the active TCP connections on a PC
* to display the IP address, default gateway, and DNS server address for a PC

## Question 25
A ping fails when performed from router R1 to directly connected router R2. The network administrator then proceeds to issue the show cdp neighbors command. Why would the network administrator issue this command if the ping failed between the two routers?
* The network administrator wants to determine if connectivity can be established from a non-directly connected network.
* The network administrator wants to verify the IP address configured on router R2.
* **The network administrator wants to verify Layer 2 connectivity.**
* The network administrator suspects a virus because the ping command did not work.

## Question 26
Which command can an administrator issue on a Cisco router to send debug messages to the vty lines?
* logging buffered
* logging synchronous
* logging console
* **terminal monitor**

## Question 27
A network engineer is analyzing reports from a recently performed network baseline. Which situation would depict a possible latency issue?
* a next-hop timeout from a traceroute
* a change in the bandwidth according to the show interfaces output
* **an increase in host-to-host ping response times**
* a change in the amount of RAM according to the show version output

## Question 28
Which command has to be configured on the router to complete the SSH configuration?
* **transport input ssh**
* service password-encryption
* ip domain-name cisco.com
* enable secret class

## Question 29
When configuring SSH on a router to implement secure network management, a network engineer has issued the login local and transport input ssh line vty commands. What three additional configuration actions have to be performed to complete the SSH configuration? (Choose three.)
* Set the user privilege levels.
* **Create a valid local username and password database.**
* Manually enable SSH after the RSA keys are generated.
* **Configure the correct IP domain name.**
* Configure role-based CLI access.
* **Generate the asymmetric RSA keys.**

## Question 30
A technician is to document the current configurations of all network devices in a college, including those in off-site buildings. Which protocol would be best to use to securely access the network devices?
* FTP
* **SSH**
* HTTP
* Telnet

## Question 31
On which two interfaces or ports can security be improved by configuring executive timeouts? (Choose two.)
* serial interfaces
* **console ports**
* **vty ports**
* loopback interfaces
* fast Ethernet interfaces

## Question 32
What is considered the most effective way to mitigate a worm attack?
* Ensure that AAA is configured in the network.
* Ensure that all systems have the most current virus definitions.
* **Download security updates from the operating system vendor and patch all vulnerable systems.**
* Change system passwords every 30 days.

## Question 33
Which statement describes the ping and tracert commands?
* Both ping and tracert can show results in a graphical display.
* Ping shows whether the transmission is successful; tracert does not.
* **Tracert shows each hop, while ping shows a destination reply only.**
* Tracert uses IP addresses; ping does not.

## Question 34
An administrator decides to use "admin" as the password on a newly installed router. Which statement applies to the password choice?
* It is strong because it contains 10 numbers and special characters.
* **It is weak because it is often the default password on new devices.**
* It is strong because it uses a minimum of 10 numbers, letters and special characters.
* It is strong because it uses a passphrase.

## Question 35
Only employees connected to IPv6 interfaces are having difficulty connecting to remote networks. The analyst wants to verify that IPv6 routing has been enabled. What is the best command to use to accomplish the task?
* copy running-config startup-config
* **show running-config**
* show interfaces
* show ip nat translations
