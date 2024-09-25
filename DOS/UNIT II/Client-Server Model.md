1. **OSI Model Inefficiency:**
   - Layered protocols like OSI generate *significant overhead* because of multiple layers that need to process and add headers to the data.
   - This overhead slows down communication, especially in wide-area networks (WANs), though it can be managed with efficient CPUs in some cases.
   - For local-area networks (LANs), the overhead is more substantial, leading to wasted CPU time and underutilization of network capacity.
   - Many LAN-based systems avoid using the full OSI stack or only use parts of it to reduce the overhead.

2. **OSI Model’s Limitation:**
   - The OSI model primarily focuses on *delivering bits from sender to receiver* but *does not address how a distributed system should be structured*.
   - This leads to the need for alternative models that are more effective for distributed systems.

3. **Client-Server Model Overview:**
   - The client-server model *organizes a system by separating it into client processes and server processes*.
   - *Servers offer services to clients (e.g., reading a block of a file), and clients request these services*.
   - This model can run on one machine or be distributed across multiple machines, where each machine could run one or many processes.
![[Pasted image 20240826163227.png]]
4. **Efficiency Through Connectionless Protocols:**
   - Unlike OSI’s connection-oriented approach, the client-server model often uses a connectionless request/reply protocol, reducing overhead.
   - The client sends a request and waits for a reply, with no need to establish or tear down a connection before or after.
   - This simplicity reduces the protocol stack, improving efficiency by eliminating unnecessary layers.

5. **Advantages of Simplicity:**
   - Only three levels of protocol are typically needed, handling physical and data link layers, as well as the request/reply protocol layer.
   - There’s no need for session management or routing, further streamlining the process.
![[Pasted image 20240826163244.png]]
6. **Implementation in Systems:**
   - Communication can be handled by system calls that manage sending and receiving messages between processes.
   - The system calls block processes until messages arrive, using pointers to manage where data is sent and received.

### Addressing

#### 1. Hardwire machine.number  to client code

   - A scenario where a server is assigned a numerical address (e.g., 243).
   - This address could refer to either a specific machine or a specific process on that machine. 
   - The issue arises when the address refers only to a machine but multiple processes are running on it. The kernel won’t know which process should handle the incoming message, leading to potential conflicts or errors.
   - To solve the ambiguity, the text suggests using a two-part naming scheme, such as `243.4` or `4@243`.
   - In this scheme, the first part (`243`) specifies the machine number, while the second part (`4`) specifies the process number on that machine.
   - This allows the kernel to identify both the correct machine and the exact process that should receive the message.
   - Another alternative is using a `machine.local-id` format instead of `machine.process`.
     - The `local-id` is typically a randomly chosen identifier (e.g., a 16-bit or 32-bit integer) that uniquely identifies a process on the machine.
     - The process tells the kernel to listen for messages addressed to its specific `local-id`.

 **Drawback**
   - Although `machine.process` addressing can work, it has significant drawbacks, especially in distributed systems.
   - This approach is not transparent to the user because the server's location (the specific machine it runs on) is hardcoded into the address.
   - This lack of transparency becomes a problem if the server’s machine (e.g., machine 243) becomes unavailable. 
   - If another machine (e.g., machine 176) is available, programs that were compiled with the hardcoded address 243 will fail to connect to the new machine, leading to system downtime or the inability to access services. 
   - Therefore, the text highlights that while `machine.process` addressing can function, it’s not ideal for creating a flexible and resilient distributed system.
#### 2. Centralized process address allocator
  
   - Assigns each process a unique address without an embedded machine number
   - Maintains a counter, incrementing it for each new process

**Drawback**
   - Doesn't scale well to large systems

#### 3. Random address selection
   - Each process picks its own identifier from a large address space (e.g. 64-bit integers)
   - Very low probability of collisions
   - Scales well

**Drawback**
   - How does the sending kernel know which machine to send to?

#### 4. Broadcasting with special locate packet
   - Sender broadcasts a locate packet with the destination process address
   - All machines check if the address is theirs
   - If so, they send back a "here I am" message
   - Sender caches the address to avoid future broadcasts
   
   **Drawback**
   - Puts extra load on the system

#### 5. ASCII server names
   - Processes referred to by ASCII strings instead of binary numbers
   - Client sends a query to a name server to get the machine number
   - Once obtained, request can be sent directly
   - Addresses can be cached

   **Drawback**
   - ASCII names require a centralized name server
	   - Consistency problems.

#### 6. Alternative approach
   - Use special hardware
   - **Network interface chips** store process addresses
   - Frames use process addresses instead of machine addresses
   - Network chip examines frame to see if the destination process is local
   ![[Pasted image 20240826173502.png]]
