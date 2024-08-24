- Located between the network layer and the application layer. The transport layer is responsible for providing services to the application layer; it *receives services from the network layer*.
### Process-to-process Communication

- It provides **process-to-process communication**. 
- A **process** is an application-layer entity (running program) that uses the services of the transport layer.
 - Network layer: *host-to-host*
 - Transport layer: *process-to-process*
 ![[Pasted image 20240824114324.png]]
### Port Number

- **Local Host**: Client; **Remote Host**: Server
- **IP address**: Defines host on a network
- **Port Number**: Defines port number on a host
-  In the TCP/IP protocol suite, the port numbers are integers between 0 and 65,535 (16 bits).
- The client program defines itself with a port number, called the **ephemeral port number** (short lived). Greater than 1,023.
- Servers are assigned *Well known port numbers*,
![[Pasted image 20240824115659.png]]

### ICANN Ranges

- **Well-known ports**: The ports ranging from 0 to 1,023 are assigned and controlled by ICANN. These are the well-known ports.
- **Registered ports**: The ports ranging from 1,024 to 49,151 are not assigned or controlled by ICANN. They can only be registered with ICANN to prevent duplication.
- **Dynamic ports**: The ports ranging from 49,152 to 65,535 are neither controlled nor registered. They can be used as temporary or private port numbers.
![[Pasted image 20240824115643.png]]

### Socket Addresses

- The combination of an IP address and a port number is called a **socket address**. 
- The client socket address defines the client process uniquely just as the server socket address defines the server process uniquely.
- To use the services of the transport layer in the Internet, we need a pair of socket addresses: the client socket address and the server socket address. These four pieces of information are part of the network-layer packet header and the transport-layer packet header. The first header contains the IP addresses; the second header contains the port numbers.
![[Pasted image 20240824120005.png]]

### Encapsulation and Decapsulation
- Message from a process gets encapsulated with a pair of socket addresses and some other info.
- During decapsulation the header is dropped.
- Transport layer receives the data and adds the transport-layer header.
![[Pasted image 20240824120244.png]]

### Multiplexing and Demultiplexing

- Entity accepts items from more than one source, this is referred to as **multiplexing** (many to one). The transport layer at the source performs multiplexing.
- Entity accepts items from more than one source, this is referred to as **demultiplexing** (one to many). Transport layer at the destination performs demultiplexing.
![[Pasted image 20240824120632.png]]

### Flow Control
Whenever an entity produces items and another entity consumes them, there should be a balance between production and consumption rates.

- **Pushing**:  If the sender delivers items whenever they are produced without a prior request from the consumer. *Requires flow control*.
- **Pushing**:  If the producer delivers the items after the consumer has requested them. *Doesn't require flow control*.
![[Pasted image 20240824121845.png]]

#### Flow Control at the transport layer:
In the transport layer of communication, there are four main entities: 
**i. the sending process**, **ii. sending transport layer**, **iii. receiving transport layer**, and **iv. receiving process**.

- The **sending process** produces message chunks and hands them to the sending transport layer. This layer acts as both a *consumer (of messages from the sending process)* and a *producer (encapsulating messages into packets for transmission)*. 
- The **receiving transport** layer consumes these packets and decapsulates them to deliver the messages to the receiving process. This delivery to the application layer is typically *done on a pull basis*, meaning the transport layer waits until the application process requests the messages.
![[Pasted image 20240824122647.png]]

#### Buffers for flow control in transport layer:
- Buffers in flow control use memory locations to *manage packet transmission between sender and receiver*. 
- Two buffers are typically used: one at the *sender's transport layer* and one at the *receiver's transport layer*. 
	- When the sender's buffer is full, it signals the application layer to pause sending data. When it has space, it signals the application layer to resume sending. 
	- Similarly, when the receiver's buffer is full, it informs the sender to stop sending packets. Once the receiver's buffer has space, it allows the sender to continue sending packets.

### Error Control
- Error control at the transport layer is responsible for
	1. Detecting and discarding corrupted packets.
	2. Keeping track of lost and discarded packets and resending them.
	3. Recognizing duplicate packets and discarding them.
	4. Buffering out-of-order packets until the missing packets arrive.
- Error control, unlike flow control, involves only the sending and receiving transport layer.
#### Sequence Numbers
Each packet in the transport layer is assigned a sequence number, which helps in several ways:
1. **Resending Lost Packets:** If a packet is lost or corrupted during transmission, the receiver can request the sender to resend it by indicating the sequence number of the missing packet.
2. **Detecting Duplicate Packets:** Duplicate packets can be detected if the receiver receives two packets with the same sequence number.
3. **Handling Out-of-Order Packets:** The receiver can detect out-of-order packets by observing gaps or jumps in the sequence numbers.

To implement sequence numbers effectively:

- **Numbering Scheme:** *Packets are numbered sequentially starting from 0 up to \( 2^m - 1 \)* where \( m \) is the *number of bits allocated for the sequence number* field in the packet header.
- **Wrap-around:** Sequence numbers wrap around after reaching \( 2^m - 1 \), which means after reaching the maximum value, the sequence number resets to 0.
- **Modulo Operation:** Sequence numbers are managed using modulo \( 2^m \) arithmetic to ensure they stay within the defined range and handle wrap-around effectively.

For instance, if \( m = 4 \), the sequence numbers range from 0 to 15. After 15, the sequence number wraps around to 0, maintaining continuity.
![[Pasted image 20240824130727.png]]

![[Pasted image 20240824130738.png]]

#### Acknowledgement
- **Positive Acknowledgment (ACK):** The receiver sends an ACK for each successfully received packet or group of packets. This indicates to the sender that the packet(s) arrived correctly.
- **Handling Corrupted Packets:** Corrupted packets are discarded by the receiver.
- **Detecting Lost Packets:** The sender uses a timer for each packet sent. If an ACK is not received before the timer expires, the sender resends the packet.
- **Managing Duplicate Packets:** Duplicate packets are discarded by the receiver without any further action.
- **Handling Out-of-Order Packets:** The receiver can either:
  - Discard out-of-order packets, treating them as lost packets, or
  - Store them until the missing packets arrive and then process them in order.

### Combination of Flow and Error Control

Combining flow control and error control involves using numbered buffers and a sliding window approach to manage packet transmission and acknowledgment efficiently.

#### Numbered Buffers

- **Sender Side:**
  - **Storing Packet Copy:** When a packet is prepared for sending, it is assigned a sequence number based on the next available buffer location. This packet is then *stored in this memory location corresponding to its sequence number (numbered buffer)* until an acknowledgment (ACK) is received.
  - **ACK Handling:** Upon receiving an ACK, the corresponding buffer location is freed, allowing new packets to be sent.

- **Receiver Side:**
  - **Storing Packet Copy:** When a packet arrives, it is stored in the *memory location corresponding to its sequence number*. This storage persists until the application layer is ready to process the packet.
  - **ACK Sending:** An ACK is sent to confirm the receipt of the packet.

#### Sliding Window
![[Pasted image 20240824134419.png]]O

- The sequence numbers, managed using modulo \(2^m\), are visualized as a circular buffer. This *buffer is represented by* **sliding window** *showing the range of sequence numbers currently being managed*.
- The window size can vary depending on the specific protocol being used.
- **Sender Buffer Management:**
	- ***Marking and Unmarking***: As packets are sent, their corresponding positions in the sliding window are marked. When an ACK is received, the corresponding position is unmarked.
	- ***Window Movement:*** If a series of consecutive positions at the beginning of the window are unmarked (indicating successful acknowledgments), the window slides forward. This movement frees up space at the end of the window for new packets.

- **Example:** For \(m = 4\), the sequence numbers range from 0 to 15. If the window size is 7, the window might cover a subset of these numbers. Once packets are acknowledged and their positions are unmarked, the window slides to accommodate new packets.

![[Pasted image 20240824135107.png]]

### Congestion Control
- **Congestion** happens when the *number of packets sent to the network exceeds the network’s capacity to handle them*, causing *delays* and *packet loss*.
- **Congestion control** involves *techniques to ensure that the network load remains within the network’s handling capacity*, preventing performance degradation and packet loss. 
- Congestion happens in any system that involves waiting. *Routers and switches have input and output queues to hold packets*. If these queues overflow due to faster incoming packet rates than processing rates, congestion occurs.
- Congestion at the transport layer is actually the result of congestion at the network layer, which manifests itself at the transport layer. 

### Connectionless and Connection-Oriented Services
- Connectionless service at the transport layer means independency between packets.
- Connection-oriented means dependency.
#### Connectionless Service
- **Independence of Packets:** Each packet is treated independently. The transport layer does not maintain any relationship between packets.
- **Process:**
  - The source application divides a message into chunks and sends each chunk to the transport layer.
  - The transport layer encapsulates and sends these chunks as packets without ensuring their order.
  - Packets may arrive out of order, and there is no inherent mechanism to detect or correct this at the transport layer.
- **Challenges:**
	- ***Out-of-Order Delivery:*** Packets can arrive at the destination in a different order than they were sent.
	- ***Packet Loss:*** If a packet is lost, there is no inherent mechanism to detect its loss or request its retransmission. 
	- ***No Flow, Error, or Congestion Control:*** Connectionless services generally do not provide mechanisms for flow control, error control, or congestion control.

![[Pasted image 20240824153216.png]]
#### Connection-Oriented Service
- **Dependence and Coordination:** A logical connection is established between the client and server before any data exchange. This connection ensures that packets are delivered in order and errors can be managed.
- **Process:**
	- ***Connection Establishment:*** The client and server establish a connection before data is exchanged.
	- ***Data Transfer:*** Data is sent in a coordinated manner, ensuring packets are delivered in the correct order and handling retransmissions if needed.
	- ***Connection Tear-Down:*** After data transfer is complete, the connection is properly terminated.
	- ***Flow, Error, and Congestion Control:*** Connection-oriented services allow for the implementation of flow control (managing the rate of data transmission), error control (detecting and correcting errors), and congestion control (preventing and managing network congestion).
- **Network Layer vs. Transport Layer:** 
  - At the network layer, connection-oriented service involves coordination between all routers and end hosts.
  - At the transport layer, connection-oriented service is end-to-end, involving only the two end hosts.
- **Protocol Flexibility:** A connection-oriented protocol at the transport layer can be implemented over either connectionless or connection-oriented network layer protocols.

![[Pasted image 20240824153419.png]]
#### Finite Sate Machine
An FSM represents a protocol's behavior as a machine with a finite number of states. The machine transitions from one state to another based on events, executing specific actions during these transitions.

- **Components:**
  - **States:** Represent different conditions or statuses of the protocol.
  - **Events:** Triggers that cause the FSM to transition from one state to another.
  - **Actions:** Operations performed during state transitions.
  - **Initial State:** The starting state of the FSM when it is activated.

##### Connectionless Transport Layer

- **Single State:** The FSM for a connectionless transport layer has only one state, typically called the "established" state. In this state:
  - ***Always Ready:*** The protocol is ready to send and receive packets without maintaining any connection state or order.
  - ***No Connection Setup:*** There is no need for connection establishment or teardown procedures.

##### Connection-Oriented Transport Layer

1. ***Closed State:*** The initial state where no connection exists. It waits for a request to open a connection.
2. ***Open-Wait-I State:*** When a connection request is made, the FSM moves to this state and sends an open request packet to the remote end.
3. ***Open-Wait-II State:*** After receiving an acknowledgment from the remote end, the FSM moves to this state. It waits for the remote end to complete the connection request if a bidirectional connection is needed.
4. ***Established State:*** Once both ends agree on the connection, the FSM transitions to the established state, allowing data exchange.

![[Pasted image 20240824160830.png]]
- **Connection Teardown:**
5. ***Close-Wait-I:*** When a connection needs to be terminated, a close request packet is sent, and the FSM moves to the close-wait-I state.
6. ***Close-Wait-II State:*** After receiving an acknowledgment from the remote end, the FSM moves to this state and waits for a close-request packet from the other end.
7. ***Closed State:*** Upon receiving the close-request packet, the FSM sends an acknowledgment and transitions back to the closed state.