- **Address Distribution Problem**:
	- ISPs grant small address ranges to businesses or households.
	- If a business grows or requires more addresses, the ISP may not be able to meet the demand due to adjacent addresses already being allocated.
- **Efficient Address Usage**:
	- In small networks, only a few computers may need simultaneous Internet access, meaning the number of public IP addresses required is fewer than the total number of devices.
	- For example, in a business with 20 computers, only 4 might need Internet access at the same time.
 - **Private and Universal Communication**:
	- The business can use **private IP addresses** for internal communication.
	- A smaller number of **public IP addresses** (provided by the ISP) are used for external communication.
- **Network Address Translation (NAT)**:
	- *NAT allows mapping of private IP addresses to a limited number of public IP addresses for communication with the global Internet.*
	- NAT also supports virtual private networks (VPNs).
- **Private Network Visibility**:
	- *The private network remains invisible to the Internet.*
	- *Global Internet only sees the public IP address of the NAT router*, e.g., **200.24.5.8**. 
- **NAT Router**:
	- A single NAT-enabled router connects the private network to the global Internet.
	- The router uses one **private IP address** for internal communication and one **global IP address** for external communication.
	![[Pasted image 20240926184730.png]]
## Address Translation
- All of the outgoing packets go through the NAT router, which replaces the source address in the packet with the global NAT address.
- All incoming packets also pass through the NAT router, which replaces the destination address in the packet (the NAT router global address) with the appropriate private address.
![[Pasted image 20240926184900.png]]
## Translation Table
- A NAT router uses a translation table to map private IP addresses to external IP addresses, enabling communication between a private network and the global Internet.
- The translation table has two columns: the private IP address and the external destination IP address.
- Translating the source addresses for an outgoing packet is straightforward. But how does the NAT router know the destination address for a packet coming from the Internet?

### One IP Address Strategy
- *The router notes the external destination address when a private network host initiates communication.*
- When the response returns, the router uses the external source address to map the response back to the correct private IP address.
- *Communication must always be initiated by a host within the private network.*
- NAT is commonly used by ISPs that *assign a single global IP address to a customer with many private addresses*.
- For example, when e-mail that originates from outside the network site is received by the ISP e-mail server, it is stored in the mailbox of the customer until retrieved with a protocol such as POP.
- **Drawbacks**:
	- *Only one private host can communicate with a given external host at a time.*
![[Pasted image 20240926193126.png]]
### Using a Pool of IP Addresses
- A pool of global IP addresses allows multiple private hosts to connect to the same external host simultaneously.
- For example, instead of using only one global address (200.24.5.8), the NAT router can use four addresses (200.24.5.8, 200.24.5.9, 200.24.5.10, and 200.24.5.11).In this case, four private-network hosts can communicate with the same external host at the same time because *each pair of addresses defines a separate connection*.
- **Drawbacks**:
	- No more than four connections can be made to the same destination.
	-  No private-network host can access two external server programs (e.g., HTTP and TELNET) at the same time.
	- No two private-network hosts can access the same external server program
### Using IP Addresses and Port Addresses
   - To resolve these limitations, the translation table can include
	   1. **source port addresses**
	   2. **destination port address**
	   3. **transport-layer protocol**
   - *This allows multiple private hosts to access different services (like HTTP, TELNET) on the same external host simultaneously.* ![[Pasted image 20240926194609.png]]
   - For example, two private hosts (172.18.3.1 and 172.18.3.2) can access the same external HTTP server (25.8.3.2) without conflict, as the translation uses unique port numbers.
   - *For this translation to work, the ephemeral port addresses (1400 and 1401) must be unique.*