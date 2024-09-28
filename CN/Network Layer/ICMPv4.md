- **Lack of error handling in IPv4**: IPv4 lacks mechanisms for error reporting or correcting.
- **Common issues**:
	- Datagram discarded by routers (no route, TTL expiration).
	- Final host discards fragments due to incomplete reception.
- **IP protocol limitation**: No built-in method to notify the original sender about errors.
- **Lack of query mechanisms**: IPv4 cannot check if a host or router is alive, or allow network management queries.
- **ICMPv4 solution**: *Designed to address error-reporting and query deficiencies in IPv4.*
- **Companion to IP protocol**: ICMPv4 operates at the network layer.
- **Encapsulation**: ICMP messages are encapsulated in IP datagrams before being sent to the data-link layer, with the protocol field set to 1 to indicate an ICMP message.

## ICMPv4 Messages
- **Two Categories**:
	1. **Error-reporting messages**: Report issues encountered by a router or host during IP packet processing.
	2. **Query messages**: Help hosts or network managers obtain specific information from other hosts or routers (e.g., discovering neighbors or redirecting messages).
### Message Format
- **Header**: 8-byte header common to all ICMP messages.
	- **First 4 bytes** are the same for all messages:
		1. **Type (8 bits)**: Defines the message type.
		2. **Code (8 bits)**: Specifies the reason for the message.
		3. **Checksum (16 bits)**: Error-checking for the message.
		4. **Remaining header**: Specific to the message type.
- **Data Section**:
	- **Error messages**: Contains information to locate the original packet that caused the error.
	- **Query messages**: Contains extra information depending on the query type.
### Error Reporting Message
- **Purpose**: *ICMP reports errors that occur during IP datagram processing.* It does not correct errors; correction is handled by higher-level protocols.
- **Key Rules for Error Reporting**:
	- No error messages for *multicast or special addresses* (e.g., loopback).
	- No error messages in response to *another ICMP error message*.
	- No error messages for *fragments* except the first fragment of a datagram.
- **Data Section**: Error messages include:
	- Original *IP header* and the *first 8 bytes of the original datagram’s data* (helpful for identifying port numbers or TCP sequence numbers).
- **Common Error Messages**:
	- **Destination Unreachable (Type 3)**: Indicates datagram could not reach its destination (e.g., "host unreachable").
	- **Source Quench (Type 4)**: Informs the sender of network congestion, advising to slow down transmissions (congestion control).
	- **Redirection (Type 5)**: Informs the source that it sent a message to the wrong router, and provides the correct router’s IP.
	- **Time Exceeded (Type 11)**: Occurs when the TTL value reaches 0 (code 0) or when all fragments of a datagram do not arrive in time (code 1).
	- **Parameter Problem (Type 12)**: Reports issues with the datagram’s header (code 0) or missing/invalid options (code 1). ![[Pasted image 20240927214524.png]]
### Query Message
- **Purpose**:
	- Probe/test the *liveliness* of hosts or routers.
	- Measure one-way or *round-trip time* between devices.
	- Check if clocks between two devices are *synchronized*.
- **Independent Use**: Query messages can be used independently, without requiring an IP datagram, although they are encapsulated in a datagram for transmission.
- **Request-Reply Pairs**: Query messages come in request-reply pairs.
- **Common Query Messages**:
	- **Echo Request (Type 8)** and **Echo Reply (Type 0)**: Test if a host or router is alive. Commonly used in tools like **ping** and **traceroute**.
	- **Timestamp Request (Type 13)** and **Timestamp Reply (Type 14)**: Measure round-trip time or check clock synchronization between devices. The request sends a 32-bit timestamp, and the reply includes two additional timestamps for calculating time differences.

#### Ping command
- Using ping command a host can send an echo request (type 8, code 0) message to another host, which, if alive, can send back an echo reply (type 0, code 0) message. 
![[Pasted image 20240927233745.png]]
![[Pasted image 20240927233757.png]]
- **Ping Command**: 
	- **Host pinged**: `auniversity.edu` with IP `152.181.8.3`.
	- **Bytes sent/received**: 56 bytes of data (84 bytes including ICMP header).
- **Ping Responses**:
	- 6 packets sent to `auniversity.edu`.
	- **Sequence numbers** (`icmp_seq=0` to `icmp_seq=5`) indicate the order of packets sent and received.
	- **TTL (Time-to-Live)**: 62, the number of hops the packet can take before being discarded.
	- **RTT (Round-trip time)**: Times for each packet are between **1.90 ms** and **2.04 ms**.
		 - `time=1.91 ms` (first ping)
		 - `time=2.04 ms`, `1.90 ms`, `1.97 ms`, `1.93 ms`, and `2.00 ms` (subsequent pings).
- **Summary**:
	- **6 packets transmitted**, **6 received**, **0% packet loss**.
	- **RTT statistics**: 
	     - **Minimum RTT**: 1.90 ms
	     - **Average RTT**: 1.95 ms
	     - **Maximum RTT**: 2.04 ms
#### Traceroute Program
- *Traces the path of a packet* through multiple routers from a source to its destination by setting the Time-to-Live (TTL) field in the IP header.
- The ping program gets help from two query messages; the traceroute program gets help from two error-reporting messages: *time-exceeded* and *destination-unreachable*.
- It is an application layer program but only the client program is needed, because, as we can see, the client program never reaches the application layer in the destination host. In other words, there is no traceroute server program.
- Encapsulated in *UDP datagram*.
- Uses port number that is not available at the destination. 
	![[Pasted image 20240927235744.png]]
-  **Traceroute Functionality**: 
	-  If there are n routers in the path, the traceroute program sends (n + 1) messages. The first n messages are discarded by the n routers, one by each router; the last message is discarded by the destination host. 
	- The traceroute program does not need to know the value of n; it is found automatically.
	- Traceroute sends UDP packets with increasing TTL values. 
	- **Router**
		- Each router decreases the TTL by 1. When TTL reaches 0, the packet is discarded, and the router sends back a "time-exceeded" ICMP message. 
		- The traceroute uses this information to identify each router (the source IP address of the error message) and the router name (in the data section of the message) along the path.
	- **Final Destination**: 
		-  If traceroute message reaches the destination host. This host is also dropped, but for another reason. *The destination host cannot find the port number specified in the UDP user datagram.* This time ICMP sends a different message, the destination-unreachable message with code 3 to show the port number is not found. This lets traceroute know the destination has been reached.
- **Round-Trip Time (RTT)**
	-  Most traceroute programs send three messages to each device, with the same TTL value, to be able to find a better estimate for the round-trip time.
##### Example Trace
- **Command**: `$ traceroute printers.com`
	![[Pasted image 20240928001055.png]]
- **Result**:
   1. First hop (router 1): `route.front.edu` with RTT values: 0.622 ms, 0.891 ms, 0.875 ms.
   2. Second hop (router 2): `ceneric.net` with RTT values: 3.069 ms, 2.875 ms, 2.930 ms.
   3. Third hop (router 3): `satire.net` with RTT values: 3.071 ms, 2.876 ms, 2.929 ms.
   4. Final destination: `alpha.printers.com` with RTT values: 5.922 ms, 5.048 ms, 4.922 ms.

The example demonstrates tracing the path of packets across three routers with varying RTTs at each hop.