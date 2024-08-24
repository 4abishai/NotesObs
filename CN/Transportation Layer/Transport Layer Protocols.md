### Simple Protocol

- It's a basic, **connectionless protocol** that lacks both **flow and error control**.
- The sender sends packets one after another without even thinking about the receiver.
- The receiver is expected to process and deliver each packet to its application layer *immediately* upon arrival.
![[Pasted image 20240824161944.png]]
1. **Sender Side:**
   - The transport layer at the sender waits for a message from its application layer.
   - Once a message is available, the transport layer encapsulates it into a packet and sends it to the receiver's transport layer.
2. **Receiver Side:**
   - The transport layer at the receiver waits for a packet to arrive from the sender.
   - Upon receiving a packet, the transport layer decapsulates the message from the packet and delivers it to its application layer.

#### Finite State Machines (FSMs)
- **Single State (Ready State):** Both the sender and receiver FSMs have only one state, called the "ready state."
  
  - **Sender FSM:**
    - ***Ready State:*** The FSM stays in this state until a message arrives from the application layer.
    - ***Event:*** When a message arrives, the FSM encapsulates it in a packet and sends it to the receiver.
    - ***Action:*** After sending the packet, the FSM returns to the ready state, waiting for the next message.

  - **Receiver FSM:**
    - ***Ready State:*** The FSM stays in this state until a packet arrives from the sender.
    - ***Event:*** When a packet arrives, the FSM decapsulates the message and delivers it to the application layer.
    - ***Action:*** After delivering the message, the FSM returns to the ready state, waiting for the next packet.
![[Pasted image 20240824162227.png]]

### Stop-and-Wait Protocol

- It is **connection oriented protocol**.
- **Flow and Error Control:** The protocol ensures that *data packets are sent one at a time*, and each packet must be acknowledged before the next one is sent.
	- This *prevents overwhelming* the receiver and *allows the detection and retransmission* of lost or corrupted packets.
- **Sliding Window:** Both the sender and the receiver use a *sliding window of size 1*, meaning that *only one packet and one acknowledgment* can be in transit at any given time.
- **Checksum:** Checksum detects *corruption*. If the checksum is incorrect, the receiver silently discards the packet, signaling the sender (via lack of acknowledgment) to resend the packet.
![[Pasted image 20240824164013.png]]
#### Sequence Numbers
- The protocol uses sequence numbers to distinguish between packets and prevent duplicates. Only two sequence numbers (0 and 1) are used, based on modulo 2 arithmetic.
  - **Sequence Number Range:** The sequence numbers alternate between 0 and 1. After sending a packet with sequence number 0, the next packet uses sequence number 1, and then it cycles back to 0.
  - **Purpose:** This minimal range ensures that the receiver can distinguish between new packets and duplicates in cases where acknowledgments are lost or corrupted.
  - Assume we have used x as a sequence number;
		1. The packet arrives safe and sound at the receiver site; the receiver sends an acknowledgment. The acknowledgment arrives at the sender site, causing the sender to send the next packet numbered x + 1.
		2. The packet is corrupted or never arrives at the receiver site; the sender resends the packet (numbered x) after the time-out. The receiver returns an acknowledgment.
		3. The packet arrives safe and sound at the receiver site; the receiver sends an acknowledgment, but the acknowledgment is corrupted or lost. The sender resends the packet (numbered x) after the time-out. Note that the packet here is a duplicate. The receiver can recognize this fact because it expects packet x + 1 but packet was received.

#### Acknowledgment Numbers
- The acknowledgment (ACK) number corresponds to the *sequence number of the next expected packet*.
  - For example, after receiving packet 0, the receiver sends an ACK with the number 1, indicating that it expects packet 1 next.
  - Similarly, after receiving packet 1, the receiver sends an ACK with the number 0, expecting packet 0 next.
- **Control Variables:**
  - ***Sender's Control Variable (S):*** Points to the slot in the send window (which has a size of 1).
  - ***Receiver's Control Variable (R):*** Points to the slot in the receive window (which also has a size of 1).
![[Pasted image 20240824170011.png]]
#### Finite State Machines (FSMs)
- Since this is a connection-oriented protocol, both the sender and receiver FSMs have to be in the "established" state before any data exchange can occur.
![[Pasted image 20240824171447.png]]
##### Sender FSM States:

1. ***Ready State:***
   - **Initial State:** The sender starts in the ready state, with the sequence number `S` initialized to 0.
   - **Event:** A request from the application layer.
   - **Actions:**
     1. Create a packet with the sequence number set to `S`.
     2. Store a copy of the packet.
     3. Send the packet.
     4. Start the timer.
   - **Transition:** The sender moves to the **blocking state** after sending the packet.

2. ***Blocking State:***
     - **Error-free ACK arrives with `ackNo = (S + 1) modulo 2`:**
        1. Stop the timer.
        2. Slide the window: Update `S = (S + 1) modulo 2`.
        3. **Transition:** Move back to the **ready state**.
     - **Corrupted ACK or ACK with `ackNo ≠ (S + 1) modulo 2` arrives:**
        1. Discard the ACK.
        2. **No state change** occurs; remain in the blocking state.
     - **Timeout occurs:**
        1. Resend the outstanding packet.
        2. Restart the timer.
        3. **No state change** occurs; remain in the blocking state.

##### Receiver FSM States:

1. ***Ready State:***
 - **Error-free packet with `seqNo = R` arrives:**
	1. Deliver the message to the application layer.
	2. Slide the window: Update `R = (R + 1) modulo 2`.
	3. Send an ACK with `ackNo = R`.
 -  **Error-free packet with `seqNo ≠ R` arrives:**
	1. Discard the packet.
	2. Send an ACK with `ackNo = R`.
 -  **Corrupted packet arrives:**
	1. Discard the packet.
	2. **No state change** occurs; remain in the ready state.

`corrupted -> Discard; Do nothing.`

- **Sender:** The sender alternates between the ready and blocking states, depending on whether it has a packet waiting for acknowledgment or if it is ready to send a new packet. The sender manages the flow of data and error control by waiting for ACKs and handling timeouts.
  
- **Receiver:** The receiver remains in a constant ready state, handling incoming packets and sending acknowledgments based on the sequence numbers of received packets.

![[Pasted image 20240824203618.png]]
#### Efficiency
The Stop-and-Wait protocol is very inefficient if our channel is thick and long, means *if it has high bandwidth delay product* 
![[Pasted image 20240824203123.png]]
![[Pasted image 20240824203134.png]]
![[Pasted image 20240824203324.png]]

#### Pipelining
- In networking and in other areas, a task is often begun before the previous task hasended. This is known as pipelining. 
- There is no pipelining in the Stop-and-Wait protocol because a sender must wait for a packet to reach the destination and be acknowledged before the next packet can be sent. 
- Pipelining improves the efficiency of the transmission if the number of bits in transition is large with respect to the bandwidth-delay product.

### Go-Back-N Protocol

The **Go-Back-N (GBN) Protocol** is a sliding window protocol that allows multiple *(N) packets to be sent before receiving acknowledgments*, enhancing transmission efficiency. However, t*he receiver can only buffer one packet at a time*. 

1. **Sequence Numbers:**
   - Sequence numbers in GBN are modulo \(2^m\), where \(m\) is the number of bits in the sequence number field.
   - This allows for a range of sequence numbers from 0 to \(2^m - 1\).

2. **Acknowledgment Numbers:**
   - Acknowledgments are cumulative. The acknowledgment number (`ackNo`) specifies the sequence number of the next expected packet.
   - For example, if `ackNo = 7`, it means that all packets with sequence numbers up to 6 have been received correctly, and the receiver expects the next packet to have a sequence number of 7.
![[Pasted image 20240825021029.png]]
3. **Send Window:**
   - The send window is an abstract concept that defines the range of sequence numbers for packets that can be in transit or can be sent. The window size is fixed at \(2^m - 1\).
   - The send window divides sequence numbers into four regions:
     1. **Already acknowledged:** Packets to the left of the window.
     2. **Outstanding packets:** Sent but not yet acknowledged.
     3. **Ready to be sent:** Packets that can be sent, but the corresponding data is not yet available.
     4. **Unavailable:** Sequence numbers that cannot be used until the window slides.
   - **Variables:**
     - `Sf` (Send window first): Sequence number of the first outstanding packet.
     - `Sn` (Send window next): Sequence number for the next packet to be sent.
     - `Ssize`: Size of the send window, fixed at \(2^m - 1\).
   - The window slides to the right when an *error-free ACK with an `ackNo` between `Sf` and `Sn` arrives*.

4. **Receive Window:**
   - The receive window ensures correct data reception and acknowledgment sending. *It always has a size of 1*.
   - The receiver is always waiting for a specific packet, and *any packet arriving out of order is discarded*.
   - **Variable:**
     - `Rn` (Receive window next): Sequence number of the next expected packet.
   - The receive window slides one slot at a time when a correct packet arrives.

5. **Timers:**
   - GBN uses a *single timer for the first outstanding packet* (`Sf`). If this timer expires, it indicates a failure in receiving acknowledgment for the first outstanding packet.
   - Upon timeout, the sender resends all outstanding packets.

6. **Resending Packets:**
   - When the timer expires, the sender resends all packets from `Sf` to `Sn - 1`.
   - For example, if `Sf = 3` and `Sn = 7`, the sender will resend packets 3, 4, 5, and 6.

#### FSM

![[Pasted image 20240825021546.png]]
##### Sender FSM:
Both variables are initialized to 0 (`Sf = Sn = 0`).

`corrupted -> Discard; Do nothing.`
###### 1. Ready State:

1. **Application Layer Request:**
   - **Action:** The sender creates a packet with the sequence number `Sn`, stores a copy of the packet, and sends it.
   - **Next Step:** 
     - If the timer is not already running, it starts.
     - Increment `Sn`: `Sn = (Sn + 1) modulo 2^m`.
     - *If the window becomes full (`Sn = (Sf + Ssize) modulo 2^m`), transition to the* **Blocking State**.

2. **Error-free ACK Received:**
   - **Action:**
     - Slide the window: `Sf = ackNo`.
     - If all outstanding packets are acknowledged (`ackNo = Sn`), stop the timer.
     - If there are still outstanding packets, restart the timer.

3. **Corrupted or Unrelated ACK Received:**
   - **Condition:** The ACK is either corrupted or its `ackNo` does not match any outstanding packets.
   - **Action:** Discard the ACK.

4. **Timeout Occurs:**
   - **Action:** Resend all outstanding packets and restart the timer.

###### 2. Blocking State:*
While in the blocking state, three possible events can occur:

1. **Error-free ACK Received:**
   - **Action:**
     - Slide the window: `Sf = ackNo`.
     - If all outstanding packets are acknowledged (`ackNo = Sn`), stop the timer.
     - If there are still outstanding packets, restart the timer.
   - **Next Step:** Transition to the **Ready State**.

2. **Corrupted or Unrelated ACK Received:**
   - **Condition:** The ACK is either corrupted or its `ackNo` does not match any outstanding packets.
   - **Action:** Discard the ACK.

3. **Timeout Occurs:**
   - **Action:** Resend all outstanding packets and restart the timer.

##### Receiver FSM:
The variable is initialized to 0 (`Rn = 0`).

1. **Error-free Packet with `seqNo = Rn` Arrives:**
   - **Action:** 
     - Deliver the message in the packet to the application layer.
     - Slide the window: `Rn = (Rn + 1) modulo 2^m`.
     - Send an ACK with `ackNo = Rn`.

2. **Error-free Packet with `seqNo` Outside the Window Arrives:**
   - **Action:** 
     - Discard the packet.
     - Send an ACK with `ackNo = Rn`.

3. **Corrupted Packet Arrives:**
   - **Action:** Discard the packet.

#### Send Window Size

  - The send window size must be less than \(2^m\) to avoid ambiguity in packet identification and to ensure proper operation of the protocol.
  - **Why less than \(2^m\)?**
    - If the send window size equals \(2^m\), the sender could send packets with sequence numbers 0 to \(2^m - 1\) within one cycle.
    - If all ACKs for these packets are lost, the sender may resend packets with the same sequence numbers in the next cycle.
    - **Error Scenario:** The *receiver, expecting packets from the next cycle, might mistakenly accept a retransmitted packet from the previous cycle as a new packet* in the current cycle. This misinterpretation could lead to *data corruption* or *loss*, as the receiver believes it’s receiving a new packet when it’s actually a duplicate.
	![[Pasted image 20240825024516.png]]

---

This is an example of a case where the *forward channel is reliable, but the reverse is not*. No data packets are lost, but some ACKs are delayed and one is lost. The example also shows how *cumulative acknowledgments can help if acknowledgments are delayed or lost*.
![[Pasted image 20240825025302.png]]

Although ACK 2 is lost, ACK 3 is cumulative and serves as both ACK 2 and ACK 3. 

---

What happens when a packet is lost. Packets 0, 1, 2, and 3 are sent. However, packet 1 is lost. The receiver receives packets 2 and 3, but they are discarded because they are received out of order (packet 1 is expected). 
*ACKs are not useful for the sender because the ackNo is equal to Sf, not greater than Sf.*
When the time-out occurs, the sender resends packets 1, 2, and 3, which are acknowledged.
![[Pasted image 20240825030512.png]]

---
### Go-Back-N versus Stop-and-Wait
 
 - The Stop-and-Wait protocol is actually a Go-Back-N protocol in which there are only two sequence numbers and the send window size is 1.
- In other words, m = 1 and 2m − 1 = 1. In Go-Back-N, we said that the arithmetic is modulo 2m; in Stop-and-Wait it is modulo 2, which is the same as 2m when m = 1.

--- 
