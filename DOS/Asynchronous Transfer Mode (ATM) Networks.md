- Sender establishes a *virtual circuit* connection to receiver(s)
- Route is determined and *routing information stored in switches*
- Data is divided into small, fixed-size units called **cells**
-  Advantages of ATM:
	- Single network can transport multiple types of data (voice, video, data, etc.)
	- Integration of services leads to cost savings
	- Enables new applications like video-on-demand and teleconferencing
- Cells follow predetermined paths through switches
- Network only sees cells, not caring about content
- Efficient for both **point-to-point** and **multicasting**
- Benefits of fixed-size cells:
	- Allow *rapid switching*
	- Eliminate delays caused by large packets blocking lines
	- New cells can be sent immediately after each transmission
- Comparison to traditional methods:
	- More flexible than conventional circuit switching
	- Better suited for broadcast media than telephone systems
	- Can handle point-to-point and multicasting efficiently
-  Applications:
	- Suitable for *various networks* (telephone, cable TV, etc.)
	- Enables *video conferencing* and access to *remote data bases*
	- Efficient for *broadcasting to multiple destinations simultaneously*

![[Pasted image 20240819221223.png]]
# ATM Layers

### 1. Physical Layer
- *ATM adaptor boards can output a stream of cells onto a wire or fiber*
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
- Europeans wanted 32-byte cells to avoid echo suppressors
- Americans preferred 64-byte cells for efficiency
- Compromise resulted in 48-byte cells, with a 5-byte header added
- *53-byte total size (48-byte data field + 5-byte header)*
- Fits well into 774-byte SONET payload
-  **Synchronization**:
	- **Two levels needed**: one for SONET frame, one for ATM cell within SONET payload
- Computer-to-switch header layout differs from switch-to-switch
- Header Layout:
	![[Pasted image 20240819222956.png]]
	- GFC field in computer-to-switch replaced by four more VPI bits in switch-to-switch
		- Both types have 48-byte payloads following the header
	- **GFC**: May be used for *flow control* in the future
	- **VPI** and **VCI**: Identify *path and virtual circuit for routing*
	- **Payload type**: *Distinguishes data cells from control cells*
	- **CLP**: Marks cell *importance for congestion* (lower priority cells dropped first)
	- **CRC**: 1-byte checksum over the header

- Fields modified at each hop
- VPI groups virtual circuits for the same destination

### 3. Adaptation Layer (AAL - ATM Adaptive Layer)
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
-  SEAL (Simple and Efficient Adaptation Layer) \[AAL 5]:
	- Nickname for AAL 5
	- Uses only one bit in the ATM header (Payload type field)
	- Bit is normally 0, set to 1 in the last cell of a packet
	- **Trailer (final 8 bytes)**: The trailer helps *mark the end of a packet*. The last cell of a packet has its *Payload type bit set to 1, indicating that it contains the trailer*. This allows the receiving end to know when a complete packet has been received.
- SEAL Packet Structure:
	- Padding (zeros) may be added between packet end and trailer start
	- Destination assembles incoming cells until it finds the end-of-packet bit
	- Trailer is then extracted and processed
-  Trailer Structure:
	- Four fields in total
		- First two fields (1 byte each) are unused
		- 2-byte field for *packet length*
		- 4-byte *checksum* over the packet, padding, and trailer

# ATM Switching Techniques
- ATM networks use *copper* or *optical* cables and switches.
- A network can have multiple switches, each with four ports for input and output.
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
