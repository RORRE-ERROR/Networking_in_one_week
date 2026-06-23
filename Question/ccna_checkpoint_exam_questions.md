# CCNA Checkpoint Exam Questions

## Question 5

**What is a characteristic of a static route that matches all packets?**

* It uses a single network address to send multiple static routes to one destination address.
* It backs up a route already discovered by a dynamic routing protocol.
* It identifies the gateway IP address to which the router sends all IP packets for which it does not have a learned or static route.
* It is configured with a higher administrative distance than the original dynamic routing protocol has.

**Correct Answer:**
It identifies the gateway IP address to which the router sends all IP packets for which it does not have a learned or static route.

---

## Question 6

**A network administrator configures the interface fa0/0 on the router R1 with the command ip address 172.16.1.254 255.255.255.0. However, when the administrator issues the command show ip route, the routing table does not show the directly connected network. What is the possible cause of the problem?**

* The configuration needs to be saved first.
* No packets with a destination network of 172.16.1.0 have been sent to R1.
* The subnet mask is incorrect for the IPv4 address.
* The interface fa0/0 has not been activated.

**Correct Answer:**
The interface fa0/0 has not been activated.

---

## Question 7

**When would it be more beneficial to use a dynamic routing protocol instead of static routing?**

* on a stub network that has a single exit point
* in an organization where routers suffer from performance issues
* on a network where there is a lot of topology changes
* in an organization with a smaller network that is not expected to grow in size

**Correct Answer:**
on a network where there is a lot of topology changes

---

## Question 8

**On which two routers would a default static route be configured? (Choose two.)**

* edge router connection to the ISP
* any router running an IOS prior to 12.0
* the router that serves as the gateway of last resort
* any router where a backup route to dynamic routing is needed for reliability
* stub router connection to the rest of the corporate or campus network

**Correct Answer:**
* edge router connection to the ISP
* stub router connection to the rest of the corporate or campus network

---

## Question 9

**Refer to the exhibit. What two commands will change the next-hop address for the 10.0.0.0/8 network from 172.16.40.2 to 192.168.1.2? (Choose two.)**

* A(config)# ip route 10.0.0.0 255.0.0.0 s0/0/0
* A(config)# ip route 10.0.0.0 255.0.0.0 192.168.1.2
* A(config)# no network 10.0.0.0 255.0.0.0 172.16.40.2
* A(config)# no ip address 10.0.0.1 255.0.0.0 172.16.40.2
* A(config)# no ip route 10.0.0.0 255.0.0.0 172.16.40.2

**Correct Answer:**
* A(config)# ip route 10.0.0.0 255.0.0.0 192.168.1.2
* A(config)# no ip route 10.0.0.0 255.0.0.0 172.16.40.2

---

## Question 10

**What is a characteristic of a floating static route?**

* It is simply a static route with 0.0.0.0/0 as the destination IPv4 address.
* It is configured with a higher administrative distance than the original dynamic routing protocol has.
* It is used to provide load balancing between static routes.
* When it is configured, it creates a gateway of last resort.

**Correct Answer:**
It is configured with a higher administrative distance than the original dynamic routing protocol has.

---

## Question 11

**Refer to the exhibit. Which set of commands will configure static routes that will allow the Park and the Alta routers to a) forward packets to each LAN and b) direct all other traffic to the Internet?**

* Park(config)# ip route 0.0.0.0 0.0.0.0 192.168.14.1
Alta(config)# ip route 10.0.234.0 255.255.255.0 192.168.14.2
Alta(config)# ip route 0.0.0.0 0.0.0.0 s0/0/0
* Park(config)# ip route 0.0.0.0 0.0.0.0 192.168.14.1
Alta(config)# ip route 10.0.234.0 255.255.255.0 192.168.14.2
Alta(config)# ip route 198.18.222.0 255.255.255.255 s0/0/0
* Park(config)# ip route 172.16.67.0 255.255.255.0 192.168.14.1
Park(config)# ip route 0.0.0.0 0.0.0.0 192.168.14.1
Alta(config)# ip route 10.0.234.0 255.255.255.0 192.168.14.2
* Park(config)# ip route 172.16.67.0 255.255.255.0 192.168.14.1
Alta(config)# ip route 10.0.234.0 255.255.255.0 192.168.14.2
Alta(config)# ip route 0.0.0.0 0.0.0.0 s0/0/1

**Correct Answer:**
Park(config)# ip route 0.0.0.0 0.0.0.0 192.168.14.1
Alta(config)# ip route 10.0.234.0 255.255.255.0 192.168.14.2
Alta(config)# ip route 0.0.0.0 0.0.0.0 s0/0/0

---

## Question 12

**Consider the following command:

`ip route 192.168.10.0 255.255.255.0 10.10.10.2 5`

Which route would have to go down in order for this static route to appear in the routing table?**

* an EIGRP-learned route to the 192.168.10.0/24 network
* an OSPF-learned route to the 192.168.10.0/24 network
* a default route
* a static route to the 192.168.10.0/24 network

**Correct Answer:**
a static route to the 192.168.10.0/24 network

---

## Question 13

**Consider the following command:

`ip route 192.168.10.0 255.255.255.0 10.10.10.2 5`

How would an administrator test this configuration?**

* Ping any valid address on the 192.168.10.0/24 network.
* Manually shut down the router interface used as a primary route.
* Ping from the 192.168.10.0 network to the 10.10.10.2 address.
* Delete the default gateway route on the router.

**Correct Answer:**
Manually shut down the router interface used as a primary route.

---

## Question 14

**Consider the following command:

`ip route 192.168.10.0 255.255.255.0 10.10.10.2 5`

What does the 5 at the end of the command signify?**

* exit interface
* administrative distance
* maximum number of hops to the 192.168.10.0/24 network
* metric

**Correct Answer:**
administrative distance

---

## Question 15

**Refer to the exhibit. Which type of IPv6 static route is configured in the exhibit?
`ipv6 route 2001:0DB8::/32 2001:0DB8:3000::1`**

* directly attached static route
* floating static route
* fully specified static route
* recursive static route

**Correct Answer:**
recursive static route

---

## Question 16

**A router has used the OSPF protocol to learn a route to the 172.16.32.0/19 network. Which command will implement a backup floating static route to this network?**

* ip route 172.16.0.0 255.255.240.0 S0/0/0 200
* ip route 172.16.0.0 255.255.224.0 S0/0/0 100
* ip route 172.16.32.0 255.255.0.0 S0/0/0 100
* ip route 172.16.32.0 255.255.224.0 S0/0/0 200

**Correct Answer:**
ip route 172.16.32.0 255.255.224.0 S0/0/0 200

---

## Question 17

**Refer to the exhibit. This network has two connections to the ISP, one via router C and one via router B. The serial link between router A and router C supports EIGRP and is the primary link to the Internet. If the primary link fails, the administrator needs a floating static route that avoids recursive route lookups and any potential next-hop issues caused by the multiaccess nature of the Ethernet segment with router B. What should the administrator configure?**

* Create a fully specified static route pointing to Fa0/0 with an AD of 1.
* Create a fully specified static route pointing to Fa0/0 with an AD of 95.
* Create a static route pointing to Fa0/0 with an AD of 1.
* Create a static route pointing to 10.1.1.1 with an AD of 95.
* Create a static route pointing to 10.1.1.1 with an AD of 1.

**Correct Answer:**
Create a fully specified static route pointing to Fa0/0 with an AD of 95.

---

## Question 18

**Refer to the graphic. Which command would be used on router A to configure a static route to direct traffic from LAN A that is destined for LAN C?**

* A(config)# ip route 192.168.3.2 255.255.255.0 192.168.4.0
* A(config)# ip route 192.168.4.0 255.255.255.0 192.168.3.2
* A(config)# ip route 192.168.4.0 255.255.255.0 192.168.5.2
* A(config)# ip route 192.168.3.0 255.255.255.0 192.168.3.1
* A(config)# ip route 192.168.5.0 255.255.255.0 192.168.3.2

**Correct Answer:**
A(config)# ip route 192.168.4.0 255.255.255.0 192.168.3.2

---

## Question 19

**Refer to the exhibit. An administrator is attempting to install an IPv6 static route on router R1 to reach the network attached to router R2. After the static route command is entered, connectivity to the network is still failing. What error has been made in the static route configuration?**

* The next hop address is incorrect.
* The interface is incorrect.
* The network prefix is incorrect.
* The destination network is incorrect.

**Correct Answer:**
The interface is incorrect.

---

## Question 20

**Refer to the exhibit. How was the host route 2001:DB8:CAFE:4::1/128 installed in the routing table?**

* The route was manually entered by an administrator.
* The route was dynamically learned from another router.
* The route was automatically installed when an IP address was configured on an active interface.
* The route was dynamically created by router R1.

**Correct Answer:**
The route was manually entered by an administrator.

---

## Question 21

**Refer to the exhibit. A ping from R1 to 10.1.1.2 is successful, but a ping from R1 to any address in the 192.168.2.0 network fails. What is the cause of this problem?**

* There is no gateway of last resort at R1.
* A default route is not configured on R1.
* The static route for 192.168.2.0 is incorrectly configured.
* The serial interface between the two routers is down.

**Correct Answer:**
The static route for 192.168.2.0 is incorrectly configured.

---

## Question 22

**Refer to the exhibit. The Branch Router has an OSPF neighbor relationship with the HQ router over the 198.51.0.4/30 network. The 198.51.0.8/30 network link should serve as a backup when the OSPF link goes down. The floating static route command ip route 0.0.0.0 0.0.0.0 S0/1/1 100 was issued on Branch and now traffic is using the backup link even when the OSPF link is up and functioning. Which change should be made to the static route command so that traffic will only use the OSPF link when it is up?**

* Change the administrative distance to 1.
* Add the next hop neighbor address of 198.51.0.8.
* Change the administrative distance to 120.
* Change the destination network to 198.51.0.5.

**Correct Answer:**
Change the administrative distance to 120.

---

## Question 23

**Refer to the exhibit. HostA is attempting to contact ServerB. Which two statements correctly describe the addressing that HostA will generate in the process? (Choose two.)**

* A packet with the destination IP address of RouterA.
* A packet with the destination IP address of ServerB.
* A packet with the destination IP address of RouterB.
* A frame with the destination MAC address of ServerB.
* A frame with the destination MAC address of SwitchA.
* A frame with the destination MAC address of RouterA.

**Correct Answer:**
* A packet with the destination IP address of ServerB.
* A frame with the destination MAC address of RouterA.

---

## Question 24

**Refer to the exhibit. The small company shown uses static routing. Users on the R2 LAN have reported a problem with connectivity. What is the issue?**

* R1 needs a default route to R2.
* R2 needs a static route to the R1 LANs.
* R1 needs a static route to the R2 LAN.
* R1 and R2 must use a dynamic routing protocol.
* R2 needs a static route to the Internet.

**Correct Answer:**
R2 needs a static route to the R1 LANs.

---

## Question 25

**Refer to the exhibit. An administrator is attempting to install a default static route on router R1 to reach the Site B network on router R2. After entering the static route command, the route is still not showing up in the routing table of router R1. What is preventing the route from installing in the routing table?**

* The next hop address is incorrect.
* The netmask is incorrect.
* The destination network is incorrect.
* The exit interface is missing.

**Correct Answer:**
The next hop address is incorrect.

---

## Question 26

**What characteristic completes the following statement?
When an IPv6 static route is configured, the next-hop address can be ......**

* a destination host route with a /128 prefix.
* the "show ipv6 route static" command.
* an IPv6 link-local address on the adjacent router.
* the interface type and interface number.

**Correct Answer:**
an IPv6 link-local address on the adjacent router.

---

## Question 27

**Refer to the exhibit. Which interface will be the exit interface to forward a data packet that has the destination IP address 172.19.115.206?**

* None, the packet will be dropped.
* GigabitEthernet0/0.
* GigabitEthernet0/1.
* The packet will take the gateway of last resort.

**Correct Answer:**
None, the packet will be dropped.

---

## Question 28

**Refer to the exhibit. Which static route would an IT technician enter to create a backup route to the 172.16.1.0 network that is only used if the primary RIP learned route fails?**

* ip route 172.16.1.0 255.255.255.0 s0/0/0
* ip route 172.16.1.0 255.255.255.0 s0/0/0 121
* ip route 172.16.1.0 255.255.255.0 s0/0/0 111
* ip route 172.16.1.0 255.255.255.0 s0/0/0 91

**Correct Answer:**
ip route 172.16.1.0 255.255.255.0 s0/0/0 121

---

## Question 29

**What two pieces of information are needed in a fully specified static route to eliminate recursive lookups? (Choose two.)**

* the IP address of the exit interface
* the administrative distance for the destination network
* the IP address of the next-hop neighbor
* the interface ID of the next-hop neighbor
* the interface ID exit interface

**Correct Answer:**
* the IP address of the next-hop neighbor
* the interface ID exit interface

---

## Question 30

**Refer to the exhibit. Which command will properly configure an IPv6 static route on R2 that will allow traffic from PC2 to reach PC1 without any recursive lookups by router R2?**

* R2(config)# ipv6 route 2001:db8:10:12::/64 2001:db8:32::1
* R2(config)# ipv6 route 2001:db8:10:12::/64 s0/0/0
* R2(config)# ipv6 route 2001:db8:10:12::/64 s0/0/1
* R2(config)# ipv6 route ::/0 2001:db8:32::1

**Correct Answer:**
R2(config)# ipv6 route 2001:db8:10:12::/64 s0/0/1 2001:db8:32::1

---

## Question 31

**Refer to the exhibit. What would happen after the IT administrator enters the new static route?**

* The 0.0.0.0 default route would be replaced with the 172.16.1.0 static route.
* The 172.16.1.0 route learned from RIP would be replaced with the 172.16.1.0 static route.
* The 172.16.1.0 static route would be entered into the running-config but not shown in the routing table.
* The 172.16.1.0 static route is added to the existing routes in the routing table.

**Correct Answer:**
The 172.16.1.0 route learned from RIP would be replaced with the 172.16.1.0 static route.

---

## Question 32

**Refer to the exhibit. What routing solution will allow both PC A and PC B to access the Internet with the minimum amount of router CPU and network bandwidth utilization?**

* Configure a static default route from R1 to Edge, a default route from Edge to the Internet, and a static route from Edge to R1.
* Configure a dynamic routing protocol between R1 and Edge and advertise all routes.
* Configure a static route from R1 to Edge and a dynamic route from Edge to R1.
* Configure a dynamic route from R1 to Edge and a static route from Edge to R1.

**Correct Answer:**
Configure a static default route from R1 to Edge, a default route from Edge to the Internet, and a static route from Edge to R1.

---

## Question 33

**Open the PT Activity. Perform the tasks in the activity instructions and then answer the question.

A user reports that PC0 cannot visit the web server www.server.com. Troubleshoot the network configuration to identify the problem.

What is the cause of the problem?**

* The DNS server address on PC0 is configured incorrectly.
* Routing between HQ and Branch is configured incorrectly.
* The clock rate on one of the serial links is configured incorrectly.
* A serial interface on Branch is configured incorrectly.

**Correct Answer:**
The DNS server address on PC0 is configured incorrectly.

---

