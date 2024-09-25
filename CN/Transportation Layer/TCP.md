- **Connection-Oriented**: TCP is a connection-oriented protocol, meaning it establishes a connection before data transfer and closes it after the transfer is complete.
- **Reliability**: TCP ensures reliable data transfer by using error detection (checksums), retransmission of lost or corrupted packets, and acknowledgment mechanisms.
- **Phases**: TCP explicitly defines phases for connection establishment, data transfer, and connection termination.
- **Protocol Combination**: TCP combines aspects of GBN and SR protocols to enhance reliability.
- **Timers**: TCP employs timers to manage retransmissions and ensure timely delivery.

### TCP Services

- **Process-to-Process Communication**: TCP, like UDP, provides communication between processes using port numbers.
- **Stream Delivery Service**: TCP is stream-oriented, allowing data to be sent as a continuous flow of bytes, unlike UDP, which sends messages with predefined boundaries.
- **Sending and Receiving Buffers**: TCP uses buffers to manage data flow between the sending and receiving processes, ensuring that data is stored until acknowledged or processed.
- **Segments**: Data is grouped into segments with added headers for control before being sent to the network layer. These segments can vary in size and are encapsulated in IP datagrams for transmission.
- **Full-Duplex Communication**: TCP supports simultaneous two-way data flow, with separate sending and receiving buffers at each endpoint.
- **Multiplexing and Demultiplexing**: TCP handles multiple connections by multiplexing at the sender and demultiplexing at the receiver, requiring a connection to be established for each pair of processes.
- **Connection-Oriented Service**: TCP establishes a logical connection between processes before data transfer, manages the data exchange, and then terminates the connection.
- **Reliable Service**: TCP ensures reliable data delivery through acknowledgment mechanisms, checking for the safe arrival of data and managing retransmissions if necessary.

### TCP Features
- TCP tracks segments but does not have a specific segment number field.
- Instead, TCP uses the **sequence number** and **acknowledgment number** fields that refer to byte numbers.

1. **Byte Numbering**:
   - TCP assigns a number to every data byte transmitted in a connection.
   - Numbering is independent in each direction.
   - *The starting byte number is arbitrarily chosen between 0 and 2³² − 1.*
   - Byte numbering is utilized for flow and error control.

2. **Sequence Number**:
   - *The number assigned to the first byte of the data in the segment.*
   - The first segment's sequence number is called the ***Initial Sequence Number (ISN)***.
   - ***Sequence number of subsequent segments is ( previous segment's sequence number ) + ( number of bytes it carried ).***
   - The *sequence number* in the segment header indicates the *number assigned to first data byte in that segment*.
   - Some control segments, even without actual data, consume a sequence number for acknowledgment purposes.

4. **Acknowledgment Number**:
	- Each party uses the acknowledgment number to *confirm the bytes it has received*.
	- *The acknowledgment number specifies the next byte expected to be received.*
	- ***It is  ( number assigned to the last byte that it has received ) + 1.*** 
	- If the receiver of the segment has successfully  received byte number x from the other party, it returns x + 1 as the acknowledgment number.
	- Acknowledgment and data can be piggybacked together.
	- TCP communication is full duplex, meaning both parties can send and receive data simultaneously.
	- The acknowledgment number is cumulative, representing all bytes received up to that point plus one.
	   - If a party uses 5,643 as an acknowledgment number, it has received all bytes *from the beginning up to 5,642* (not received 5,642 bytes; as first byte might not be 0). 

### Segment
#### Format
 - The header is 20 bytes if there are no options and up to 60 bytes if it contains options.
 ![[Pasted image 20240902202346.png]]
**Header Length**
- This 4-bit field indicates the number of 4-byte words in the TCP header. The length of the header can be between 20 and 60 bytes. Therefore, the value of this field is always between 5 (5 × 4 = 20) and 15 (15 × 4 = 60).

![[Pasted image 20240902202709.png]]
**Window size**
- This field defines the window size of the sending TCP in bytes. Note that the length of this field is 16 bits, which means that the maximum size of the window is 65,535 bytes.

**Checksum**
- Same calculation as UDP checksum.
- Use of the checksum in the UDP datagram is optional, whereas the use of the checksum for TCP is mandatory.
![[Pasted image 20240902203117.png]]

**Urgent Pointer**
-  It defines a value that must be added to the sequence number to obtain the number of the last urgent byte in the data section of the segment.

**Options**
-  40 bytes of optional information in the TCP header.

#### Encapsulation
A TCP segment encapsulates the data received from the application layer. The TCP segment is encapsulated in an IP datagram, which in turn is encapsulated in a frame at the data-link layer.

### TCP phases
#### 1. Connection Establishment
![[Pasted image 20240902203643.png]]

##### Passive Open (Server Side)
- The server program indicates to its TCP that it is ready to accept a connection.
- The server cannot initiate the connection itself.

##### Active Open (Client-Side)
- The client program signals to connect to the server.
- TCP then begins the three-way handshake process.
##### Three-Way Handshaking
- Connection establishment involves three-way handshaking.
###### Step 1: Client Sends SYN Segment (Control Segment)
- The client sends the first segment, a SYN segment, with the SYN flag set.
- The SYN segment is used for synchronizing sequence numbers and carries the client's initial sequence number (ISN).
- This segment does not contain an acknowledgment number or define the window size.
- *It carries no data but consumes one sequence number.*

###### Step 2: Server Sends SYN + ACK Segment:
- The server responds with a SYN + ACK segment, setting both SYN and ACK flags.
- This segment serves two purposes: initializing the server's sequence number and acknowledging the client's SYN segment.
- The segment also defines the receive window size (rwnd) for the client.
- *Like the SYN segment, it does not carry data but consumes one sequence number.*

###### Step 3: Client Sends ACK Segment:
- The client sends the third segment, an ACK segment, to acknowledge receipt of the server's SYN + ACK segment.
- *If this segment carries no data, it consumes no sequence number.*
- *However, some implementations may allow this segment to carry the first chunk of data, consuming sequence numbers equivalent to the number of data bytes.*

##### SYN Flooding Attack
- Attackers send a large number of SYN segments to a server, pretending to be different clients by faking the source IP addresses.
- The server, assuming these are legitimate connection requests, allocates resources like Transfer Control Block (TCB) tables and sets timers.
- The server sends SYN + ACK segments back to the fake clients.
- Since the clients are fake, these SYN + ACK segments are lost, and the server waits for the third step of the handshake.
- During this waiting period, the server's resources are tied up and remain unused and legitimate connections can no longer be made.
- Type of DoS attack.
- **Solutions:**
	- Limiting connection requests.
	- Filtering legitimate datagrams.
	- Verifying IP address by cookie.

#### 2. Transmission
![[Pasted image 20240903204100.png]]
##### Pushing Data
- TCP allows for flexible data buffering and delivery to increase efficiency.
- If immediate transmission and delivery are required (e.g., for interactive applications), the sender can request a push operation.
- In a push operation, TCP sends the data immediately without waiting for the buffer to fill and sets the PSH flag to ensure prompt delivery.
##### Urgent Data
- TCP is stream-oriented, meaning data is treated as a continuous stream of bytes.
- When an application needs to send urgent data, it can set the URG bit in the segment.
- The urgent data is placed at the beginning of the segment, and the urgent pointer field indicates the end of the urgent data.
- Urgent data is not a priority or out-of-band service; instead, it marks a portion of the byte stream for special treatment by the receiving application.

#### 3. Connection Termination
##### Three-Way Handshaking
![[Pasted image 20240903204959.png]]

###### Step 1: Client Sends FIN Segment:
- The client TCP, after receiving a close command from the client process, sends a FIN segment with the FIN flag set.
- The FIN segment may include the last chunk of data from the client or may just be a control segment.
- The FIN segment consumes one sequence number if it does not carry data.
###### Step 2: Server Sends FIN + ACK Segment:
- This segment confirms the receipt of the client's FIN segment and simultaneously announces the closing of the connection in the opposite direction.
- *The FIN + ACK segment consumes only one sequence number if it does not carry data.*
###### Step 3: Client Sends ACK Segment:
- *This segment cannot carry data and does not consume any sequence numbers.*

##### Half-Close
   ![[Pasted image 20240903233618.png]]
- TCP allows one end of a connection to stop sending data while still receiving data, known as a half-close.
- Either the server or the client can initiate a half-close.
- A half-close is useful when the server needs all the data from the client before starting a process, such as sorting.
- *The client sends a FIN segment to indicate it has finished sending data.*
- The server acknowledges the half-close by sending an ACK segment, but it can continue to send data to the client.
- *After sending all processed data, the server sends a FIN segment to close the connection in the server-to-client direction.*
- The client acknowledges this with an ACK segment.
- Once the connection is half-closed, data can only travel from the server to the client, ACKs can still be exchanged.

##### Connection Reset
- TCP at one end may deny a connection request, may abort an existing connection, or may terminate an idle connection. All of these are done with the RST (reset) flag.

### Transition Diagram
#### Half-Close
![[Pasted image 20240904004911.png]]
- The TCP sends a FIN segment upon receiving `active close` and goes to the FIN-WAIT-1 state. When it receives the ACK segment, it goes to the FIN-WAIT-2 state (data are still received). When the client receives a FIN segment, it sends an ACK segment and goes to the TIME-WAIT state.
-  The server, upon receiving the FIN segment, sends all queued data to the server with a virtual EOF marker, which means that the connection must be closed. It sends an ACK segment and goes to the CLOSE-WAIT state, here it transfers data to the client until it receives `passive close` from the process making it send FIN.
![[Pasted image 20240904011551.png]]
### Windows in TCP
- Window size is dictated by receiver (flow control) and sender (congestion control).
#### Send Window
![[Pasted image 20240904111101.png]]
##### Difference with SR send window
1.  *The window size in SR is the number of packets, but the window size in TCP is the number of bytes.*
2. TCP can store data received from the process and send them later.
3. SR may use several timers for each packet sent, but TCP protocol uses only one timer.

#### Receive Window
![[Pasted image 20240904111459.png]]

- *Receive window never shrinks.*
##### Difference with SR receive window
- *TCP allows the receiving process to pull data at its own pace.* The receive window size is then always smaller or equal to the buffer size. The receive window size determines the number of bytes that the receive window can accept from the sender before being overwhelmed (flow control).
	`rwnd = buffer size − number of waiting bytes to be pulled`
- TCP can use both selective and cumulative acknowledgement.

### Flow Control
#### Data Flow in TCP
   - Data travels from the sending process to the sending TCP, then from the sending TCP to the receiving TCP, and finally from the receiving TCP to the receiving process (paths 1, 2, and 3).
   - Flow control feedback travels from the receiving TCP to the sending TCP and from the sending TCP to the sending process (paths 4 and 5).
![[Pasted image 20240904114718.png]]
#### Flow Control Feedback
   - Most TCP implementations do not provide flow control feedback from the receiving process to the receiving TCP. Instead, the receiving process pulls data when ready.
   - The receiving TCP controls the sending TCP, while the sending TCP controls the sending process.

#### Flow Control at the Sending Process
   - The sending TCP manages flow control with the sending process by rejecting data when its window is full (path 5).
   - *Flow control mainly focuses on feedback from the receiving TCP to the sending TCP (path 4).*

#### Opening and Closing Windows
   - To manage flow control, TCP adjusts the sender's and receiver's window sizes, even though the buffer size is fixed when the connection is established.
   - The receive window closes (moves its left wall right) when more bytes arrive and opens (moves its right wall right) when more bytes are pulled by the receiving process.

#### Flow Control at Sending TCP
   - *The receiver controls the send window:*
     - The send window closes (moves its left wall right) when new acknowledgments are received.
     - *It opens (moves its right wall right) when the advertised receive window size (rwnd) allows it.*
		`new ackNo + new rwnd > last ackNo + last rwnd` 
     - The send window may shrink if the new window size does not allow it to open further.
![[Pasted image 20240904225324.png]]
1. **Receive Window Rule**:
   - The receive window (rwnd) cannot shrink. This means that once the receiver advertises a certain window size, it cannot decrease.

2. **Send Window Shrinking**:
   - The send window can potentially shrink if the receiver defines a new rwnd that results in a smaller window.
   - However, some TCP implementations prevent the shrinking of the send window by not allowing the right boundary of the send window to move to the left.

3. **Preventing Window Shrinking**:
   - To avoid shrinking, the receiver must maintain the relationship:
     - **new ackNo + new rwnd ≥ last ackNo + last rwnd**
   - This ensures that the right boundary of the send window does not move left, which would shrink the window.

#### Window Shrinking Issues
- If the send window shrinks, it can create problems where some already sent bytes fall outside the new window, causing confusion about which bytes have been transmitted.
![[Pasted image 20240904225918.png]]
- The following relation must always be true to present send window from shrinking,
	`new ackNo + new rwnd ≥ last ackNo + last rwnd`
- If reducing the `rwnd` value would cause the send window to shrink (i.e., the new `ackNo + rwnd` would be less than the previous `ackNo + rwnd`), the receiver delays sending the acknowledgement with the new window size.
#### Silly Window Syndrome
- A serious problem can arise in the sliding window operation when either the sending application program creates data slowly or the receiving application program consumes data slowly, or both. Any of these situations results in the sending of data in very small segments, which reduces the efficiency of the operation. 
### Error Control
-  Error control includes mechanisms for detecting and resending corrupted segments, resending lost segments, storing out-of-order segments until missing segments arrive, and detecting and discarding duplicated
#### Error Control Mechanisms
##### 1. Checksum
- It is 16 bits.
-  If a segment is corrupted, as detected by an invalid checksum, the segment is discarded by the destination TCP and is considered as lost. 
##### 2. Acknowledgement
- It confirms the receipt of data segments. 
- Control segments that carry no data, but consume a sequence number, are also acknowledged. 
- ACK segments are never acknowledged.

##### 3. Time out
- When the retransmission timer expires or when the sender receives three duplicate ACKs for the first segment in the queue, that segment is retransmitted.
#### Types of Acknowledgement
##### Cumulative Acknowledgement (ACK)
- This acknowledgement implies that all bytes up to (but not including) the acknowledged byte have been received correctly and in order.
##### Selective Acknowledgement (SACK)
- It enhances TCP by allowing the receiver to acknowledge specific segments that have been received out of order, reducing the need for unnecessary retransmissions and improving performance in complex network conditions.
- Since SACK is not part of the original TCP specification, its use requires both the sender and receiver to support it.

#### Generating Acknowledgement
-  **RULE 1: Piggybacking:** When sending data, an acknowledgment (ACK) is included, indicating the next expected sequence number to reduce traffic.
-  **RULE 2: Delayed ACKs:** If the receiver has *no data to send* and the segment is in order, it *delays the ACK for a short period (usually 500 ms)* unless another segment arrives. This reduces the number of ACK segments.
-  **RULE 3: Immediate ACK for Unacknowledged Segments:** If a segment arrives in order and the previous one hasn't been acknowledged, an immediate ACK is sent to avoid unnecessary retransmissions.
-  **RULE 4: Out-of-Order Segments:** If a segment with a higher sequence number arrives, an immediate ACK is sent, prompting *fast retransmission* of missing segments.
-  **RULE 5: Missing Segment Arrival:** When a previously missing segment arrives, an ACK is sent to update the sequence number expected.
- **RULE 6: Duplicate Segments:** Duplicate segments are discarded, but an ACK is immediately sent to indicate the next expected sequence number, addressing potential issues with lost ACK segments.
#### Retransmission
1. **Retransmission after RTO (Retransmission Time-Out):** Each TCP connection maintains a retransmission timer (RTO). When this timer expires, the segment at the front of the queue (with the smallest sequence number) is retransmitted, and the timer is reset. The RTO is dynamic, adjusted based on the round-trip time (RTT) of segments.
2. **Retransmission after Three Duplicate ACKs (Fast Retransmission):** To avoid long delays from waiting for the RTO, TCP implements fast retransmission. If the sender receives three duplicate ACKs (indicating a segment is missing), the missing segment is retransmitted immediately, without waiting for the RTO to expire. This helps to expedite data transmission across the network.
#### Out-of-Order Segments
- Data may arrive out of order and be temporarily stored by the receiving TCP, but TCP guarantees that no out-of-order data are delivered to the process.
#### FSMs for data transfer in TCP
- TCP can best be modeled as a Selective-Repeat protocol.
##### Sender-Side FSM
![[Pasted image 20240905030150.png]]
- In SR there is no possibility of fast transmission (three duplicate ACKs).
- In SR there is no possibility of window size adjustment based on the value of rwnd.
##### Receiver-Side FSM
![[Pasted image 20240905030438.png]]
- *In TCP unlike SR, there is delaying ACK in unidirectional communication.*
- *In TCP unlike SR, immediate ACK must be sent if the receiver has some data to return.*
#### Scenarios
##### Normal Operation
![[Pasted image 20240905031137.png]]

##### Lost Segment
![[Pasted image 20240905031548.png]]
- If RTO timer has expired the lost segment has similar working to that of SR.
-  A lost or corrupted segment is treated the same way by the receiver. A lost segment is discarded somewhere in the network; a corrupted segment is discarded by the receiver itself.
- Here, the RTO timer expires before receiving third duplicate acknowledgement.
- The receiver TCP delivers only ordered data to the process.
##### Fast Retranssmission
![[Pasted image 20240905031858.png]]
- The RTO timer must not expire and receive three duplicate ACKs for retransmission to occur.
##### Delayed Segment
-  Delayed segments sometimes may time out and resent. 
- If the delayed segment arrives after it has been resent, it is considered a duplicate segment and discarded.
##### Duplicate Segment
- When a segment arrives that contains a sequence number equal to an already received and stored segment, it is discarded.
##### Automatically Corrected Lost ACK
-  A lost acknowledgment may not even be noticed by the source TCP if RTO timer hasn't expired.
- *TCP uses cumulative acknowledgment.*
	- The next acknowledgment automatically corrects the loss of the previous acknowledgment.

##### Lost Acknowledgment Corrected by Resending a Segment
![[Pasted image 20240905032710.png]]
- Only one segment is retransmitted although two segments are not acknowledged. When the sender receives the retransmitted ACK, it knows that both segments are safe and sound because the acknowledgment is cumulative.
##### Deadlock Created by Lost Acknowledgment
- TCP can encounter a deadlock when an acknowledgment (ACK) with a nonzero receive window (`rwnd`) value is lost.
	- **Receiver Closes the Window:** The receiver sends an ACK with `rwnd` set to zero, asking the sender to temporarily stop sending data.
	- **Receiver Reopens the Window:** Later, the receiver is ready to receive more data and sends an ACK with a nonzero `rwnd` to remove the restriction.
	- **ACK is Lost:** If this ACK is lost in transit, the sender continues to believe the window is closed and waits for an update.
	- **Deadlock Occurs:** The receiver assumes the sender has received the updated `rwnd` and waits for data, while the sender waits for an acknowledgment of a nonzero window. Both sides are waiting indefinitely.
	- Since no retransmission timer is set in this scenario, the deadlock persists.
- To prevent this issue, TCP uses a **persistence timer**. This timer ensures that the sender periodically probes the receiver to check if the window size has changed.


### Congestion Control
#### Congestion Window
1. **Flow Control via Receiver Window (rwnd):** TCP uses the receive window (rwnd) to control the sender's transmission rate, ensuring the receiver’s buffer doesn’t overflow. 
2. **Router Congestion:** Congestion can still occur in routers, even if there’s no receiver congestion. When routers become overwhelmed, they drop segments, increasing retransmissions and exacerbating congestion.
3. **TCP’s Responsibility:** While congestion happens in IP (which lacks congestion control), TCP must manage network congestion to prevent communication collapse.
4. **Balancing Data Transmission:** TCP must balance transmission—sending too aggressively worsens congestion, but being overly conservative underutilizes available bandwidth.
5. **Congestion Window (cwnd):** TCP uses a variable, **cwnd**, to manage congestion in the network. The sender’s window size is the smaller of **rwnd** (receiver window) and **cwnd** (congestion window).
6. **Effective Window Size:** The actual send window size is determined by the minimum of **rwnd** and **cwnd**.

#### Congestion Detection
1. **Congestion Detection Events:** TCP identifies network congestion through two main events: time-outs and receiving three duplicate ACKs.
2. **Time-Out:** If the sender does not receive an ACK within a specified time, it assumes the segment(s) are lost due to congestion.
3. **Three Duplicate ACKs:** Receiving three ACKs with the same acknowledgment number indicates a missing segment, suggesting potential congestion. However, this usually indicates less severe congestion compared to a time-out.
4. **Congestion Severity:** Three duplicate ACKs imply that while one segment is missing, the network is not critically congested, or it has recovered. A time-out indicates stronger congestion.
5. **TCP Versions:** The original Tahoe TCP treated time-outs and three duplicate ACKs similarly, while the later Reno TCP differentiates between the two events in its congestion control approach.
6. **Feedback Mechanism:** TCP relies on ACKs to detect congestion—lack of timely ACKs suggests severe congestion, while multiple duplicate ACKs indicate milder congestion.

#### Congestion Policies
##### 1. Slow Start: Exponential Increase
1. **Initial Congestion Window (cwnd):** The slow-start algorithm begins with a congestion window size (cwnd) of 1 Maximum Segment Size (MSS).
2. **Exponential Growth:** For each received acknowledgment (ACK), the cwnd increases by 1 MSS. This results in exponential growth of the window size:
![[Pasted image 20240915215226.png]]

3. **Growth Rate:** The growth of cwnd is exponential relative to the number of RTTs, which makes this phase aggressive in increasing the transmission rate.
4. **Slow-Start Threshold (ssthresh):** Slow start continues until the cwnd reaches a threshold value known as ssthresh. Once this threshold is reached, the slow-start phase ends and the algorithm transitions to a different phase.
5. **Effect of Delayed ACKs:** If ACKs are delayed (e.g., cumulative ACKs for multiple segments), the increase in cwnd is less aggressive:
   - With cumulative ACKs, cwnd increases by 1 MSS per ACK rather than per segment. Thus, the growth is still exponential but less pronounced compared to the ideal case where each ACK corresponds to a single segment.
6. **Summary:** Slow start results in exponential growth of the congestion window until a threshold is reached, after which the growth rate changes. Delayed ACKs can slow down the effective growth rate of cwnd.
![[Pasted image 20240915213404.png]]
##### 2. Congestion Avoidance: Additive Increase
1. **Purpose:** To slow down the exponential growth of the congestion window (cwnd) and avoid congestion, TCP introduces the congestion avoidance phase.
2. **Transition:** When **cwnd** reaches the slow-start threshold (ssthresh), the algorithm transitions from slow start to congestion avoidance.
3. **Additive Increase:** 
   - In congestion avoidance, **cwnd** increases additively instead of exponentially.
   - For every full window of segments acknowledged, **cwnd** is increased by 1 MSS.
4. **Increment Formula:** 
   - The increase in **cwnd** per acknowledgment is $$ \ \frac{1}{\text{cwnd}}  $$ MSS. 
   - Therefore, the size of **cwnd** grows by 1 MSS every RTT, assuming a full window of segments is acknowledged.
5. **Growth Rate:** 
   - The growth rate of **cwnd** is linear with respect to the number of RTTs, which is more conservative than the exponential growth in the slow start phase.
6. **Example:** 
![[Pasted image 20240915215338.png]]
7. **Behavior:** The congestion window increases linearly until congestion is detected. Upon detecting congestion, TCP may revert to slow start or adjust **ssthresh** and the growth strategy.
![[Pasted image 20240915214738.png]]

###### Fast Recovery
-  Like congestion avoidance, this algorithm is also an additive increase, but it increases the size of the congestion window when a duplicate ACK arrives (after the three duplicate ACKs that trigger the use of this algorithm). 
- If a duplicate ACK arrives, 
	- cwnd = cwnd + (1 / cwnd).

#### Types of TCP
##### 1. Taho TCP
![[Pasted image 20240915215959.png]]
1. **Algorithms:** Taho TCP uses two algorithms for congestion control: slow start and congestion avoidance.
	- This version of TCP emphasizes the combination of aggressive growth in slow start and more conservative growth in congestion avoidance 
1. **Initial Conditions:** 
   - Starts with **cwnd** = 1 MSS and **ssthresh** set to a pre-agreed value (typically a multiple of MSS).
   - In the slow-start phase, **cwnd** increases exponentially by 1 MSS for each ACK received.
2. **Congestion Detection:** 
   - **Time-out** or receiving **three duplicate ACKs** signals congestion.
   - Upon detecting congestion, Taho TCP resets **cwnd** to 1 MSS and sets **ssthresh** to half of the current **cwnd**.
3. **Transition to Congestion Avoidance:** 
   - Once **cwnd** reaches **ssthresh** without detecting further congestion, TCP transitions to congestion avoidance.
4. **Congestion Avoidance Phase:** 
   - In this phase, **cwnd** increases linearly by 1 MSS for every **cwnd** ACKs received.
   - There is no fixed upper limit for **cwnd**; it grows conservatively until congestion is detected again.
5. **Handling Congestion in Avoidance Phase:**
   - On detecting congestion, **ssthresh** is set to half of the current **cwnd**.
   - TCP then returns to the slow-start phase with the new **ssthresh** value.
6. **Adjustment of ssthresh:** 
   - The value of **ssthresh** can increase if congestion is detected and **cwnd** is larger than **ssthresh**. For example, if **ssthresh** was originally 8 MSS and **cwnd** is 20 MSS, the new **ssthresh** might be 10 MSS.
##### 2. Reno TCP
![[Pasted image 20240915222801.png]]
1. **Introduction:** Reno TCP introduces the fast-recovery state to improve congestion control by handling congestion signals differently.
2. **Congestion Detection:**
	- **Time-Out:** Moves TCP to the slow-start state, similar to Taho TCP.
	- **Three Duplicate ACKs:** Triggers the fast-recovery state instead of moving directly to slow start.
3. **Fast-Recovery State:**
	- **Behavior:** Functions between slow start and congestion avoidance.
	- **cwnd Initialization:** Starts with **cwnd** set to **ssthresh** + 3 MSS, rather than 1 MSS as in slow start.
	- **Growth:** While in fast recovery, **cwnd** grows exponentially as long as duplicate ACKs continue to arrive.
4. **Events in Fast Recovery:**
	- **More Duplicate ACKs:** **cwnd** continues to grow exponentially.
	- **Time-Out:** TCP assumes severe congestion and reverts to the slow-start state.
	- **New (Non-Duplicate) ACK:** Transition to congestion avoidance, but **cwnd** is deflated to **ssthresh**.
5. **FSM Overview:** The fast-recovery state allows TCP to quickly recover from packet loss without a full return to slow start, leading to more efficient recovery and less drastic reductions in transmission rate.
6. **FSM Simplification:** In the Reno TCP FSM, less critical events are omitted to focus on the key states and transitions.

##### NewReno TCP
1. **Optimization on Reno TCP:** NewReno improves upon Reno TCP by handling multiple segment losses more efficiently.
   
2. **Handling Duplicate ACKs:**
   - When three duplicate ACKs arrive, NewReno retransmits the lost segment as in Reno.
   - If the new (non-duplicate) ACK acknowledges all data up to the end of the window, NewReno assumes only one segment was lost.
   - If the new ACK acknowledges data before the end of the window, NewReno assumes additional segments are lost and retransmits them without waiting for more duplicate ACKs.

3. **Multiple Loss Recovery:** This allows NewReno to recover from multiple lost segments within the same window without returning to slow start, making it more efficient in handling packet loss.

###### Additive Increase, Multiplicative Decrease (AIMD)
1. **Additive Increase:** During congestion avoidance, for every ACK received, **cwnd** increases by a small amount (typically 1 MSS per RTT). This linear increase prevents overwhelming the network.
   - Formula: \( \text{cwnd} = \text{cwnd} + \frac{1}{\text{cwnd}} \)

2. **Multiplicative Decrease:** When congestion is detected (through time-out or three duplicate ACKs), **cwnd** is reduced by half to alleviate congestion.
   - Formula: \( \text{cwnd} = \frac{\text{cwnd}}{2} \)

3. **Sawtooth Pattern:** The combination of additive increase and multiplicative decrease creates a sawtooth pattern in the **cwnd** over time, gradually increasing during congestion avoidance and sharply decreasing when congestion is detected.

4. **Dominance of AIMD:** In long TCP connections, the slow-start and fast-recovery phases are brief, so most of the time TCP operates under AIMD, which efficiently balances network utilization and congestion control.

### TCP Throughput
   - Given that the minimum **cwnd** is half of the maximum **cwnd**, the throughput can be more accurately calculated as:
  $$ 
   \text{Throughput} = \frac{0.75 \times W_{\text{max}}}{\text{RTT}}
   $$
   - Where Wmax is the maximum window size when congestion is detected.
   - The factor of 0.75 reflects the reduction of **cwnd** after congestion is detected and the AIMD process.