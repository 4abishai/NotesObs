In the client/server paradigm, communication occurs between two running application programs: a client and a server. The client initiates communication by sending a request, while the server waits for and handles these requests, sending back the results. The server must always be running to accept client requests, but the client only needs to run when it has a request to send. Typically, the server runs indefinitely, while the client runs only long enough to send requests and receive responses before stopping.

### Application Interface
A *client process communicates with a server* process using an Application Programming Interface (API). An API is a set of instructions that *allows a process to interact with the lower layers* of the TCP/IP protocol suite (transport, network, data link, physical) to open connections, send and receive data, and close connections. This interface acts as a *bridge between the application layer process and the operating system*, which handles the first four layers of the TCP/IP protocol. Computer manufacturers build these layers into the operating system and provide an API, enabling application layer processes to communicate over the Internet. Common APIs for communication include the **socket interface**, **transport layer interface (TLI)**, and **STREAM**.

### Socket Interface
- The socket interface, is a *set of instructions that facilitate communication between the application layer and the operating system*. 
- This interface allows processes to communicate with each other using a set of instructions similar to those used for other input and output operations in programming languages like C, C++, and Java. 

![[Pasted image 20240730013406.png]]
### Sockets
- Sockets are *abstractions*, not physical entities, and act like *terminals* or *files*. 
- They are *data structures* created and used by application programs to manage communication. 
- In the context of application layer communication, a client process communicates with a server process through two sockets, one at each end. The client and server perceive the sockets as the *entities that handle requests and responses*. By creating sockets with the correct source and destination addresses, and using standard instructions to send and receive data, the underlying operating system and TCP/IP protocol handle the rest of the communication process.

![[Pasted image 20240730013553.png]]

### Socket Addresses
In client-server communication, a pair of socket addresses is needed to establish a two-way connection. Each socket address comprises two components:

1. **IP Address**: Identifies the specific computer on the Internet. It is a 32-bit integer in the current version of the Internet Protocol (IPv4).

2. **Port Number**: Identifies a specific process or application running on that computer. It is a 16-bit integer.

A socket address combines these two elements—an IP address and a port number—to *uniquely identify both the sending and receiving processes* in a network communication.

![[Pasted image 20240730013926.png]]

