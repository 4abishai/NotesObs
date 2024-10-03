Unicast Routing Protocols include mechanisms for communication between routers, domain management, and interaction with other protocols. Three major unicast routing protocols used on the Internet today are:

1. **Routing Information Protocol (RIP)** – based on the **distance-vector algorithm**.
2. **Open Shortest Path First (OSPF)** – based on the **link-state algorithm**.
3. **Border Gateway Protocol (BGP)** – based on the **path-vector algorithm**.
## Internet Structure

The Internet has evolved from a simple, hierarchical tree-like structure with a single backbone into a more complex, **multi-backbone system**. Today, several backbones are operated by private communication companies that provide **global connectivity**. 

At various levels, **provider networks** and **consumer networks** connect to these backbones, forming a vast web of interconnected ISPs. The three key layers in this structure are:

1. **Backbones**: Large, global networks providing connectivity at the highest level.
2. **Provider Networks**: Mid-level networks that provide services to end users and utilize the backbones for global connectivity.
3. **Consumer Networks**: Small-scale networks (e.g., home or business) that connect to the provider networks for Internet services.
	![[Pasted image 20240930190106.png]]
- These backbones and networks connect at **peering points**, allowing data to move between different networks. 
- Any of these networks, whether backbone, provider, or consumer, can be considered an **Internet Service Provider (ISP)**.

### Hierarchical Routing

Due to the scale and complexity of the Internet, *routing cannot rely on a single protocol*. The vast number of routers and networks poses two main challenges:

1. **Scalability**: A single routing protocol would require enormous forwarding tables, making packet forwarding and updating tables inefficient and time-consuming.
2. **Administrative Autonomy**: Each **Autonomous System (AS)**, which can be an ISP or a collection of networks, is managed independently. *Each AS has its own policies, routing preferences, and infrastructure.*

To address these challenges, the Internet employs **hierarchical routing**, where each AS can run its own internal routing protocol. The **intra-AS routing protocols**, also known as **intradomain routing protocols** or **Interior Gateway Protocols (IGPs)**, manage routing within an AS. On the other hand, **inter-AS routing**, or **interdomain routing**, is managed by a single, global protocol known as the **Exterior Gateway Protocol (EGP)**, which facilitates communication between different ASs.

- **Intra-AS routing protocols (IGPs)**: 
	- These are used for *routing within an AS*. Common examples include:
		- **RIP**: A simple distance-vector-based protocol.
		- **OSPF**: A more advanced link-state-based protocol.
	- Provides efficient local routing.
  
- **Inter-AS routing protocols (EGP)**: 
	- Used for *routing between ASs*. 
		- **BGP**: Path-vector-based protocol.
	- Plays a crucial role in ensuring *global connectivity* and *policy-based routing* across the Internet.
### Autonomous Systems (AS)

- *It is a network or collection of networks under a single administrative entity, typically an Internet Service Provider (ISP).* 
- Each AS operates independently when it comes to managing its internal routing policies and infrastructure. 
- The **Internet Corporation for Assigned Names and Numbers (ICANN)** assigns each AS a unique **Autonomous System Number (ASN)**, which is a 16-bit *identifier*. 

Autonomous Systems are not categorized by their size but by how they connect and interact with other ASs. Based on this, they are classified into three types: **Stub AS**, **Multihomed AS**, and **Transient AS**. This 
#### Types of Autonomous Systems

This classification influences how the **interdomain routing protocols** function with respect to each AS. Autonomous Systems are not categorized by their size but by *how they connect and interact with other AS*s.
1. **Stub AS**
	- *Has only one connection to another AS.*
	- It can only initiate or terminate data traffic but cannot be used as a transit network for other ASs. In other words, data cannot pass through it to reach another AS.
	- **Example**: Customer networks that are either the source or the destination of data are typical examples of a stub AS.
2. **Multihomed AS**
	- It is *connected to more than one AS* but *does not allow transit traffic to pass through*. It has multiple connections for redundancy or better reliability, but its policies prevent it from being used as an intermediate AS for routing data between other ASs.
	- **Example**: Some customer ASs, such as businesses using multiple ISPs for redundancy, are multihomed ASs. They use the services of more than one provider network, but their configuration does not allow data to pass through their network.
3. **Transient AS**
	- It is *connected to multiple ASs* and *allows traffic to pass through*. These ASs serve as intermediary systems, enabling data to move from one AS to another.
	- **Example**: Provider networks and backbone networks that enable global connectivity and allow the flow of traffic between different ASs are classified as transient ASs.

#### Impact on Routing Protocols

- The type of AS affects the operation of the **interdomain routing protocol**, specifically **BGP (Border Gateway Protocol)**, which is responsible for routing traffic between ASs. 
	- Transient ASs, because they allow traffic to pass through, play a critical role in shaping the overall flow of Internet traffic across regions and networks. 
	- Stub and multihomed ASs have a more limited role, focusing on handling traffic for their own networks rather than facilitating global data transfer.

---
## 1. Routing Information Protocol

### Hop Count as a Metric

- In RIP, the cost between networks is measured in **hop count**, which refers to the number of networks (or subnets) a packet must traverse to reach its destination. 
- The **maximum hop count** allowed in RIP is 15. A hop count of 16 is considered **infinity**, meaning the destination is unreachable.
- The hop count metric simplifies routing calculations, but *it ignores other factors like delay or bandwidth*.
	![[Pasted image 20240930225438.png]]

### Forwarding Tables
	
![[Pasted image 20240930202805.png]]
- In RIP, routers maintain **forwarding tables** to forward packets to their destination networks. Each entry in the forwarding table includes:
	- **Destination Network**: The network address to which the packet is destined.
	- **Next Router**: The address of the next hop router on the path to the destination.
	- **Cost**: The hop count or number of hops required to reach the destination network.
- The forwarding table implicitly describes the **least-cost tree** through which packets will traverse to reach their destination.
   
### RIP Implementation

- RIP is implemented as a background process or **daemon** in Unix systems, called **routed** (route-daemon).
- RIP operates at the **application layer**, using **UDP** as its transport protocol on **port 520**. Although it is a routing protocol, RIP messages are encapsulated in **UDP datagrams**, which are further encapsulated in **IP packets**.
- RIP follows a **timed update** process where each router broadcasts its routing table to its neighbors periodically.
	![[Pasted image 20240930225335.png]]

### RIP Versions

1. **RIP-1**: 
	 - Was designed without support for **subnet masks**.
	 - *It was limited in terms of scalability and features, making it suitable for smaller networks.*
2. **RIP-2**: 
	- An improved version of RIP that is *backward compatible with RIP-1*. 
	- RIP-2 adds support for **subnet masks**, **authentication**, and **multicasting** of routing updates (instead of broadcasting). 
	- It offers better security and efficiency.

### RIP Message 

![[Pasted image 20240930220345.png]]
- The client and server processes exchange messages to share routing information.
- These messages follow a specific format and serve the purpose of requesting or sending updates about routing tables. 

#### RIP Message Structure

- *Multiple entries can be included in a single RIP-2 message.*
- Each entry corresponds to one line in the forwarding table.

#### Types of RIP Messages

1. **Request Messages**
	- A **request message** is sent by a router to *ask for routing information*. It is typically used when:
		- A router has just come online and needs to populate its routing table.
		- A router has **time-out entries**, meaning some of its routing information is outdated or lost.
	- *A request can ask for specific entries in the routing table or all entries from the neighboring router.*

2. **Response (Update) Messages**
	- A **response message** contains the requested routing information and can be either **solicited** or **unsolicited**.
		1. **Solicited Response**: This message is *sent in direct response to a request message*. It contains routing information for the specific destinations requested by the router.
		2. **Unsolicited Response**: This is *sent periodically (every* **30 seconds**) *or when there is a change in the forwarding table*. These periodic updates help ensure that all routers maintain up-to-date routing information without needing to ask for it.
The **Routing Information Protocol (RIP)** implements a modified version of the distance-vector routing algorithm, with additional features to manage routing information more effectively. Here’s how it works:

### Modifications to the Distance-Vector Algorithm in RIP

1. **Entire Forwarding Table Sent in Response Messages**:
	- In contrast to just sending distance vectors, a router using RIP sends the entire contents of its forwarding table in a **response message** to its neighbors.

2. **Modifying the Forwarding Table**:
	- When a router receives a response message (which contains the forwarding table of a neighboring router), it does the following:
		- **Increments each hop count** by 1 to account for the extra hop from the sending router.
		- **Changes the "next router" field** to the address of the sending router.
   
The router compares the modified forwarding table (received table) with its current forwarding table (old table) and updates its routes according to three rules:
1. If the **received route does not exist** in the old forwarding table, it adds the route.
2. If the **received route has a lower cost** than the existing route, the received route is selected as the new one.
3. If the **received route has a higher cost** but the next router is the same, the received route is still selected. This happens when a router advertises a route with a previously lower cost but now sends a cost of infinity (16 in RIP), indicating the route is no longer available.

4. **Forwarding Table Sorting**:
	- The updated forwarding table is sorted based on destination routes, often using the **longest prefix match** strategy.

### Timers in RIP

1. **Periodic Timer**:
	- Controls the *regular updates of the routing table*. It is set to a random value between **25 and 35 seconds** to prevent all routers from sending updates simultaneously, which could cause network congestion.
   
2. **Expiration Timer**:
	- Tracks the *validity of each route*. When an update is received for a route, its expiration timer is reset to **180 seconds**. If no update is received within that time, the route is marked as expired, and its hop count is set to 16 (unreachable).

3. **Garbage Collection Timer**:
	- *Once a route becomes invalid, it isn't immediately purged from the forwarding table*. Instead, the route is marked as unreachable (hop count 16), and the garbage collection timer is set to **120 seconds**. When this timer expires, the route is removed from the table.

### Performance of RIP

1. **Update Messages**:
	- RIP update messages are simple and *sent only to neighboring router*s, so they generally do not create excessive traffic. 
	- *Randomized periodic timers* also help minimize traffic bursts.
   
2. **Convergence of Forwarding Tables**:
	- RIP's use of the distance-vector algorithm can lead to **slow convergence**, particularly in larger networks. 
	- However, since RIP *limits the network diameter to* **15 hops**, convergence issues are less pronounced.
	- Problems like *count-to-infinity* and routing loops are mitigated using techniques like *poison reverse* and *split horizon*.

3. **Robustness**:
	- *RIP’s reliance on information from neighbors makes it vulnerable to failures or misconfigurations.* 
	- However, poison reverse and split horizon techniques help reduce the impact of such issues.

---
## 2. Open Shortest Path First (OSPF)
### Metric in OSPF
- OSPF assigns a cost to each link between routers. This cost can be based on various factors, including:
	1. **Throughput**
	2.  **Round-trip time (RTT)** 
	3.  **Reliability** 
	4. **Hop count** 
- **TOS (Type of Service)**: OSPF allows different costs for different types of service. This means that *traffic with different service requirements (such as delay-sensitive or bandwidth-heavy traffic) can take different paths*.
	![[Pasted image 20241002172915.png]]

### Forwarding Tables in OSPF
- Once OSPF routers discover the shortest paths using Dijkstra’s algorithm, they create forwarding tables.
- These tables define how to forward packets towards the destination. The paths are chosen based on the lowest overall cost.
- *The main difference between OSPF and RIP in this context is the cost calculation. While RIP uses a simple hop count, OSPF’s metric is more flexible and can vary based on the chosen parameters (e.g., throughput or RTT).*
	- If we use the hop count for OSPF, the tables will be exactly the same.

### OSPF Areas and Hierarchical Structure

The creation of areas allows OSPF to divide a large AS into *smaller, more manageable sections*, *reducing the overall traffic load caused by flooding link-state packets* (LSPs).

- **Area Structure:** OSPF divides the AS into multiple smaller sections called areas.
- **Backbone Area (Area 0):** There is always a designated backbone area (Area 0) responsible for *inter-area communication*. Routers within the backbone area ensure that *LSPs from one area are passed to all other areas*, thus facilitating the exchange of routing information across the entire AS.
- **Area Identification:** Each area has a unique area identification number. The backbone area is always assigned the ID "0."
	![[Pasted image 20241002194440.png]]

### Link-State Advertisements (LSAs)

*OSPF routers advertise the status of their links to neighboring routers* through Link-State Advertisements (LSAs). OSPF uses five types of LSAs, each tailored for different routing scenarios.

1. **Router Link LSA:**
	- Advertises the presence of a router as a node.
	- Defines the types of links connecting the router to other nodes:
		- **Transient Link:** Connects to a network that is *linked to other routers*.
		- **Stub Link:** Connects to a network that *does not forward traffic to other networks* (non-through network).
		- **Point-to-Point Link:** Connects *directly to another router*.

2. **Network Link LSA:**
	- *Advertises the network itself as a node.*
	- Since networks cannot advertise themselves, a **designated router** handles this task, announcing its own IP address and the addresses of all other routers on the network.

3. **Summary Link to Network LSA:**
	- Generated by an **Area Border Router (ABR)**.
	- Advertises the *summary of link-state information collected by the backbone to the individual areas or vice versa.* 
	- This type of LSA enables different *areas to communicate with each other* through the backbone area.

4. **Summary Link to AS LSA:**
	- Generated by an **Autonomous System Border Router (ASBR)**.
	- *Summarizes information about links from external ASs and advertises them to the backbone area of the current AS.* 
	- This helps in *exchanging routing information with other ASs*, which is essential in inter-AS routing using protocols like **BGP** (Border Gateway Protocol).

5. **External Link LSA:**
	- Generated by an **ASBR**.
	- *Advertises the presence of an external network outside the AS to the backbone area.* 
	- This information is then distributed to other areas within the AS.
	![[Pasted image 20241002194713.png]]

### OSPF Implementation

#### OSPF Integration with IP
- OSPF operates as a *separate protocol at the network layer*, but its *messages are encapsulated in IP datagrams*.
- The **protocol field** in the IP header for OSPF is set to **89**, indicating that the message is specifically for OSPF. This encapsulation allows OSPF messages to propagate through the network just like regular IP datagrams.
- OSPF has two versions: **Version 1** (obsolete) and **Version 2** (commonly used in most implementations).

#### OSPF Messages
- All OSPF messages share a **common header** format and, in some cases, a **link-state general header**.
- OSPF uses five different types of messages, each with a specific role in routing. 

1. **Hello Message (Type 1):**
	- Used by routers to introduce themselves to neighbors and announce the list of known neighbors.
	- This message initiates the *establishment of adjacency* between routers and is crucial for neighbor discovery.

2. **Database Description Message (Type 2):**
	- *Sent in response to the hello message.*
	- Helps a newly joined router to receive a summary of the link-state database (LSDB) from its neighbors.
	- *The new router uses this information to compare its LSDB with its neighbors and request specific missing LS entries if needed.*

3. **Link-State Request Message (Type 3):**
	- *Sent by a router to request detailed information about a specific link-state entry from a neighbor.*
	- This message is used when a router finds that it is missing some LS information after receiving a database description message.

4. **Link-State Update Message (Type 4):**
	- The primary message used to *update and synchronize the LSDB between routers*.
	- This message can contain *five different versions*, corresponding to the five types of LSAs (router link, network link, summary link to network, summary link to AS border router, and external link).
	- Ensures that routers have consistent and up-to-date link-state information.

5. **Link-State Acknowledgment Message (Type 5):**
	- Sent by a router to *acknowledge the receipt of a link-state update message*.
	- Provides reliability in OSPF by ensuring that link-state updates are received correctly by the intended routers.
	![[Pasted image 20241002194942.png]]

### Authentication
- The OSPF **common header** includes fields that allow for the **authentication** of the message sender.
- *Authentication helps protect the OSPF network from unauthorized entities sending malicious routing updates.* 
- Without authentication, a malicious actor could manipulate routing tables by sending false OSPF messages.

### OSPF Algorithm

- OSPF implements the **link-state routing algorithm** (based on Dijkstra’s algorithm) to calculate the shortest path between routers.
- However, the OSPF algorithm includes additional components to manage message exchange and routing table updates:

1. **Shortest-Path Tree Construction:**
	- After each router calculates the shortest-path tree using Dijkstra’s algorithm, it uses this tree to create a **routing table** that will guide the forwarding of packets.
2. **Message Handling:**
	- The *algorithm is augmented to manage the sending and receiving of all five message types* described above. This ensures that OSPF routers can effectively exchange routing information and maintain accurate LSDBs.

### Benefits of the Hierarchical Structure in OSPF
- **Efficient LSP Flooding:** By segmenting the AS into areas, OSPF minimizes the traffic overhead associated with flooding LSPs across the entire AS.
- **Scalability:** The area hierarchy allows OSPF to scale effectively in large, complex networks.
- **Backbone Connectivity:** The backbone area (Area 0) ensures inter-area routing while keeping each area's link-state database (LSDB) independent and manageable.

### Performance
-  **Update Messages:** They have a somewhat complex format. They also are flooded to the whole area. If the area is large, these messages *may create heavy traffic and use a lot of bandwidth*.
- **Convergence of Forwarding Tables:** When the flooding of LSPs is completed, each router can create its own shortest-path tree and forwarding table; convergence is fairly quick. However, *each router needs to run the Dijkstra’s algorithm, which may take some time*.
-  **Robustness:** The OSPF protocol is more robust than RIP because, after receiving the completed LSDB, each *router is independent* and does not depend on other routers in the area. Corruption or failure in one router does not affect other routers as seriously as in RIP.
---
