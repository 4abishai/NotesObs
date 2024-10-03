- In an internet, the goal of the network layer is to deliver a datagram from its source to its destination or destinations. 
	- **Unicast Routing:** If a datagram is destined for only one destination (one- to-one delivery).
	- **Multicast Routing:** If the datagram is destined for several desti- nations (one-to-many delivery).
	- **Broadcast Routing:** If the datagram is supposed to be delivered to all hosts in the internet (one-to-all).
- In unicast routing, a packet is routed, hop by hop, from its source to its destination by the help of forwarding tables.
- The source host needs no forwarding table because it delivers its packet to the default router in its local network. The destination host needs no forwarding table either because it receives the packet from its default router in its local network. *This means that only the routers that glue together the networks in the* *internet need forwarding tables.* 
## Internet as Graph
- In computer networking, an internet can be represented as a graph, where **nodes** are routers and **edges** are the networks that connect these routers. This type of modeling allows for the determination of the best route between any two routers using graph theory, where **least-cost routing** is one of the key strategies.
- **Weighted Graph Representation**:
	- An internet is modeled as a weighted graph.
	- Each **node** in the graph is a router, and each **edge** between nodes represents a network connecting two routers.
	- The **weight** of an edge corresponds to the "cost" of traversing that network, which can represent metrics like *bandwidth, delay, or a combination of other factors.*
### Least-Cost Routing
- *In a weighted graph, the best route from a source to a destination is the one that has the least cumulative cost.*
	![[Pasted image 20240928121909.png]]
- The least-cost route between router A and router E is through B (A → B → E), with a total cost of 6.
### Least-Cost Trees
- If there are **N routers**, the shortest route from each router to all other routers needs to be determined. This leads to the idea of a **least-cost tree**.
- *A least-cost tree is essentially a tree graph where the source router is the* **root**, *and the shortest path (least-cost route) is established between the root and all other nodes.*
- Instead of calculating N × (N - 1) individual routes, each node can maintain one least-cost tree that covers all destinations.
#### Properties of Least-Cost Trees:
![[Pasted image 20240928123848.png]]
1. **Symmetry**: 
	- The least-cost route from X to Y in X’s tree is the inverse of the least cost route from Y to X in Y’s tree.
	- The least-cost route between two routers is the same in both directions. 
	- For example, the route from **A to F** in **A's tree** (A → B → E → F) has a cost of 8, and the route from **F to A** in **F's tree** (F → E → B → A) has the same cost.
2. **Path Equivalence Across Trees**: 
	- Instead of travelling from X to Z using X’s tree, we can travel from X to Y using X’s tree and continue from Y to Z using Y’s tree.
	- Instead of always traveling through the source router’s tree, you can travel through intermediate trees. 
	- For example, you can go from A to G in **A's tree** directly (A → B → E → F → G), or you can travel to E in **A's tree** (A → B → E) and continue in **E's tree** from E to G (E → F → G). The total cost is the same, 9 in this case.
## Routing Algorithms
### 1. Distance-Vector Routing
- **Distance-Vector (DV) routing** algorithm is built on the concept of exchanging information between routers to eventually find the shortest paths across the network.
#### Process of Distance-Vector Routing:
1. **Initial Least-Cost Trees**:
	- *Each router starts by creating a rudimentary least-cost tree, based only on information about its* **immediate neighbors**.
	- These initial *trees are incomplete* because they don’t have knowledge about the whole network yet.
2. **Information Exchange**:
	- Routers continuously share their knowledge (distance vectors) with their ***immediate neighbors***.
	- *Each router updates its own view of the network based on information received from neighbors, gradually building a more complete view.*
3. **Building a Complete Tree**:
	- By repeatedly exchanging information and updating distance vectors, each router progressively builds a **complete least-cost tree**, representing the shortest paths to every other router in the network.
	![[Pasted image 20240928205842.png]]
#### Bellman-Ford Equation:
The **Bellman-Ford equation** forms the mathematical foundation of DV routing. It calculates the shortest path from a source node **x** to a destination node **y**, possibly through several intermediary nodes like **a**, **b**, or **c**.

![[Pasted image 20240928140721.png]]
$$
D_{xy} = \min \{ (c_{xa} + D_{ay}), (c_{xb} + D_{by}), (c_{xc} + D_{cy}), \dots \}
$$
- D_{xy}  is the shortest distance (or least cost) between nodes **x** and **y**.
- c_{xa} is the cost between node **x** and intermediary node **a**.
- D_{ay} is the shortest distance from intermediary node **a** to the destination **y**.
#### Simplified Bellman-Ford Equation:
To update the least-cost path through new intermediary node **z**, the equation simplifies to:
![[Pasted image 20240928140744.png]]
$$
D_{xy} = \min \{ D_{xy}, (c_{xz} + D_{zy}) \}
$$

This version is used when updating existing routes with a new path through **z** if it results in a shorter distance.
#### Concept of Distance Vectors:
![[Pasted image 20240928145620.png]]
- **Distance vector** *is essentially a list that contains the shortest distances (or least costs) from one router to every other router in the network.*
- Each router maintains a distance vector, which it shares with its immediate neighbors.
- *Using the Bellman-Ford equation, routers update their distance vectors whenever they receive new information from their neighbors.*
- DV represents a simplified version of the least-cost tree for a node in a network.
- DV only provides the minimum cost (or distance) from the current node (root) to every other node (destination) in the network.
- **Initial Creation**:
	- Each node in the network begins by creating an initial **rudimentary distance vector**. This vector only includes information about the **immediate neighbors** of the node. The node sends out greeting messages to identify its neighbors and records the distance between itself and each one.
	- All non-neighbor nodes are assigned an initial distance value of **infinity**, indicating that the node does not yet know how to reach those other nodes.
- **Distance Vector Structure**:
	- A distance vector is essentially a **one-dimensional array**. The index of each cell represents a specific destination (node), and the value in the cell represents the **least cost** (or shortest distance) to that destination.
	- Unlike a least-cost tree, a distance vector does not show the full path to each destination; it only shows the **minimum cost** to reach it.
#### Exchanging Distance Vectors:
- Once each node has created its rudimentary distance vector, it shares a **copy of its vector** with all of its immediate neighbors.
- As nodes receive distance vectors from their neighbors, they use the **Bellman-Ford equation** to update their own vectors. For every neighbor, the node checks if the cost to reach a destination through that neighbor is **shorter** than the current known cost.
   $$
   D_{xy} = \min \{ D_{xy}, (c_{xz} + D_{zy}) \}
   $$
   - D_{xy} is the current known shortest distance from node **x** to destination **y**.
   - c_{xz} is the cost to reach the neighbor node **z** from node **x**.
   - D_{zy} is the shortest distance from **z** to destination **y** (as reported by **z** in its distance vector).
![[Pasted image 20240928150241.png]]
- Each time a node updates its distance vector, it **immediately sends** the updated vector to its neighbors, allowing the network to propagate new information as quickly as possible.
- This process continues until the network converges, allowing all nodes to know the shortest paths to all other nodes in the network.
#### Convergence:
- Over time, through multiple exchanges of distance vectors between neighbors, each node in the network will gradually refine its distance vector. 
- The system will eventually **stabilize**, meaning that each node will have the correct least-cost distance to every other node in the network.

#### Algorithm
![[Pasted image 20240928210757.png]]
#### Count to Infinity Problem in Distance-Vector Routing
-  Arises when a link in the network fails, and the nodes take too long to realize that a particular destination is no longer reachable. 
- While **good news** (a decrease in cost) spreads quickly through the network, **bad news** (an increase in cost, especially when a link breaks) can propagate slowly, causing instability in the routing tables.
##### Why It Happens:
- When a link fails, the router directly connected to the failure should update its routing table to reflect that the destination is unreachable, setting the distance to **infinity**.
- However, due to the nature of distance-vector routing, if other routers have not yet been updated, they might still advertise incorrect routes, leading routers to believe the destination is still reachable through an alternative (but faulty) path.
- This faulty path may continuously be used until multiple updates occur, during which the cost for the failed route slowly increases until all nodes realize the destination is unreachable, eventually reaching infinity.
##### Example
![[Pasted image 20240928233633.png]]
1. **Link Failure**:
	- The link between **A** and **X** fails. Router **A** immediately updates its routing table to reflect that **X** is unreachable, setting the cost to **infinity**.
2. **Instability**:
	- Before **B** learns about the link failure, it sends its routing table to **A**. Since **B** still believes that it can reach **X**, it advertises a path to **X** with the old cost (which is now outdated).
	- **A**, assuming that **B** has found an alternative route to **X**, updates its table and changes the cost to reflect that **X** can be reached via **B**. This is incorrect but happens because **A** trusts **B**'s advertisement.
3. **Feedback Loop**:
	- Now **A** sends its new routing table to **B**, claiming that it can reach **X** through **B**. This creates a loop because **B** similarly updates its table, thinking **A** has found a new route to **X**.
	- As a result, packets destined for **X** bounce back and forth between **A** and **B** because **A** thinks **B** has a route to **X** and **B** thinks **A** has a route to **X**.
4. **Count to Infinity**:
	- With each exchange of routing updates, the cost to reach **X** slowly increases for both **A** and **B**, until it eventually reaches **infinity**. At this point, both nodes realize that **X** is no longer reachable, and the system stabilizes.
	- However, during the time it takes for the cost to reach infinity, the routing tables are unstable, and packets destined for **X** are caught in a loop.
#### Split Horizon Technique
- *Prevents a router from sending routing information back in the direction from which it came.* Specifically, when a router determines the best path to a destination through a specific neighbor, it avoids advertising that route back to that neighbor. 
- This prevents redundant or misleading updates that could lead to routing loops or incorrect information propagation.
##### How it Works:
Consider a simple scenario with nodes **A**, **B**, and **X**:
- Initially, both nodes **A** and **B** have routes to node **X**.
- Suddenly, the link between **A** and **X** fails. Without split horizon, **A** would update its table to reflect that **X** is unreachable, but **B** might send an update back to **A**, suggesting it knows a route to **X** via **A** (even though the link has failed). This would create confusion and a potential loop.
##### With split horizon
1. When **A** detects the failure of the link to **X**, it updates its table to mark **X** as unreachable (cost set to infinity).
2. **B**, upon receiving **A**'s updated table, should ideally correct its table as well, but without split horizon, it might send its own old table back to **A**, suggesting a faulty route.
3. **Split horizon** prevents **B** from sending its route to **X** back to **A** because **B** knows it learned about **X** from **A** originally. Therefore, **B** avoids advertising routes for **X** to **A**.
4. This ensures that **A** correctly maintains the infinity cost for **X**, and eventually, **B** will also update its table, making the network stable after just one update cycle.
##### Benefits of Split Horizon:
- **Faster Convergence**: It helps the network converge to the correct routing state faster when there is a change in topology, especially when a link failure occurs.
- **Prevents Loops**: It directly tackles the **two-node loop problem** that can occur when two nodes continuously advertise incorrect routes to each other.
- **Less Confusion**: It minimizes the chance of routers sending outdated or incorrect routing information to their neighbors.
#### Poisoned Reverse
- In the **split horizon** strategy, a router avoids advertising routes back to the router from which it learned them. However, this can create ambiguity because a router may not be sure if the silence regarding a route is due to split horizon or because the route no longer exists. 
- **Poisoned reverse** *strategy addresses this issue by sending updates with the route cost explicitly set to* **infinity**. 
	- This tells the neighboring router, "I learned this route from you, but now I can't use it, and neither should you." This ensures that there is no confusion about the status of a route and helps prevent routing loops.
##### How it Works:
1. Initially, **A** and **B** both have routes to **X**, and they regularly exchange distance vectors.
2. If **A**'s direct link to **X** fails, **A** updates its distance to **X** as infinity and informs **B**.
3. When **B** receives this update, without poisoned reverse, it would typically stop advertising the route to **X** to **A** (split horizon). However, **A** might not know if this lack of advertisement is due to split horizon or if **B** also lost its route to **X**.
4. With **poisoned reverse**, **B** explicitly sends a route to **X** back to **A** but with the distance set to infinity. This indicates to **A** that **B**'s route to **X** is no longer valid, preventing confusion.
5. Now both **A** and **B** will set their distances to **X** as infinity, quickly stabilizing the system and avoiding any loops.
##### Drawback:
- It can increase the size of routing updates because it still sends information (even if it's infinity), which wouldn't happen in split horizon where no update is sent.
### 2. Link-State Routing
- *It provides a complete map of the network, where each router has detailed information about the entire network's topology.* 
	- This is achieved through the collection and sharing of **link-state information**.
#### Link-State Database (LSDB)
   - **Link-State Database** *(LSDB) contains a complete view of the network's topology, including all links and their associated costs.*
   - The LSDB is a shared database, meaning that every node in the network maintains an identical copy of it. 
   - The LSDB is represented as a **two-dimensional matrix**, where each cell corresponds to the cost of the link between two nodes. 
	   - If the cost is infinity, the link is either broken or doesn't exist.
	   ![[Pasted image 20240928233716.png]]
#### To create graph (LSDB)
##### 1. Flooding and Link-State Packets (LSP)
   - To build the LSDB, each node sends out **Link-State Packets (LSPs)** to its direct neighbors.
   - The LSP contains two key pieces of information: 
     1. The **identity** of the neighboring node.
     2. The **cost** of the link connecting the two nodes.
   - *These LSPs are flooded throughout the network, ensuring that every router receives the LSPs of all other nodes.*
   - Flooding occurs through a controlled process:
     - A node compares any incoming LSP with its current copy. *If the incoming LSP is newer (determined by sequence numbers), it keeps the new LSP and discards the older one.*
     - The LSP is then forwarded to all other neighbors (except the one from which it was received), ensuring that the LSP spreads throughout the network.
   - This process stops when nodes with only one connection receive the LSPs, and eventually, every node will have the full set of LSPs required to build the LSDB.
##### 2. Formation of Least-Cost Trees (Using Dijkstra's Algorithm):
- Once a node has built its LSDB, it can create a **least-cost tree** using the **Dijkstra Algorithm** to find the shortest paths from a source node to all other nodes in the network.
1. **Initialization:**
	- The node begins by creating a tree with just itself and sets the cost to its direct neighbors.	
2. **Expand the Tree Iteratively:**
	- It then looks at all nodes it can reach, selecting the node with the lowest cost.
	- After adding that node to the tree, it checks if the addition of that node provides a cheaper route to other nodes. If so, it updates the costs for those nodes.
3. **Repeat:**
	- This continues until the tree contains all nodes in the network, ensuring that the **least-cost paths** to every node have been identified.
	
![[Pasted image 20240928234928.png]]
#### Comparison with Distance-Vector Routing
   - **Distance-Vector Routing:** In this algorithm, each router informs its **neighbors** about what it knows regarding the whole network.
   - **Link-State Routing:** Each router informs the **entire network** about what it knows about its **neighbors** (flooding).
   ![[Pasted image 20240929231309.png]]
#### Advantages of Link-State Routing:
1. **Accuracy:** Each node has a full view of the network, which leads to more accurate and reliable routing decisions compared to distance-vector routing.
2. **Fast Convergence:** Changes in the network (e.g., broken links) propagate quickly, as all nodes receive updated LSPs and can recompute their least-cost trees immediately.
3. **Loop-Free Routes:** Since each node uses the same LSDB and runs the same algorithm, the paths computed are inherently loop-free.
#### Drawbacks:
1. **Memory and CPU Requirements:** Since each router maintains a full copy of the LSDB and runs computationally intensive algorithms like Dijkstra's, link-state routing requires more *memory and processing power*.
2. **Complexity in Large Networks:** In very large networks, maintaining and distributing the LSDB can become resource-intensive.
#### Dijkstra’s Algorithm
![[Pasted image 20240928235037.png]]