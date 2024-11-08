- In data communications, *switching at the network layer is primarily achieved through packet switching*. A router functions as a switch, connecting input ports to output ports, analogous to an electrical switch. 
- There are two main switching techniques: **circuit switching** and **packet switching**. 
- Only packet switching is used at the network layer, where *data is handled in the form of packets*.
	- At network layer, messages from the upper layers are divided into smaller, manageable packets. 
	- These packets are sent individually from the source to the destination, which waits to receive all packets before reconstructing the original message for the upper layer. 
#### 1. Datagram Approach: Connectionless Service
- Packet-switched networks employ two approaches to routing packets: the **datagram approach** and the **virtual-circuit approach**.
- Network-layer protocol treats each packet independently, with each packet having no relationship to any other packet. 
- Packets in a message may or may not travel the same path to their destination.
- Each packet is routed based on the information contained in its **header**: **source** and **destination addresses**.
- The source address may be used to send an error message to the source if the packet is discarded.
#### 2. Virtual-Circuit Approach: Connection-Oriented Service
- In a connection-oriented service, there is a *relationship between all packets belonging to a message*.
- A virtual connection is set up before sending all datagrams in a message to define their path.
- After the connection setup, datagrams follow the same path.
- Packets must contain the **source** and **destination addresses** and a **flow label (virtual-circuit identifier)**.
- The flow label defines the *virtual path for the packet*.
- Source and destination addresses are still kept by parts of the Internet at the network layer.
- Reasons for retaining addresses include:
	- Part of the packet path may use *connectionless service*.
	- The network layer *protocol is designed with these addresses and changing them may take time*.

