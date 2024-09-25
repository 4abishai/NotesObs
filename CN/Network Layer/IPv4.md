## IPV4 Datagram Format
![[Pasted image 20240924233132.png]]
##### HLEN
- The 4-bit header length (HLEN) field defines the total length of the datagram header in 4-byte words
- To make the value of the header length (number of bytes) fit in a 4-bit header length, the total length of the header is calculated as 4-byte words.
-  The total length is divided by 4 and the value is inserted in the field. 
- The receiver needs to multiply the value of this field by 4 to find the total length.
##### Total Length
-  A 16-bit number can define a total length of up to 65,535 (when all bits are 1s)
##### Time-to-live
- When a source host sends the datagram, it stores a number in this field. This value is approximately two times the maximum number of routes between any two hosts.
- Each router that processes the datagram decrements this number by one. If this value, after being decremented, is zero, the router discards the datagram.
##### Protocol
- When the payload is encapsulated in a datagram at the source IP, the corresponding protocol number is inserted in this field; when the datagram arrives at the destination, the value of this field helps to define to which protocol the payload should be delivered.
- This fields provides multiplexing at the source and demultiplexing at the destination.
##### Header checksum
- IP is not a reliable protocol; it does not check whether the payload carried by a datagram is corrupted during the transmission. 
- IP adds a header checksum field to check the header, but not the payload. 
- Since the value of some fields, such as TTL, which are related to fragmentation and options, may change from router to router, the checksum needs to be recalculated at each route.

## Fragmentation
- As a datagram travels through different networks, routers decapsulate and re-encapsulate it into different frames.
- The format and size of these frames depend on the physical network's protocol.
- Each network protocol has a maximum payload size (**Maximum Transfer Unit - MTU**) that a frame can encapsulate.
- MTU values vary across networks; for example, a typical LAN has an MTU of 1500 bytes.
- The IP protocol allows datagrams up to 65,535 bytes, but these may need to be fragmented when the datagram exceeds the MTU of the underlying network.
- When a datagram is too large for a network's MTU, it is fragmented into smaller pieces.
- Each fragment gets its own header, copying most fields, but certain fields like flags, fragmentation offset, and total length are updated.
- Fragments can be further fragmented if they encounter a smaller MTU along their path.
- *Reassembly* of fragments occurs only at the destination host.
- This is because fragments may take different routes, and reassembling them before they reach the final destination would reduce efficiency.
### Header Changes in Fragmentation
- When fragmentation occurs, most of the IP header is copied for each fragment, except for the flags, fragmentation offset, and total length, which are updated.
- The checksum is recalculated for each fragment.

### Fields Related to Fragmentation in IP Datagrams
#### 1. Identification Field
- This 16-bit field uniquely identifies the datagram from the source host.The combination of the identification and source IP address must uniquely define a datagram as it leaves the source host.
- When the IP protocol sends a datagram, it assigns a unique identification number using a counter. This number is copied into all fragments when fragmentation occurs.
- The identification number helps the destination reassemble fragments by identifying which fragments belong to the same datagram.
- The identification number helps the destination in reassembling the data- gram. It knows that all fragments having the same identification value should be assembled into one datagram.
#### 2. Flags Field
- This is a 3-bit field, with the following key bits:
- **Reserved bit** (leftmost): Not used.
- **D (Do Not Fragment) bit**: 
	- If set to 1, the datagram cannot be fragmented. If fragmentation is required but the D bit is set, the datagram is discarded, and an ICMP error message is sent to the source.
	- If set to 0, fragmentation is allowed.
- **M (More Fragments) bit**: 
	- If set to 1, it indicates that more fragments follow.
	- If set to 0, it indicates that the current fragment is the last one.
#### 3. Fragmentation Offset Field
- This 13-bit field shows the relative position of a fragment compared to the original datagram.
- IP datagrams can be up to **65,535 bytes** in size. 8191 is too small to directly represent positions for a 65,535-byte datagram using just 13 bits.
	$$ \frac{65535}{8} = 8191.875$$
- 13 bits can represent 0 to 8191.
- The offset it measured in terms of **8 bytes**.
$$ {8191} \times {8} $$
- The offset helps the destination correctly reassemble fragments in the correct order, even if they arrive out of sequence.
### Example:
- **Datagram Fragmentation**: A 4000-byte datagram is split into three fragments:
1. **First fragment**: Carries bytes 0 to 1399; its offset is 0 (0/8 = 0).
2. **Second fragment**: Carries bytes 1400 to 2799; its offset is 175 (1400/8 = 175).
3. **Third fragment**: Carries bytes 2800 to 3999; its offset is 350 (2800/8 = 350).

- **Fragmentation of a Fragment**: If a fragment is further fragmented, the offset field continues to refer to the original datagram. For instance, if the second fragment (offset 175) is fragmented into two parts (800 bytes and 600 bytes), the offsets of the new fragments are still relative to the original datagram.

![[Pasted image 20240925020400.png]]
### Reassembly Process:
The destination reassembles the original datagram using the following steps:
1. The first fragment has an offset field value of zero.
2. Divide the length of the first fragment by 8. The second fragment has an offset value equal to that result.
3. Divide the total length of the first and second fragment by 8. The third fragment has an offset value equal to that result.
4. Continue the process. The last fragment has its M bit set to 0.

## IP Security
Packet modification and IP spoofing are common network attacks, but IPSec (IP Security) provides robust countermeasures against these and similar threats.
### Types of Attacks:
1. **Packet Modification**:
   - In this attack, an attacker intercepts and alters the contents of a packet before sending it to the intended receiver, who believes it came from the legitimate sender.
   - **Prevention**: A **data integrity mechanism** can be used to verify that the packet hasn't been tampered with during transmission. If the data integrity check fails, the receiver discards the packet.
2. **IP Spoofing**:
   - Here, an attacker forges the source address of an IP packet to masquerade as another user or system. For instance, an attacker could send a packet to a bank appearing to come from a legitimate customer.
   - **Prevention**: This attack can be mitigated using **origin authentication**, which ensures the packet is genuinely from the claimed source.
### IPSec Protocol:
IPSec is a security protocol used in conjunction with the IP protocol to provide a secure, connection-oriented service that protects against the above attacks. IPSec provides four main services:
1. **Algorithm and Key Agreement**:
   - Two entities that want to communicate securely establish a shared set of algorithms and encryption keys. This agreement forms the foundation for secure communications.
2. **Packet Encryption**:
   - Encryption ensures that any intercepted packets are unreadable without the proper decryption key. This prevents packet sniffing (unauthorized interception of packets) from being successful.
3. **Data Integrity**:
   - Ensures that packets remain unchanged during transmission. If a packet is altered in any way, it fails the integrity test and is discarded by the receiver, thus protecting against **packet modification**.
4. **Origin Authentication**:
   - Verifies the sender's identity, ensuring that the packet truly comes from the claimed source. This protects against **IP spoofing** by verifying that the packet is not sent by an imposter. 

## IPV4 addressing
   - An IPv4 address is a 32-bit identifier used to uniquely and universally define the connection of a device (host or router) to the Internet.
   - The address refers to the connection, not the device itself, as it may change when the device is moved to a different network.
   - Each IPv4 address uniquely defines one connection to the Internet.
   - Devices with multiple network connections will have multiple IPv4 addresses.
   - The IPv4 address system is accepted universally by any Internet-connected device.
   - IPv4 has a 32-bit address space, meaning 2^32 possible addresses (4,294,967,296).
   - This allows for over 4 billion unique connections to the Internet.
   - IPv4 addresses are expressed in three notations:
     1. **Binary notation**: 32 bits, divided into 4 octets (8 bits each).
     2. **Dotted-decimal notation**: Each octet is written in decimal form, separated by dots (e.g., 192.168.1.1).
     3. **Hexadecimal notation**: Common in network programming, uses 8 hex digits to represent 32 bits.
- **Hierarchy in Addressing**:  
	 1. **Prefix**: Defines the network (n bits).
	 2. **Suffix**: Defines the node or device connection ((32 - n) bits).
6. **Addressing Schemes**:
	- **Classful Addressing** (Obsolete): Used a fixed-length network prefix.
	- **Classless Addressing**: Uses a variable-length prefix, which is the modern method.
### Addressing schemes
#### 1. Classful Addressing
- IPv4 originally used a fixed-length prefix to accommodate different network sizes.
- The address space was divided into **five classes** (A, B, C, D, E), each with varying prefix lengths to serve different types of networks.
- **Class A**:
	- First bit is always 0, so only 7 bits are used for the network prefix.
	- Allows for 128 networks, each supporting 16,777,216 hosts (large networks).
- **Class B**:
	- First two bits are (10)₂, leaving 14 bits for the network prefix.
	- Allows for 16,384 networks, each supporting 65,536 hosts (medium networks).
- **Class C**:
	- First three bits are (110)₂, leaving 21 bits for the network prefix.
	- Allows for 2,097,152 networks, each supporting 256 hosts (small networks).
- **Class D**: Used for multicast; no division into prefix and suffix.
- **Class E**: Reserved for future use, also not divided into prefix and suffix.
- **Address Depletion**:  
	- Large blocks like Class A and B were underutilized, leading to wasted addresses.
	- Class C addresses were too small for many organizations, and Class E addresses were unused.
- **Subnetting and Supernetting**:
	- **Subnetting**: Divides large networks (Class A/B) into smaller subnets with a longer prefix, but large organizations resisted sharing unused addresses for the benefit of smaller organizations.
	- **Supernetting**: Combines multiple Class C networks into a larger block, but made routing more complex.
- **Advantage of Classful Addressing**:  
	- Easy identification of address class and prefix length due to the fixed structure of the prefix for each class. No additional information was required to distinguish the network part from the host part. 
![[Pasted image 20240925184348.png]]

#### 2. Classless Addressing
-  **Motivation for Classless Addressing (CIDR)**:  
	- **Address depletion**: Subnetting and supernetting in classful addressing didn't solve the problem, and a larger address space was needed.  
	- **ISPs and Flexibility**: ISPs needed more flexibility in address allocation for individuals and businesses, which led to the introduction of classless addressing in 1996.  
	- Short term fix, as IPV6 was later developed.
- **Variable-Length Blocks**:  
	- Classless addressing uses blocks of addresses with variable lengths instead of fixed classes (A, B, C).  
	- Organizations can be allocated blocks of varying sizes (e.g., 1, 2, 4, 128, etc.), depending on their needs.
	- Each block is defined by a **prefix** (network identifier) and a **suffix** (node/device identifier).  
	- Prefix is the part of the address that remains the same for all devices within the same network.
		- In CIDR, the prefix length is flexible and defines how many bits of the 32-bit IPv4 address are dedicated to identifying the network.
	- Suffix is used to identify specific devices (nodes) within that network.
	- Number of addresses in a block needs to be a power of 2. We can have a block of 2^0, 2^1, 2^2, . . . , 2^32 addresses. 
	- The block sizes must be a power of 2, and the prefix length can range from 0 to 32 bits.
	- An organization can be granted one block of addresses.
	![[Pasted image 20240925193001.png]]
	-  *The size of the network is inversely proportional to the length of the prefix.* A small prefix means a larger network; a large prefix means a smaller network.
- **Flexibility and Efficiency**:  
	- Classless addressing allows efficient use of IP addresses by adjusting the prefix length to fit the required number of addresses.  
	- A smaller prefix length corresponds to a larger network, while a larger prefix length corresponds to a smaller network.
- **Slash Notation (CIDR)**:  
	- In classless addressing, the prefix length is indicated separately using **slash notation** or **CIDR (Classless Interdomain Routing)**.  
	![[Pasted image 20240925193854.png]]
	- In the address `192.168.1.0/24`, the prefix is the first 24 bits (`192.168.1`), which identifies the network. The last 8 bits (the suffix) identify individual devices in that network.
- **Extracting Information from an Address**:  
   Given an address within a block:  
	   1. **Number of addresses**: `N = 2^(32−n)` where `n` is the prefix length.  
	   2. **First address**: Set the (32−n) rightmost bits to 0.  
	   3. **Last address**: Set the (32−n) rightmost bits to 1.  
   ![[Pasted image 20240926002046.png]]
![[Pasted image 20240925201605.png]]
![[Pasted image 20240925201640.png]]
- **Address Mask**:
	- The **address mask** is a 32-bit number where the leftmost `n` bits are 1s (for the prefix) and the remaining (32−n) bits are 0s (for the suffix). 
	- It helps in extracting information about the block using bitwise operations:
	     1. **Number of addresses**: `N = NOT (Mask) + 1`.
	     2. **First address**: `(Any address in the block) AND (Mask)`.
	     3. **Last address**: `(Any address in the block) OR [(NOT (Mask)]`.
![[Pasted image 20240925204454.png]]
- **Network Address**:
	- The **network address** is the first address in the block and is essential for routing.  
	- When a packet arrives at a router, the router uses the network address to determine which network (or interface) the packet should be forwarded to. This is achieved through a forwarding table. ![[Pasted image 20240926002114.png]]
- **Block Allocation**:
	- The **Internet Corporation for Assigned Names and Numbers (ICANN)** is responsible for block allocation. ICANN assigns large blocks to ISPs, which in turn allocate smaller blocks to their customers. ![[Pasted image 20240925232821.png]]
	- An ISP requests a block of 1000 IP addresses, but since 1000 is not a power of 2, the ISP is allocated 1024 addresses, which is the next power of 2 (since $$ 1024 = 2^{10}\ $$ 
	- The number of addresses \(N = 1024\).
	- The formula to calculate the prefix length (n) is: $$ n = 32 - \log_2(N) = 32 - \log_2(1024) = 32 - 10 = 22 $$
	- Therefore, the prefix length is **22 bits**. This means the first 22 bits of the IP address represent the network, and the remaining 10 bits can be used for the individual host addresses.
	- The ISP is assigned the block **18.14.12.0/22**.
	- In this case, the **prefix length** is 22, which means the first 22 bits represent the network, and the block contains 1024 addresses (since \(2^{32-22} = 1024\)).
	- The first address in the block is **18.14.12.0**.
	- To find the decimal value of this address, convert it from dotted decimal to binary and then to a decimal number:
	- **18.14.12.0** in binary is: ` 00010010.00001110.00001100.00000000 `
	- Convert this binary number to decimal: $$ (18 \times 256^3) + (14 \times 256^2) + (12 \times 256^1) + (0 \times 256^0) = 302,910,464 $$
	- The decimal value of the first address is **302,910,464**.
	- To ensure proper block allocation, the first address must be divisible by the number of addresses in the block $$ \frac{302,910,464}{1024} = 295,613 \, (\text{an integer value}) $$
	- Since **302,910,464** is divisible by **1024**, it confirms that the block is properly aligned.
- **Subnetting**:
	- Subnetting allows an organization or ISP to further divide its block of addresses into smaller subranges, each representing a subnetwork (or subnet).
	- This creates multiple levels of hierarchy, allowing more efficient use of address space.
     1. The number of addresses in each subnetwork must be a power of 2.
     2. The prefix length for each subnetwork, `nsub` (subnet mask), is found using the formula: $$ n_{\text{sub}} = 32 - \log_2 N_{\text{sub}} $$
     3. The starting address of each subnetwork must be divisible by the number of addresses in that subnetwork. ![[Pasted image 20240926002234.png]] ![[Pasted image 20240926003507.png]]
- **Address aggregation**
	- **Address aggregation** (or summarization) is one of the key benefits of the **CIDR** strategy.
	- By combining smaller blocks of addresses into a larger block, routing can be simplified. The router only needs to consider the prefix of the larger block, reducing the size of routing tables.
	- ISPs often use address aggregation when assigning subblocks to customers, ensuring that routing remains efficient. ![[Pasted image 20240926005220.png]]	 

