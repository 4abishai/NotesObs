- Standardizes communication protocols between different systems.

- The OSI model consists of seven layers, each handling specific aspects of communication:
   - Each layer provides services to the layer above and uses services from the layer below.
   - *Layers can be modified independently*, making the model flexible and adaptable.

-  When a process on one machine communicates with a process on another:
   - The message passes down through the layers on the sending machine, with each layer adding its own header.
   - On the receiving machine, the message passes up through the layers, with each layer examining and stripping off its header.

-  The collection of protocols used in a particular system is called a *protocol suite* or *protocol stack*.
![[Pasted image 20240819234056.png]]
### 1. Physical Layer

- **Transmission Media:** Determines the type of medium used for communication (e.g., twisted-pair cables, coaxial cables, fiber optics, or wireless).
- **Bit Representation:** Defines how bits are represented on the physical medium, such as using electrical voltage levels, light pulses, or radio waves.
- **Data Rate:** Determines the rate at which data is transmitted over the medium, often measured in bits per second (bps).
- **Signal Encoding:** Converts data into a format suitable for transmission over the physical medium.
- **Synchronization:** Ensures that the sender and receiver are synchronized so that data can be correctly interpreted.
- **Topology:** Defines the physical layout of the network (e.g., bus, star, ring).
- **Physical Interfaces:** Specifies the mechanical, electrical, and procedural interface between the physical medium and network devices, such as connectors, pins, and wiring standards.
  
### 2. Data Link
- **Framing:**
	- The data link layer encapsulates network layer data packets into frames. A frame includes the data payload, along with header and trailer information necessary for error detection and control.
	-  The layer adds *special bit patterns at the start and end* of each frame to mark them.
	- It computes a **checksum** by *adding up all the bytes in the frame in a specific way* and appends it to the frame. When a frame arrives, the receiver recomputes the checksum and compares it to the transmitted checksum. If they match, the frame is accepted; if not, the receiver requests re-transmission.
	- Frames are assigned *sequence numbers in the header to keep track of them*.
- **Error Detection and Correction:** 
	- Common methods include checksums, cyclic redundancy checks (CRC), and, in some cases, automatic repeat requests (ARQ).
- **MAC (Media Access Control):** 
	- The MAC sublayer within the data link layer is responsible for addressing and controlling access to the physical medium. It assigns MAC addresses to devices, which are used to *identify the source and destination of data frames.*

### 3. Network Layer (Hop-by-Hop communication)
- The Network Layer is responsible for routing messages from sender to receiver *across the network*.
- Messages may need to make multiple hops to reach their destination. *Choosing the best path is the primary task of this layer*.
	- The shortest route isn't always the best. Factors like delay, traffic, and congestion are considered.
- **Logical Addressing**: Assigns and manages IP addresses (IPv4 or IPv6), which are used to identify devices on a network.
	-  ***IP (Internet Protocol):*** Allows sending packets without prior setup. Each packet is routed independently, with no fixed path established.
- **Packet Forwarding**: Transmits data packets between different networks, ensuring that they reach the correct destination.
-  Protocol type:
	1. ***Connection-oriented*** (X.25): Used by *public networks*, requires setup with  a `Call Request`.
	2. ***Connectionless*** (IP): Part of the US *Dept. of Def.* protocol suite, doesn't require setup.
- Handles the *data's journey across individual network segments, known as "hops."* Each *hop represents the movement of a packet from one router to another* as it traverses the network.
### 4. Transport Layer (End-to-End communication)
- **Segmentation and Reassembly**: Dividing *larger data from the application layer into smaller segments for transmission*, and reassembling them at the destination.
- **Connection Establishment and Termination**: Managing the start and end of communication sessions between devices.
- **Flow Control**: Regulating the data flow to prevent congestion and ensure efficient transmission.
- **Error Detection and Correction**: Ensuring data integrity by detecting and correcting errors in transmission.
- **Multiplexing**: Allowing *multiple applications to share a single network connection* by distinguishing between different sessions.
- **Protocols**: TCP and UDP.
- Manages the entire journey of a data segment from source to destination.
### 5. Session Layer
- **Session Management**: Between two devices
	- **Session*** is a connection that allows the two device to exchange data over a period of time.
- Provides **dialog control** to *track which party is currently communicating*.
- Offers **synchronization** facilities.
	- Allows users to insert *checkpoints* into long transfers.
	- Enables recovery from crashes by *returning to the last checkpoint* rather than starting over from the beginning.
- It is rarely supported in practice.
- The session layer is not present in the DoD (Department of Defense) protocol suite.

### 6. Presentation Layer
- Unlike lower layers that focus on reliable and efficient bit transmission, the presentation layer is concerned with the **meaning of the data**.
- **Serialization**: The Presentation Layer *translates data between the formats* the network uses and the formats the application expects.
- This layer is responsible for data encryption and decryption.
- It enables the sender to notify the receiver about the format of a message.
- This layer makes it easier for *machines with different internal representations to communicate*.

### 7. Application Layer
- Provides services directly to the end-user.
- While the Presentation Layer handles the actual data formatting, the Application Layer defines the semantics of the data exchange.
- It is where the *user interaction happens*.
- It's described as a collection of miscellaneous protocols for **(high level) common activities**.
	- Electronic mail
	- File transfer
	- Connecting remote terminals to computers over a network
-  Notable protocols:
	- X.400 electronic mail protocol
	- X.500 directory server