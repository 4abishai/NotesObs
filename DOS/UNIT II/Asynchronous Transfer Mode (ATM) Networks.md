## What is ATM?
Asynchronous Transfer Mode (ATM) is a high-speed networking technology that uses fixed-size cells of 53 bytes to transmit data. Each ATM cell consists of a 5-byte header and a 48-byte payload.

ATM operates at the ***data link layer*** (Layer 2) of the OSI model, and it is ***connection-oriented***, meaning it *establishes a virtual connection between endpoints before data transmission begins*. ATM is known for its ability to provide **Quality of Service (QoS)** guarantees, making it suitable for real-time applications like voice and video conferencing. 
## Charactersitics:
- Sender establishes a *virtual circuit* connection to receiver(s)
- Route is determined and *routing information stored in switches*
- Data is divided into small, fixed-size units called **cells**
## Advantages of ATM:
- Single network *can transport multiple types of data* (voice, video, data, etc.)
- Integration of services leads to cost savings
- Enables new applications like *video-on-demand* and *teleconferencing*
- Efficient for *broadcasting to multiple destinations simultaneously*.
## Benefits of fixed-size cells:
- Allow *rapid switching*
- Eliminate delays caused by large packets blocking lines
- New cells can be sent immediately after each transmission
## Comparison to traditional methods:
- More flexible than conventional circuit switching
- Better suited for broadcast media than telephone systems
- Can handle **point-to-point** and **multicasting** efficiently

## ATM Layers
![[Pasted image 20240819221223.png]]
### 1. Physical Layer
- It is responsible for the *transmission and reception of raw bitstreams over a physical medium*.
- Empty cells are transmitted when no data is available
- ATM is *synchronous at the physical layer*, but *asynchronous within virtual circuits*
- **SONET (Synchronous Optical NETwork)**:
	- Used in the physical layer for ATM
	- *ATM cells are placed in the payload portion of SONET frames*
	- Compatible with AT&T and other carriers' internal transmission systems
	- In Europe, a similar system called *SDH* is used
- ATM and SONET unlikely for audio telephones (ISDN used instead)
- ATM and SONET considered ideal for videophones

### 2. ATM Layer
 - The ATM layer is responsible for the cell-based transmission. It deals with the transmission, and switching of small, fixed-sized packets called **ATM cells**. Each cell is 53 bytes long, consisting of a 5-byte header and a 48-byte payload. 
 - This layer handles functions like *cell multiplexing*, *cell switching*, and the maintenance of *Quality of Service (QoS)*.
#### Header Layout:
![[Pasted image 20240819222956.png]]
- **Fields**:	
	- ***GFC***: May be used for *flow control* in the future
	- ***VPI*** and ***VCI***: Identify *path and virtual circuit for routing*
	- ***Payload type***: *Distinguishes data cells from control cells*
	- ***CLP***: Marks cell *importance for congestion* (lower priority cells dropped first)
	- ***CRC***: 1-byte checksum over the header
- GFC field in computer-to-switch replaced by four more VPI bits in switch-to-switch
- Both types have 48-byte payloads following the header
- Fields modified at each hop
- VPI groups virtual circuits for the same destination

### 3. Adaptation Layer (AAL - ATM Adaptive Layer)
- *Handles segmentation and reassembly of data, error detection, and the handling of timing issues.*
-  Need for Adaptation Layer:
	- At 155 Mbps, a cell arrives every 3 μsec
	- Most CPUs can't handle 300,000 interrupts/sec
	- Adaptation layer *breaks packets into cells and reassembles them*, generating *one interrupt per packet instead of per cell*
- Adaptation layer runs on the **host adaptor board**
-  **Traffic Classes** (*bit rate* & *data traffic*):
	1. Constant bit rate (for audio and video)
	2. Variable bit rate with bounded delay
	3. Connection-oriented data traffic
	4. Connectionless data traffic
-  Evolution of Classes:
	- Classes 3 and 4 were merged into a new class 3/4
	- Computer industry proposed AAL 5 for computer-to-computer traffic
####  SEAL (Simple and Efficient Adaptation Layer) \[AAL 5]:
- Nickname for AAL 5
- Uses only one bit in the ATM header (Payload type field)
- Bit is normally 0, set to 1 in the last cell of a packet
- **Trailer (final 8 bytes)**: The trailer helps *mark the end of a packet*. The last cell of a packet has its *Payload type bit set to 1, indicating that it contains the trailer*. This allows the receiving end to know when a complete packet has been received.
- **SEAL Packet Structure**:
	- Padding (zeros) may be added between packet end and trailer start
	- Destination assembles incoming cells until it finds the end-of-packet bit
	- Trailer is then extracted and processed
-  **Trailer Structure**:
	- Four fields in total
		- First two fields (1 byte each) are unused
		- 2-byte field for *packet length*
		- 4-byte *checksum* over the packet, padding, and trailer

### 4. Upper Layer
  - These are the layers above the adaptation layer, typically consisting of the network, transport, and application layers (e.g., IP, TCP, applications). These layers are not specific to ATM and are more related to the general OSI model or TCP/IP model. The adaptation layer ensures that these upper-layer protocols can effectively communicate over the ATM network.

# ATM Switching Techniques
- ATM networks use *copper* or *optical* cables and switches.
- A network can have multiple switches, each switch with four ports for input and output.
![[Pasted image 20240819233853.png]]
- Switches have input and output lines connected by a *parallel switching fabric*.
- Parallel switching is necessary due to the *speed required* (3 μsec at OC-3) and *multiple simultaneous inputs*.
-  **Cell Routing**:
	- When a cell arrives, its VPI and VCI fields are examined to determine the correct output port.
	- Cells must be delivered in order.
-  **Head-of-line Blocking**:
	- This problem occurs when two *cells arrive simultaneously on different input lines* but need the *same output port*.
	- Simply discarding one cell is inefficient.
	- Solution:
		- A common solution is to *randomly select one cell to forward and hold the other for the next round*.
		- Some designs *copy cells into output port queues instead of keeping them in input buffers*.
		- Switches may use a *pool of buffers for both input and output*.
		- Input-side buffering with *selective forwarding* is another option.
- Other Switch Designs:
	- **Time division switches** using shared memory
	- **Space division switches** with multiple paths between inputs and outputs
	- Designs using **buses** or **rings**

### Implications of ATM on distributed systems

1. **Impact of Bandwidth**: The availability of high bandwidth (e.g., 155 Mbps, 622 Mbps, and potentially 2.5 Gbps) creates significant implications for distributed systems, mainly due to the sudden availability of large bandwidths. This has pronounced effects on wide-area distributed systems, where *latency becomes a critical issue*.

2. **Latency Concerns**: As network speeds increase, the time-to-reply decreases, but for very high-speed networks, it approaches zero, *which means most of the time is spent waiting for acknowledgments*. For smaller messages, this issue becomes even more pronounced, leading to inefficiencies.
	- Even though high-speed networks can transmit large amounts of data quickly, delay is introduced by waiting for acknowledgments.

3. **Flow Control Challenges**: Managing large files, such as a 10 GB video, over ATM networks can be problematic. *If buffer space is insufficient, data loss may occur, requiring retransmissions and potentially leading to inefficient use of bandwidth.* Traditional flow control methods, like sliding windows, may not be effective, leading to the need for new approaches.

4. **Transcontinental Delay**: The inherent delay in transcontinental communications (e.g., from New York to California) can significantly affect performance, especially when multiple sequential requests are made. This delay can lead to noticeable lag, which may prompt a rethink of distributed system architectures.

5. **Congestion and Cell Dropping**: ATM networks allow switches to drop cells when congested, which can cause significant problems, especially for services requiring uniform rates, like audio streaming.

