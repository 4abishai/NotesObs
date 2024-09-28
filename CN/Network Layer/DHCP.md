- **Address Assignment**: DHCP (Dynamic Host Configuration Protocol) allows automatic assignment of IP addresses to hosts and routers in a network.
- **Client-Server Paradigm**: DHCP is an application-layer protocol that works in a client-server architecture, aiding the network layer (TCP/IP).
- **Plug-and-Play Protocol**: Widely used on the Internet, it is often referred to as a plug-and-play protocol.
- **Permanent and Temporary Addresses**: DHCP can assign both permanent and temporary IP addresses.
	- **Permanent**: For hosts and routers.
	- **Temporary**: Useful for situations like travelers connecting to a hotel network or ISPs providing dynamic services.
		- It also allows an ISP with 1000 granted addresses to provide services to 4000 households, assuming not more than one-forth of customers use the Internet at the same time. 
- **Efficiency for ISPs**: An ISP can provide services to more users than the number of available IP addresses by dynamically assigning temporary addresses.
- **Four Key Pieces of Information Provided by DHCP**:
	- Computer's IP address
	- Network prefix (address mask)
	- Default router’s address
	- Name server’s address (for DNS)
## DHCP Message Format
![[Pasted image 20240926145739.png]]
- **Client-Server Protocol**: DHCP operates in a client-server model where the client sends a request, and the server responds.
- DHCP messages consist of several fields, including important information for network configuration. One crucial field is the option field.
	- **Option Field**:
		- Purpose: It can carry *additional* or *vendor-specific information*.
		- Size: 64 bytes.
		- Format:
		   1. **Tag Field** (1 byte): Defines the option type.
		   2. **Length Field** (1 byte): Specifies the length of the value.
		   3. **Value Field** (variable length): Holds the option value.
	- **Tag Field 53**: This tag is used for defining DHCP message types. These message types guide the operation of DHCP. 
	![[Pasted image 20240926141009.png]] 
	- **Magic Cookie**:
		- A unique number used by the server, with the value **99.130.83.99**.
		- Helps the *client identify the start of the options data* within the message.
## DHCP operation
![[Pasted image 20240926163152.png]]
## DHCP well known ports
- DHCP uses two well-known ports:
	- **Port 67**: For the DHCP server.
	- **Port 68**: For the DHCP client.
- **Broadcast Nature**: DHCP server responses are broadcasted to all hosts on the network, which can lead to conflicts if different applications use the same temporary port number.
- **Conflict Prevention**: DHCP uses the well-known port 68 for the client, rather than an ephemeral port, to prevent conflicts in message delivery.
- **Conflict Example**:
	- If a **DHCP client** and a **DAYTIME client** (another service) both use the same temporary port (e.g., 56017), both would receive the DHCP server's response, confusing the DAYTIME client.
- **Handling Multiple DHCP Clients**: If two DHCP clients are running simultaneously (e.g., after a power outage), their responses are differentiated by the **transaction ID**, ensuring correct message delivery to each client.

## Transition States
![[Pasted image 20240926182952.png]]
1. **INIT State**: 
	- The client starts in the initializing state (INIT).
	- It broadcasts a **DHCP Discover** message to find available DHCP servers.
2. **SELECTING State**:
	- After receiving offers from servers, the client moves to the SELECTING state.
	- The client may receive multiple offers in this state and selects one.
3. **REQUESTING State**:
	- Once an offer is selected, the client sends a **DHCP Request** message to the server.
	- The client then enters the REQUESTING state.
4. **BOUND State**:
	- If the server responds with an **ACK message**, the client moves to the BOUND state and begins using the assigned IP address.
5. **Lease Renewal**:
	- When 50% of the lease time expires, the client moves to the **RENEWING state** and sends a request to renew the lease.
	- If the lease is renewed (ACK received), the client returns to the BOUND state.
6. **REBINDING State**:
	- If the lease is not renewed and 75% of the lease time expires, the client moves to the **REBINDING state**.
	- If the server renews the lease at this point, the client moves back to the BOUND state. If not, the client returns to the INIT state and requests a new IP address.
- **IP Address Usage**: The client can only use the IP address in three states: **BOUND**, **RENEWING**, and **REBINDING**.
- **Three Timers**:
	- **Renewal Timer**: Set to 50% of the lease time (for lease renewal attempts).
	- **Rebinding Timer**: Set to 75% of the lease time (if renewal fails).
	- **Expiration Timer**: Set to the full lease time (when the IP lease fully expires).