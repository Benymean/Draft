# Network Subnetting and Infrastructure Analysis Report

## Table of Contents

1. **Executive Summary**
2. **Subnetting Analysis**
   - **Network Topology Analysis**
   - **Subnetting Requirements**
   - **Methodology**
     - Calculation of Bits Needed for Subnetting
     - Calculation of the New Subnet Mask
     - Determining the Number of Available Hosts per Subnet
   - **Subnet Allocation Scheme for 192.168.221.0/24 Network**
   - **Expansion of New Offices**
3. **Encapsulation, Traffic and Traversal Analysis**
   - **Network Traffic Analysis: ICMP Echo Request from PC1 to Printer2**
      - Initial ARP Request by PC1
      - ARP Reply from Gateway
      - ICMP Echo Request from PC1
      - Router Forwards ICMP to Printer2's Network
      - ARP Reply from Printer2
      - CMP Echo Reply from Printer2
   - **Analysis of Path Determination**
4. **Data-Link Layer Analysis**
   - **Managed vs. Unmanaged Switches**
   - **Impact on Network Addressing**
   - **Identification of Switch Manufacturer**
   - **Uplink Interface from a Different Manufacturer**
   - **Devices and Broadcast Domains**
   - **Devices and Collision Domains**
5. **References**

\pagebreak

## 1. Executive Summary

This report delivers an exhaustive analysis of critical network infrastructure components spanning several key areas. From examining the subtleties of traffic traversal between offices to dissecting the nuances of subnetting for optimal network segmentation, the report synthesizes a comprehensive understanding of our network's current and future state. We also delve into the specifics of Data-Link layer functionality, assessing the impact of switch types on network performance and management.

\pagebreak

## 2. Subnetting Analysis
This section of the report analyzes the current network structure and proposes a subnetting scheme that not only meets the immediate needs of each office and center but also accommodates the growth of our network infrastructure with additional subnets for new office locations. Here is a methodical approach to calculating the necessary bits for subnetting, defining the new subnet masks, and determining the host capacities for each subnet.

### 2.1 Network Topology Analysis

The current network topology reveals distinct sections that necessitate separate subnets to accommodate their unique communication and management needs. These sections include:

- Bathurst Office
- Parramatta CBD Office
- Ultimo Data Center
- Ultimo Call Center
- WAN 1
- WAN 2

The assessment considers the absence of VLANs or other logical separations, focusing on ensuring efficient network organization and management.

### 2.2 Subnetting Requirements

Based on the physical and logical layout depicted in the network topology, the following subnetting strategy is proposed:

- **Bathurst Office**: Requires one subnet to support its network infrastructure. A dedicated subnet will facilitate localized network management and security, given its distinct location and function.
- **Parramatta CBD Office**: Requires one subnet. This allocation allows for tailored network configurations that meet the operational requirements of the Parramatta CBD office
- **Ultimo Data Center and Call Center**: Requires two subnets. The two switches connected to one router logically indicate that the Data Center and Call Center operate on separate subnets.
- **WAN 1 and WAN 2**: Requires eight subnets. This allocation is crucial for managing wide-area network traffic, ensuring robust connectivity and security across disparate network segments.

A total of 12 subnets are required to optimize network efficiency, security, and manageability across different operational areas and WAN connections.

### 2.3 Methodology

#### Calculation of Bits Needed for Subnetting

To accommodate the requirement for 12 subnets within the 192.168.221.0/24 network, it was essential to calculate the number of bits needed from the host portion of the IP address. The calculation is based on the formula:

$$2^X \geq \text{Number of Subnets}$$

Given the need for 12 subnets, we apply the formula as follows:

$$2^X \geq 12$$

Evaluating \(X\) for the smallest integer value that meets or exceeds 12 subnets:

$$
\begin{aligned}
&\text{For } X = 3, 2^3 = 8, \text{ which is not sufficient.} \\
&\text{For } X = 4, 2^4 = 16, \text{ which meets the requirement.}
\end{aligned}
$$

Thus, \(X = 4\) bits must be borrowed from the host part of the IP address to create a minimum of 12 subnets.

#### Calculation of the New Subnet Mask

With the original network being a /24, indicating that 24 bits are reserved for the network part, an additional 4 bits calculated for subnetting were added to derive the new subnet mask. This process adjusts the subnet mask from /24 to /28. The adjustment is detailed as follows:

$$
\text{Original Subnet Mask} = /24
$$

And determining that \(X = 4\) bits need to be borrowed, we update the subnet mask to:

$$
\text{New Subnet Mask} = /24 + X = /24 + 4 = /28
$$

This transition to a /28 subnet mask allows for more granular control over network segmentation and addressing.

#### Determining the Number of Available Hosts per Subnet

After allocating bits for subnet creation, the remaining bits in the host part were used to determine the maximum number of hosts per subnet. The formula employed accounts for the subtraction of 2 (for the network address and the broadcast address, which are not assignable to hosts) as follows:

$$\text{Available Hosts per Subnet} = 2^{(32 - \text{New Subnet Mask})} - 2$$

With the adoption of a /28 subnet mask, this translates to:

$$\text{Available Hosts per Subnet} = 2^{(32 - 28)} - 2 = 2^4 - 2 = 14$$

This result shows that each subnet can accommodate up to 14 hosts, taking into account the subtraction of 2 for the network address and the broadcast address, which are not assignable to hosts.

### 2.4 Subnet Allocation Scheme

| Subnet No. | Network Address   | Broadcast Address   | Usable Host Range             | Max Hosts | Netmask           |
|------------|-------------------|---------------------|-------------------------------|-----------|-------------------|
| 1          | 192.168.221.0     | 192.168.221.15      | 192.168.221.1 - 192.168.221.14  | 14        | 255.255.255.240   |
| 2          | 192.168.221.16    | 192.168.221.31      | 192.168.221.17 - 192.168.221.30 | 14        | 255.255.255.240   |
| 3          | 192.168.221.32    | 192.168.221.47      | 192.168.221.33 - 192.168.221.46 | 14        | 255.255.255.240   |
| 4          | 192.168.221.48    | 192.168.221.63      | 192.168.221.49 - 192.168.221.62 | 14        | 255.255.255.240   |
| 5          | 192.168.221.64    | 192.168.221.79      | 192.168.221.65 - 192.168.221.78 | 14        | 255.255.255.240   |
| 6          | 192.168.221.80    | 192.168.221.95      | 192.168.221.81 - 192.168.221.94 | 14        | 255.255.255.240   |
| 7          | 192.168.221.96    | 192.168.221.111     | 192.168.221.97 - 192.168.221.110 | 14        | 255.255.255.240   |
| 8          | 192.168.221.112   | 192.168.221.127     | 192.168.221.113 - 192.168.221.126 | 14        | 255.255.255.240   |
| 9          | 192.168.221.128   | 192.168.221.143     | 192.168.221.129 - 192.168.221.142 | 14        | 255.255.255.240   |
| 10         | 192.168.221.144   | 192.168.221.159     | 192.168.221.145 - 192.168.221.158 | 14        | 255.255.255.240   |
| 11         | 192.168.221.160   | 192.168.221.175     | 192.168.221.161 - 192.168.221.174 | 14        | 255.255.255.240   |
| 12         | 192.168.221.176   | 192.168.221.191     | 192.168.221.177 - 192.168.221.190 | 14        | 255.255.255.240   |

*This table outlines the division of the 192.168.221.0/24 network into 12 subnets using a Fixed Length Subnet Mask (FLSM) strategy.*

### 2.5 Expansion of New Offices

The initial FLSM strategy, designed for 12 subnets within the 192.168.221.0/24 network, can be effectively expanded to integrate three new office locations: Armidale, Cooma, and Dubbo. These offices mirror the subnet structure of the Bathurst office, with each requiring a single subnet to support their operations.

The subnet allocations for the new offices will be:

- **Armidale**: Assigned the subnet 192.168.221.192/28, with a usable host range of 192.168.221.193 to 192.168.221.206.
- **Cooma**: Assigned the subnet 192.168.221.208/28, with a usable host range of 192.168.221.209 to 192.168.221.222.
- **Dubbo**: Assigned the subnet 192.168.221.224/28, with a usable host range of 192.168.221.225 to 192.168.221.238.

With these additions, the updated FLSM scheme will utilizes 15 of the possible 16 subnets, effectively leaving one subnet in reserve for future network expansion or additional requirements.

\pagebreak

## 3. Encapsulation, Traffic and Traversal Analysis

This part of the report provides a succinct overview of the encapsulation and path determination processes for an ICMP echo request sent from PC1 at the Bathurst office to Printer2 in the Parramatta CBD. The communication initiates with an ARP request to establish MAC address mappings, followed by an ICMP packet journey through the network, which is subject to routing decisions based on protocol-specific metrics and configurations.

### 3.1 Network Traffic Analysis: ICMP Echo Request from PC1 to Printer2

#### Initial ARP Request by PC1
- PC1 initiated an ARP broadcast to determine the MAC address of its default gateway.
- **Traffic Created:** `[Ethernet SRC: 00:80:10:5B:48:26, DST: FF:FF:FF:FF:FF:FF [ARP Who has {Gateway IP}? Tell 00:80:10:5B:48:26]]`
- **Resulting Action:** The local switch (Switch5) updated its MAC table, recording the MAC address of PC1.

#### ARP Reply from Gateway
- The gateway for Bathurst responded with its MAC address.
- **Traffic Created:** `[Ethernet SRC: {Gateway MAC}, DST: 00:80:10:5B:48:26 [ARP {Gateway IP} is at {Gateway MAC}]]`
- **Resulting Action:** PC1 updated its ARP cache with the gateway's MAC address.

#### ICMP Echo Request from PC1
- PC1 now sends the ICMP Echo Request destined for Printer2, but directed to the router’s MAC since it is on a different network.
- **Traffic Created:** `[Ethernet SRC: 00:80:10:5B:48:26, DST: {Gateway MAC} [IP SRC: {PC1 IP}, DST: {Printer2 IP} [ICMP Echo Request]]]`
- **Resulting Action:** Router1 updates its MAC table with PC1's MAC address.

#### Router Forwards ICMP to Printer2's Network
- The Parramatta router might send an ARP request if Printer2's MAC address is unknown.
- **Traffic Created (if needed):** `[Ethernet SRC: {Router Parramatta MAC}, DST: FF:FF:FF:FF:FF:FF [ARP Who has {Printer2 IP}? Tell {Router Parramatta MAC}]]`
- **Resulting Action:** Relevant MAC tables and ARP caches are updated.

#### ARP Reply from Printer2
- Printer2 responds with its MAC address to the router.
- **Traffic Created:** `[Ethernet SRC: 00:00:0F:03:2B:42, DST: {Router Parramatta MAC} [ARP {Printer2 IP} is at 00:00:0F:03:2B:42]]`

#### ICMP Echo Reply from Printer2
- Printer2 sends an ICMP Echo Reply to PC1.
- **Traffic Created:** `[Ethernet SRC: 00:00:0F:03:2B:42, DST: {Router Parramatta MAC} [IP SRC: {Printer2 IP}, DST: {PC1 IP} [ICMP Echo Reply]]]`
- **Resulting Action:** The echo reply follows the reverse path to reach PC1.

### 3.2 Analysis of Path Determination
Given the lack of specific metrics and network configurations, the choice of path for traffic from PC1 in Bathurst to Printer2 in Parramatta CBD is determined by the routing protocol used within the network. Here's a brief explanation of the path selection:

- **Hop Count:**
**WAN1** consists of three router hops, which is generally preferred for being the shorter path. Routing protocols like RIP would choose **WAN1** due to its lower hop count [1].

- **Routing Protocol Metrics:**
If the network employs OSPF or EIGRP, the path could be selected based on bandwidth, delay, or other factors.
Even with these advanced protocols, **WAN1** could still be preferred if it provides sufficient bandwidth and lower latency [2].

- **Network Configuration and ISP Policies**
Routing protocols are the rules or algorithms that dictate how routers communicate with each other to distribute routing information [2]. The actual path could differ if network policies prioritize metrics other than hop count.
**WAN2** could be chosen if, for example, it offers higher bandwidth or more reliable links, despite having an additional hop.

In absence of specific details, the general assumption would default to the path with fewer hops, indicating **WAN1** as the likely path taken.

\pagebreak

## 4. Data-Link Layer Analysis
Understanding the Data-Link layer's dynamics is essential for optimizing our network's performance and reliability. This section evaluates the role of switch types within our infrastructure, their impact on IP addressing, and network segmentation. It also explores the implications of mixed-manufacturer environments and the structuring of broadcast and collision domains.

### A) Managed vs. Unmanaged Switches

**Managed Switches** supports deployment across various network topologies like Spanning Tree Protocol, ring, mesh, stacking, and aggregation, enhancing redundancy and reliability. It streamlines network management and troubleshooting with features like remote and SDN management, telemetry data for traffic insights, and power supply to endpoint devices. Robust security measures regulate access, monitor for threats, and address breaches, ensuring network safety. Additionally, it optimizes device and application performance through quality-of-service (QoS) features, prioritizing traffic and organizing devices by service type for improved network efficiency [3].

**Unmanaged Switches** offer basic connectivity at a low cost with plug-and-play operation through autonegotiation. They are best suited for simple network topologies like star and daisy chain configurations. These switches improve upon Ethernet hubs by creating and storing MAC-address tables for slightly enhanced traffic management. However, they do not differentiate between multicast and broadcast traffic, leading to potential congestion problems known as broadcast storms. This issue is particularly critical for industrial IoT devices that depend on multicast traffic for device commands [3]. 

### B) Impact on Network Addressing

**Managed switches** allow network administrators to carve out multiple VLANs, enabling the creation of numerous broadcast domains within a single physical network infrastructure. This leads to improved network organization, security, and traffic management [4]. 

**Unmanaged switches** generally cannot be modified or managed [5], resulting in a uniform network space without the benefits of segmentation, typically referred to as a flat network topology [6].

### C) Identification of Switch Manufacturer

The Manufacturer of Switch12 has been identified through the MAC address analysis. The MAC address, a unique identifier assigned by the manufacturer, consists of an Organizationally Unique Identifier (OUI) [7], which comprises the first half of the address. For Switch12, with a MAC address of **5C:16:C7:54:4A:2B**, the OUI **5C:16:C7** has been traced to **Arista Networks** by querying a publicly accessible hardware address database.

### D) Uplink Interface from a Different Manufacturer

There are several practical reasons why Switch12 may have a different uplink interface from a different manufacturer

- **Compatibility and Performance**: Network devices from different manufacturers offer unique features and performance for specific tasks. Unlike normal ports Uplink ports are meant for higher-level traffic [8]. A different company's interface might be used if it offers this superior performance, better compatibility with existing network standards, or specific features that enhance the functionality between the switch and router.

- **Cost-Effectiveness & Avoidance of Vendor Lock-in:**: Cost is a major factor in network design. The decision might be driven by budget constraints, where the interface from another company offers a more cost-effective solution without significantly compromising on quality or performance. The relationship with vendors and the support they offer can influence hardware selection. Using different uplink interfaces from different manufacturers helps avoid vendor lock-in, which can lead to competitive pricing and more options for negotiating service contracts [8].

- **Improved Network Scalability:** Uplink ports facilitate the efficient aggregation of traffic from multiple sources, enhancing the overall performance of the network. As organizations grow, their network traffic and connectivity needs also expand. The ability to incorporate switches with uplink interfaces from various manufacturers into an existing network setup allows for greater flexibility. This means networks can be expanded or reconfigured with minimal disruptions and without the need for replacing the entire infrastructure [9].

- **Interoperability and Standards Compliance**: Networking standards ensure that devices from different manufacturers can work together seamlessly. This allows network designers to choose from a wider range of products to meet their specific needs, focusing on interoperability and compliance with networking standards [10].

In summary, the decision to use an uplink interface from a different company involves balancing various factors, including performance, cost, vendor relationships, network evolution, and the specific needs of the network architecture.

### E) Devices and Broadcast Domains

This table only includes the broadcast domains for local area networks and does not include the WAN links since they are typically not considered broadcast domains due to their point-to-point nature [11]. Each router's local interface that connects to the local network can also be seen as part of the broadcast domain it serves.

| Broadcast Domain    | Devices Included                                                                                      |
|---------------------|------------------------------------------------------------------------------------------------------|
| Paramatta CBD       | Server1, Server2, Printer2, PC0, Door Card Reader, Switch1, Switch3, Router6 interface (for CBD)     |
| Bathurst            | PC1, PC2, PC3, IP Phone1, IP Phone3, Hub1, Switch5, Router1 interface (for Bathurst)                  |
| Ultimo Call Center  | Call1, Call2, ..., Call10, Call Center Switch, Router8 interface (for Call Center)                    |
| Ultimo Data Center  | Server10, Server11, ..., Server13, Data Center Switch, Router8 interface (for Data Center)           |

### F) Devices and Collision Domains

A collision domain is a network segment where data packets can "collide" with one another when being sent over a network medium. A hub shares the same collision domain across all of its ports. This means that all devices connected to a hub are in the same collision domain. However, switches and routers do not propagate collisions; each port on a switch or router is typically its own collision domain [12].

| Collision Domain                      | Devices in Collision Domain                                      |
|---------------------------------------|------------------------------------------------------------------|
| Paramatta CBD - Switch1 Port 1        | Server1                                                          |
| Paramatta CBD - Switch1 Port 2        | Server2                                                          |
| Paramatta CBD - Switch3 Port 1        | PC0                                                              |
| Paramatta CBD - Switch3 Port 2        | Door Card Reader                                                 |
| Paramatta CBD - Switch1 to Router6    | Router6 (CBD Interface)                                          |
| Bathurst - Switch5 Port 1             | PC1                                                              |
| Bathurst - Switch5 Port 2             | PC2                                                              |
| Bathurst - Switch5 to Router1         | Router1 (Bathurst Interface)                                     |
| Bathurst - Hub1                       | IP Phone1, IP Phone3, PC3 (All share one collision domain with the hub) |
| Ultimo Call Center - Switch 12 Port 1 | Call1                                                            |
| Ultimo Call Center - Switch 12 Port 2 | Call2                                                            |
| ...                                   | ... (additional Call devices each on their own port)             |
| Ultimo Call Center - Switch 12 Port 10| Call10                                                           |
| Ultimo Data Center - Switch 6 Port 1  | Server10                                                         |
| Ultimo Data Center - Switch 6 Port 2  | Server11                                                         |
| Ultimo Data Center - Switch 6 Port 3  | Server12                                                         |
| Ultimo Data Center - Switch 6 Port 4  | Server13                                                         |
| WAN 1 - Router2 to Router4            | Router2 Interface, Router4 Interface                             |
| WAN 2 - Router4 to Router1            | Router4 Interface, Router1 Interface                             |
| WAN 3 - Router3 to Router5            | Router3 Interface, Router5 Interface                             |
| WAN 4 - Router5 to Router7            | Router5 Interface, Router7 Interface                             |
| WAN 5 - Router3 to Router1            | Router3 Interface, Router1 Interface                             |

\pagebreak

## 5. References

[1] C. L. Hedrick, “Routing Information Protocol,” *RFC Editor*, 1988.

[2] J. Kurose and K. Ross, Computer networking: A top-down approach, global edition, 8th ed. London, England: Pearson Education, 2021. [Ebook] Available: Univerzitet Crne Go

[3] C. Hilla, “Managed vs. unmanaged switches? Explore the benefits of network automation,” *Cisco Blogs*, 31-Oct-2019. [Online]. Available: https://blogs.cisco.com/manufacturing/managed-vs-unmanaged-switches-explore-the-benefits-of-network-automationdtidesootr000515ccidcc001101. [Accessed: 03-Apr-2024].

[4] T. Agarwal, “Managed Switch : Working, Differences, Advantages & its applications,” ElProCus - *Electronic Projects for Engineering Students*, 06-Jul-2022. [Online]. Available: https://www.elprocus.com/managed-switch/. [Accessed: 03-Apr-2024].

[5] “Different Types of Network Switches,” *Cisco*. [Online]. Available: https://www.cisco.com/c/en/us/solutions/small-business/resource-center/networking/understanding-the-different-types-of-network-switches.html [Accessed: 03-Apr-2024].

[6]“Understanding a flat network - Enterprise Cloud Security and Governance,” *O'reilly* [Online]. Available: https://learning.oreilly.com/library/view/enterprise-cloud-security/9781788299558/a15b6210-c3cd-4823-81f5-b57eaacb08e2.xhtml [Accessed Apr. 36, 2024].

[7] IEEE Registration Authority,” *IEEE Standards Association*, 05-Jan-2022. [Online]. Available: https://standards.ieee.org/faqs/regauth/. [Accessed: 04-Apr-2024].

[8] AscentOptics, “Understanding switch uplink ports and their functionality,” *AscentOptics Blog,* 22-Dec-2023. [Online]. Available: https://ascentoptics.com/blog/understanding-switch-uplink-ports-and-their-functionality/. [Accessed: 06-Apr-2024].

[9] Acre security, “Uplink Ports: their importance, variations, and significance,” *Acresecurity.com,* 15-Nov-2023. . [Online]. Available: https://acresecurity.com/blog/uplink-ports-their-importance-variations-and-significance [Accessed: Apr. 2, 2024]. 
‌

[10] S. Dahmen-Lhuissier, “Why standards,” *ETSI*. [Online]. Available: https://www.etsi.org/standards/why-standards. [Accessed: 03-Apr-2024].

[11] Cisco networking academy connecting networks companion guide: Connecting to the WAN,” *Ciscopress.com.* [Online]. Available: https://www.ciscopress.com/articles/article.asp?p=2202411&seqNum=5. [Accessed: 06-Apr-2024].
‌

[12] D. Rountree, Security for Microsoft Windows System Administrators. *Syngress Publishing*, 2011. [Ebook] Available: O'reilly
