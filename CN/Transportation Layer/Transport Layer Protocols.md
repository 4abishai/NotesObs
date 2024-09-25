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
	- As the window size is 1.
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
![[Pasted image 20240825111738.png]]
##### Sender FSM:

`corrupted -> Discard; Do nothing.`
`out of order -> Discard; Do nothing.`

#### Receiver FSM:

`corrupted -> Discard; Do nothing.`
`out of order -> Discard; Send ACK = Rn`

#### Send Window Size

  - The send window size must be less than \(2^m\) to avoid ambiguity in packet identification and to ensure proper operation of the protocol.
  - **Why less than \(2^m\)?**
    - If the send window size equals \(2^m\), the sender could send packets with sequence numbers 0 to \(2^m - 1\) within one cycle.
    - If all ACKs for these packets are lost, the sender may resend packets with the same sequence numbers in the next cycle.
    - **Error Scenario:** ***The receiver, expecting packets from the next cycle, might mistakenly accept a retransmitted packet from the previous cycle as a new packet in the current cycle***. This misinterpretation could lead to *data corruption* or *loss*, as the receiver believes it’s receiving a new packet when it’s actually a duplicate.
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
### Selective Repeat Protocol
The Selective-Repeat (SR) Protocol is designed to address the inefficiencies of the Go-Back-N (GBN) protocol, particularly in situations where packet loss is frequent. 

1. **Selective Resending:**
   - Unlike GBN, where a single lost packet results in the retransmission of all subsequent packets, *SR retransmits only the specific packet that was lost or corrupted*. This selective resending reduces unnecessary data transmission, making SR more efficient, especially in high-loss environments.

2. **Send and Receive Windows:**
   - Both the sender and receiver maintain windows, but with key differences compared to GBN:
     - **Send Window:** 
       - The size of the send window in SR is \(2^{m-1}\), where \(m\) is the number of bits in the sequence number. For instance, if \(m = 4\), the sequence numbers range from 0 to 15, but the maximum size of the window is 8.
       - ***This reduced window size ensures that even if packets arrive out of order, they can be properly managed without ambiguity.***
     - **Receive Window:**
       - The receive window is the same size as the send window.

3. **Timer Mechanism:**
   - Ideally, SR would use a separate timer for each outstanding packet, which would allow for precise retransmission of only the lost packet when its timer expires. However, in practice, many implementations use a single timer for simplicity, similar to GBN.

4. **Acknowledgments (ACKs):**
   - **Cumulative vs. Individual ACKs:**
     - In GBN, ACKs are cumulative; the receiver acknowledges the highest sequence number of packets received in order, implying all previous packets have been received correctly.
     - In SR, each ACK corresponds to a specific packet. This means that the receiver sends an ACK for each correctly received packet, regardless of whether it is in order or not. This allows the sender to know exactly which packets need to be retransmitted.
![[Pasted image 20240825160025.png]]
#### Advantages of Selective-Repeat Protocol:

- **Efficiency:** SR is more bandwidth-efficient than GBN in scenarios where packet loss is high, as it avoids unnecessary retransmissions.
- **Reliability:** The protocol ensures that out-of-order packets are handled correctly, maintaining the integrity and order of data delivery.
- **Flexibility:** SR is better suited for networks with variable or unpredictable packet loss, as it allows for more granular control over retransmissions.

#### Disadvantages:

- **Complexity:** SR is more complex to implement than GBN, particularly in managing multiple timers and handling out-of-order packets at the receiver.
- **Memory Usage:** The receiver must have enough buffer space to store out-of-order packets until the missing packets are received.

#### FSM
![[Pasted image 20240825161345.png]]

---
- A reliable transport layer promises to deliver packets in order.
	- At the receiver site we need to distinguish between the acceptance of a packet and its delivery to the application layer. 
- There are two conditions for the delivery of packets to the application layer:
	1. *A set of consecutive packets must have arrived*. 
	2. *The set starts from the beginning of the window.* 

At the second arrival, packet 2 arrives and is stored and marked (shaded slot), but it cannot be delivered because packet 1 is missing. At the next arrival, packet 3 arrives and is marked and stored, but still none of the packets can be delivered. Only at the last arrival, when finally a copy of packet 1 arrives, can packets 1, 2, and 3 be delivered to the application layer. 
![[Pasted image 20240825161444.png]]

---
#### Window Sizes in Selective-Repeat Protocol
  
  - For \(m = 2\), the sequence numbers range from 0 to \(2^m - 1 = 3\).

1. **Window Size = 2:**
   - **Scenario:**
     - Suppose all acknowledgments are lost.
     - The sender's timer for packet 0 expires, and it resends packet 0.
   - **Receiver's Perspective:**
     - The receiver's window is now expecting packet 2.
     - Since the sequence number 0 is not in the receiver's window, the duplicate packet 0 is correctly discarded.
     - This ensures that no erroneous data is accepted.

2. **Window Size = 3:**
   - **Scenario:**
     - Again, assume all acknowledgments are lost.
     - The sender resends packet 0.
   - **Receiver's Perspective:**
     - The receiver's window is expecting packet 0 (because 0 is part of the window when the size is 3).
     - The receiver mistakenly accepts this duplicate packet 0 as a new packet in the next cycle.
   - **Error:** This situation introduces a significant error because the receiver cannot distinguish between a retransmitted packet and a new one in the next cycle. This results in the wrong data being processed.
	![[Pasted image 20240825163529.png]]
---
