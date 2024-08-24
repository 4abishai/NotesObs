 It is a *hierarchical protocol* made up of *interactive modules*, each of which provides a specific functionality. 

- *The router is involved only in three layers*; there is no transport or application layer in a router as long as the router is used only for routing. Although a *router* is always involved in *one network layer*, it is involved in *n combinations of link and physical layers* in which n is the number of links the router is connected to. The reason is that *each link may use its own data-link or physical protocol*.
- The duty of the *application, transport, and network* layers is *end-to-end*.
- The duty of the *data-link* and *physical layers* is *hop-to-hop*, in which a hop is a host or router. 
- The domain of duty of the top three layers is the *internet*, and the domain of duty of the two lower layers is the *link*.
-  In the *top three layers*, the data unit (packets) should *not be changed by any router or link-layer switch*. In the bottom *two layers*, the packet created by the host is *changed only by the routers*, not by the link-layer switches.
- The link between two hops does not change the object.
![[Pasted image 20240811225537.png]]

1. **Physical Layer**: 
   - Responsible for *transmitting individual bits in a frame* across the link.
   - It is the lowest layer, working beneath the transmission media.
   - It is physical as there is still hidden *transmission layer*.

2. **Data-Link Layer**:
   - Moves datagrams across a link (LANs and WANs) determined by routers, when the next link to travel is determined by the router.

3. **Network Layer**:
   - Establishes *host-to-host* connections.
   - Routers select the *best path* for packet delivery.

4. **Transport Layer**:
   - Also provides end-to-end communication between source and destination hosts.
   - Encapsulates messages from the application layer into transport-layer packets (called a *segment* or a *user datagram* in different protocols).
   - Services the application layer by delivering messages to corresponding applications on the destination host.

5. **Application Layer**:
   - The logical connection between the two application layers is end-to-end.
   - Communication at the application layer is between two processes (two programs running at this layer). 
   - To communicate, a process sends a request to the other process and receives a response.
![[Pasted image 20240811225632.png]]
![[Pasted image 20240811230110.png]]
![[Pasted image 20240811230159.png]]
