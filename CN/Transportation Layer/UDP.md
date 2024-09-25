- User Datagram Protocol (UDP) is a connectionless and unreliable transport protocol.
- Provides process-to-process communication, unlike IP's host-to-host communication.
- Lacks reliability but offers simplicity and minimal overhead.
- Suitable for sending small messages where reliability is not a concern.
- Requires less interaction between sender and receiver than TCP.
- Ideal for applications where speed is prioritized over accuracy.
- Some applications of UDP are discussed later in the section.

### User Datagram (UDP Packet) Key Points:
- **UDP Packets** are called user datagrams.
- They have a **fixed-size header** of 8 bytes, consisting of four fields, each 2 bytes (16 bits).
- The first two fields specify the **source and destination port numbers**.
- The third field defines the **total length** of the user datagram (header plus data), with a maximum size of 65,535 bytes.
- The actual size is usually less than 65,535 bytes because the user datagram is stored in an IP datagram, which has the same total length limit.
- The last field optionally includes a **checksum**.
![[Pasted image 20240915171824.png]]

![[Pasted image 20240915170930.png]]
### UDP Services
#### Process-to-Process Communication
   - UDP enables communication between processes using socket addresses, which combine IP addresses with port numbers.
#### Connectionless Services
   - UDP is connectionless, meaning each user datagram is independent.
   - There is no relationship between different datagrams from the same source or destination.
   - User datagrams are not numbered and do not require connection establishment or termination.
   - Each datagram can take a different path, and the sender cannot rely on UDP to segment a data stream into multiple datagrams.
   - Only short messages (less than 65,507 bytes) are suitable for UDP.
#### Flow Control
   - UDP lacks flow control and a window mechanism.
   - The receiver might be overwhelmed by incoming messages.
   - The process using UDP must handle flow control if needed.
#### Error Control
   - UDP includes only a checksum for error detection.
   - There is no mechanism to handle lost or duplicated messages.
   - Any erroneous datagram detected via the checksum is discarded without notification.
   - Error control must be managed by the process using UDP if required.

#### UDP Checksum
   - The UDP checksum is calculated using three sections: 
     - **Pseudoheader** (a part of the IP header with some fields set to 0)
     - **UDP header**
     - **Data from the application layer**
   - The pseudoheader ensures that the packet is delivered to the correct host by including parts of the IP header.
   - It prevents delivering a datagram to the wrong host if the IP header gets corrupted.
   - The *protocol field, with a value of 17 for UDP*, helps distinguish UDP packets from TCP packets.
   - If the protocol field changes during transmission, the checksum at the receiver will detect the error, and UDP will drop the packet to prevent incorrect delivery to the wrong protocol.
   - The sender can choose not to calculate the checksum, in which case the checksum field is set to all 0s.
   - *If the checksum is calculated and the result is 0, the sender sets the checksum to all 1s before sending the packet. This avoids confusion since a checksum of all 1s is not typical in normal circumstances.*
![[Pasted image 20240915172718.png]]
![[Pasted image 20240915173243.png]]
#### Congestion Control
   - UDP does not provide congestion control due to its connectionless nature.
   - It assumes packets are small and sporadic, though this may not hold true for modern real-time audio and video transfers.

#### Encapsulation and Decapsulation
   - UDP encapsulates messages for transmission and decapsulates them upon receipt, enabling communication between processes.

#### Queuing
   - Queues are associated with ports in UDP.
   - At the client site, processes request port numbers, and some implementations create both incoming and outgoing queues, while others only create an incoming queue.

#### Multiplexing and Demultiplexing
   - UDP handles multiple processes by multiplexing and demultiplexing since a host can have several processes using UDP services at once.

#### Comparison with a Generic Simple Protocol
   - UDP is similar to a connectionless simple protocol but with an optional checksum for error detection.
   - The checksum allows UDP to detect and discard corrupted packets without sending feedback to the sender.

### Typical Applications of UDP

1. **Simple Request-Response Communication:**
   - UDP is ideal for applications requiring quick request-response exchanges with minimal concern for flow or error control, unlike bulk data transfer processes like FTP.
2. **Processes with Internal Flow and Error Control:**
   - Applications like **Trivial File Transfer Protocol (TFTP)**, which have their own flow and error control mechanisms, can effectively use UDP.
3. **Multicasting:**
   - *UDP supports multicasting, making it suitable for applications that need to send messages to multiple recipients, a feature not available in TCP.*
4. **Management Protocols:**
   - UDP is commonly used for network management protocols such as **Simple Network Management Protocol (SNMP)**.
5. **Routing Protocols:**
   - UDP is used in routing protocols like the **Routing Information Protocol (RIP)** for route updates.
6. **Interactive Real-Time Applications:**
   - UDP is preferred for real-time applications (e.g., live audio and video streaming) where uneven delay is unacceptable.