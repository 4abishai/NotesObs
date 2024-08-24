1. Communication in distributed systems relies on message passing due to the absence of shared memory.

2. The OSI model was developed to standardize communication protocols between different systems.

3. The OSI model consists of seven layers, each handling specific aspects of communication:
   - Each layer provides services to the layer above and uses services from the layer below.
   - *Layers can be modified independently*, making the model flexible and adaptable.

4. When a process on one machine communicates with a process on another:
   - The message passes down through the layers on the sending machine, with each layer adding its own header.
   - On the receiving machine, the message passes up through the layers, with each layer examining and stripping off its header.

5. The model distinguishes between connection-oriented and connectionless protocols:
   - Connection-oriented protocols establish a connection before data exchange (e.g., telephone calls).
   - Connectionless protocols don't require connection setup (e.g., dropping a letter in a mailbox).

6. The OSI model allows for standardization and interoperability between different systems and technologies.

7. The collection of protocols used in a particular system is called a *protocol suite* or *protocol stack*.
![[Pasted image 20240819234056.png]]
### 1. Physical Layer

1. The Physical Layer is responsible for transmitting raw bits (0s and 1s) over a communication channel.
2. Key concerns of this layer include:
   - Voltage levels used to represent 0 and 1
   - Data transmission rate (bits per second)
   - Whether transmission can occur in both directions simultaneously
   - Physical characteristics of the network connector (plug), including its size, shape, and pin configuration
3. The Physical Layer protocol standardizes various aspects of the physical connection:
   - Electrical specifications
   - Mechanical specifications
   - Signaling interfaces
4. Its goal is to ensure that when one machine sends a 0 bit, it's received as a 0 bit by the other machine, not as a 1 bit.
5. Multiple Physical Layer standards have been developed for different types of media.
6. An example given is the RS-232-C standard, which is used for serial communication lines.

### 2. Data Link
- The Data Link Layer's main task is to detect and correct errors that may occur during transmission, as real communication networks are prone to errors.
-  **Framing**: It groups bits from the Physical Layer into units called frames.
-  The layer adds *special bit patterns at the start and end* of each frame to mark them.
- It computes a **checksum** by *adding up all the bytes in the frame in a specific way* and appends it to the frame. When a frame arrives, the receiver recomputes the checksum and compares it to the transmitted checksum. If they match, the frame is accepted; if not, the receiver requests re-transmission.
- Frames are assigned *sequence numbers in the header to keep track of them*.
-  **Header field**: These requests, responses, and parameters (like frame numbers) are defined in the header field.

### 3. Network Layer
- The Network Layer is responsible for routing messages from sender to receiver *across the network*.
- On a LAN, routing is simple as the receiver can directly take the message. On a WAN, routing becomes more complex.
- Messages may need to make multiple hops to reach their destination. *Choosing the best path is the primary task of this layer*.
- The shortest route isn't always the best. Factors like delay, traffic, and congestion are considered.
-  Some algorithms adapt to changing network loads, while others use long term averages.
-  Protocol types: Two main types are described:
   i. **Connection-oriented** (X.25): Used by *public networks*, requires setup with  a `Call Request`.
   ii. **Connectionless** (IP): Part of the US *Dept. of Def.* protocol suite, doesn't require setup.
-  **IP (Internet Protocol)**: Allows sending packets without prior setup. Each packet is routed independently, with no fixed path established.
-  Terminology: Messages at this layer are called **packets** (specifically "IP packets" for the Internet Protocol).

### 4. Transport Layer
   - Provides reliable connection between sender and receiver
   - Breaks messages from the session layer into smaller packets
   - Assigns sequence numbers to packets before sending
   - Packets can be lost during transmission
   - The layer *ensures packets arrive in the correct sequence*
   - It can handle out-of-order packet arrivals
   - Connection-oriented by definition
   - Maintain the illusion of an undamaged, ordered data stream
   - ISO transport protocol has *five variants (TP0 through TP4)*
   - DoD transport protocol is TCP (Transmission Control Protocol)
   - UDP (Universal Datagram Protocol) is a connectionless alternative
   - TCP/IP is widely used in universities and UNIX systems
   - UDP is used for programs that don't need connection-oriented protocols

### 5. Session Layer
- It's described as an enhanced version of the transport layer.
- Provides **dialog control** to *track which party is currently communicating*.
- Offers **synchronization** facilities.
   - Allows users to insert *checkpoints* into long transfers.
   - Enables recovery from crashes by *returning to the last checkpoint* rather than starting over from the beginning.
- Few applications are interested in the session layer.
- It is rarely supported in practice.
   - The session layer is not present in the DoD (Department of Defense) protocol suite.

### 6. Presentation Layer
- Unlike lower layers that focus on reliable and efficient bit transmission, the presentation layer is concerned with the **meaning of the data**.
- Most messages are not random bit strings, but *structured information*.
	- Examples include people's names, addresses, and amounts of money.
- The presentation layer *allows for the definition of records* containing specific fields.
- It enables the sender to notify the receiver about the format of a message.
- This layer makes it easier for *machines with different internal representations to communicate*.

### 7. Application Layer
- It's described as a collection of miscellaneous protocols for **(high level) common activities**.
	- Electronic mail
	- File transfer
	- Connecting remote terminals to computers over a network
-  Notable protocols:
	- X.400 electronic mail protocol
	- X.500 directory server