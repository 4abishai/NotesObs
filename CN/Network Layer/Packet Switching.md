   - At the network layer, routers act as switches, connecting an input port to an output port, similar to electrical switches.
- **Types of Switching**:
	 - **Circuit Switching** (used at the physical layer) is not employed at the network layer.
	 - **Packet Switching** is used at the network layer, as data is handled in packets.
- Messages are divided into smaller packets by the source.
- Packets are sent one by one and reassembled at the destination before delivering the full message to the upper layer.
- **Two approaches can be used for routing packets**
	 - **Datagram Approach**.
	 - **Virtual Circuit Approach** (both will be discussed later).

## Datagram Approach: Connectionless Service
   - The network layer provides a connectionless service where each packet is treated independently.
   - Packets may travel different paths to reach the destination.
   - The network layer only ensures that packets are delivered, without guaranteeing a specific path.
   - *Each packet is routed based on its destination address in the header.*
   - The source address is used only to send error messages if needed (e.g., if a packet is discarded).
   - In a connectionless service, routers act as switches, making decisions for each packet individually based on its header information.
   ![[Pasted image 20240924203107.png]]
   ![[Pasted image 20240924203134.png]]

## Virtual-Circuit Approach: Connection-Oriented Service
- A relationship is established between packets belonging to the same message.
- A virtual connection (path) is set up before sending packets, ensuring all packets follow the same path.
- Each packet contains a **flow label (virtual circuit identifier)** that defines the path.
- Source and destination addresses are still present, but *routing is primarily based on the label*.
![[Pasted image 20240924205953.png]]
![[Pasted image 20240924205857.png]]
- **Three-Phase Process**:
	- **Setup Phase**: 
		 - A virtual circuit is established.
		 - A request packet (with source and destination addresses) is sent from the source to the destination. The router creates an entry in its table for this virtual circuit, but it is only able to fill three of the four columns.
		 ![[Pasted image 20240924210227.png]]
		 - The destination responds with an acknowledgment packet, it completes the entries in the switching tables. ![[Pasted image 20240924210356.png]]
	- **Data-Transfer Phase**: 
	 - Packets are sent along the established virtual circuit.
	 - Routers update the flow label as packets are forwarded, and all packets follow the same route.
	- **Teardown Phase**: 
	 - After data transfer, a teardown packet is sent by the source to terminate the connection.
	 - The destination confirms, and routers delete the virtual circuit entries.
