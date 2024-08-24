### World Wide Web
- The Web functions as a vast, distributed repository of information, where web pages are interconnected through links, allowing for expansive growth without overloading servers.
- This linking is based on hypertext, a concept predating the Internet, enabling automatic retrieval of documents via clickable links. The term has evolved to hypermedia, encompassing not just text but also images, audio, and video. 
- Beyond document retrieval, the Web now supports activities like online shopping, gaming, and on-demand access to radio and television programs.

![[Pasted image 20240801193737.png]]
#### Architecture
The World Wide Web (WWW) operates as a distributed client/server service. Clients use browsers to access services hosted on servers distributed across various locations called sites. Each site contains one or more web pages, which can either be simple (without links to other pages) or composite (containing links to other pages within the same or different sites). Every web page is essentially a file with a specific name and address.

### Web Client (Browser)
Web browsers, offered by various vendors, interpret and display web pages using a similar architecture consisting of three main components: a controller, client protocols, and interpreters.

- **Controller**: Manages input from the keyboard or mouse, uses client programs to access documents, and employs interpreters to display the accessed documents on the screen.
- **Client Protocols**: Include protocols like HTTP or FTP used to retrieve documents.
- **Interpreters**: Depending on the document type, interpreters can be HTML, Java, or JavaScript.

![[Pasted image 20240801202355.png]]

### Web Server
- A web server *stores web pages* and *sends the corresponding documents to clients* upon request. 
- To enhance efficiency, *servers often cache* frequently requested files in memory, as accessing memory is faster than accessing a disk. 
- Additionally, servers can use *multithreading* or *multiprocessing* to handle *multiple requests simultaneously*, further improving performance.

### Uniform Resource Locator
A Uniform Resource Locator (URL) *uniquely identifies a web page* and consists of four parts: protocol, host, port, and path.

1. **Protocol**: Specifies the client/server application to be used, such as HTTP (common) or FTP.
2. **Host**: Identifies the server, either by IP address (e.g., 64.23.56.17) or domain name (e.g., forouzan.com).
3. **Port**: A 16-bit integer *identifies server application port*, typically predefined (e.g., port 80 for HTTP). If a different port is used, it must be specified explicitly.
4. **Path**: Indicates the *file's location* and name within the server's file system, formatted according to the operating system (e.g., /top/next/last/myfile in UNIX).

These components are combined in a URL format:
- Standard: `protocol://host/path`
- With port: `protocol://host:port/path`

### Web Documents
Web documents on the World Wide Web can be categorized into three types: static, dynamic, and active.

1. **Static Documents**:
    - **Description**: These have fixed content, created and stored on the server. The client can only access a copy; the *content is determined at creation* and *not altered by user interaction*.
    - **Languages Used**: HTML, XML, XSL, XHTML.
    - **Example**: An HTML page containing text and images.

2. **Dynamic Documents**:
    - **Description**: Created by the server *in response to a client's request*. The content can vary with each request, as it is generated at the time of the request.
    - **Technologies**: Common Gateway Interface (CGI) in the past, now typically Java Server Pages (JSP) and other scripting languages.
    - **Example**: A web page displaying the current date and time can be altered.

3. **Active Documents**:
    - **Description**: These require a program or script to run on the client's side, enabling interactivity or animation.
    - **Technologies**: Java applets, JavaScript.

Each type of document serves different purposes, ranging from simple information display to interactive applications.

### Hypertext Transfer Protocol (HTTP)
The Hypertext Transfer Protocol (HTTP) is used to facilitate the retrieval of web pages from the Web by defining how client/server programs communicate.

- **Client/Server Interaction**: The HTTP client sends a request, and the HTTP server responds.
- **Port Numbers**: The server uses port number 80, while the client uses a temporary port number.
- **Underlying Protocol**: HTTP relies on TCP, a connection-oriented and reliable protocol.
  - **Connection-Oriented**: A connection must be established between the client and server before any transaction.
  - **Reliability**: TCP ensures the integrity and delivery of messages, handling errors and losses, so the client and server don't need to manage these issues directly.
- **Connection Lifecycle**: Connections are established before transactions and terminated afterward.

When retrieving web page documents that contain embedded hypertext, multiple requests and responses might be necessary. If the *objects are on different servers, new TCP connections must be created for each object*. If some *objects are on the same server, there are two options*:

#### Nonpersistent Connections
In nonpersistent connections, *a new TCP connection is established for each request/response*. The steps are:
1. The client opens a TCP connection and sends a request.
2. The server sends the response and closes the connection.
3. The client reads the data until an end-of-file marker is encountered, then closes the connection.

For a file with links to \( N \) different pictures on the same server, the connection must be opened and closed \( N + 1 \) times. This strategy imposes *high overhead on the server* due to the need for *\( N + 1 \) different buffers* each time a connection is opened.

- For each connection, TCP requires at least three handshake messages to establish the connection, but the request can be sent with the third one. 
- After the connection is established, the object can be transferred. 
- After receiving an object ( response ), another three handshake messages are needed to terminate the connection.

![[Pasted image 20240801220228.png]]
#### Persistent Connections
HTTP version 1.1 specifies persistent connections by default. In persistent connections:
- The server keeps the *connection open for more requests after sending a response*.
- The server can close the connection at the **client's request** or after a **time-out**.
- The sender typically includes the *data length with each response*. If the length is unknown (e.g., dynamic or active documents), the server informs the client and closes the connection after sending the data, signaling the end of the data.

Persistent connections save time and resources by:
- Reducing the need for **multiple sets of buffers and variables** at each site.
- Saving the **round-trip time** for connection establishment and termination.

![[Pasted image 20240801220733.png]]

#### Request Formats
##### HTTP Request Message
The HTTP request message consists of several components that define the type of request and additional information. In HTTP version 1.1, several methods are defined, each serving different purposes:
###### Request Methods
1. **GET**: 
   - Used to retrieve data from the server.
   - The body of the message is empty.
2. **HEAD**: 
   - Requests metadata about a web page (e.g., last modified date).
   - The response has a header but no body.
   - Used to test URL validity.
3. **PUT**: 
   - Allows the client to upload a new web page to the server.
   - Requires permission from the server.
4. **POST**: 
   - Similar to PUT, but used to send data to the server to be added to or modify a web page.
5. **TRACE**: 
   - Used for debugging.
   - The server echoes back the request to verify receipt.
6. **DELETE**: 
   - Allows the client to delete a web page on the server if permitted.
7. **CONNECT**: 
   - Initially reserved for future use.
   - Often used by proxy servers.
8. **OPTIONS**: 
   - Asks the server about the properties of a web page.
###### Components of the Request Line
1. **Method Field**: Specifies the request type (e.g., GET, POST).
2. **URL**: Defines the address and name of the requested web page.
3. **Version**: Specifies the version of the HTTP protocol (e.g., HTTP/1.1).

###### Request Header Lines
- The request line can be followed by zero or more header lines.
- Each header line provides additional information from the client to the server.
- Format: `Header-Name: Header-Value`
- Example: A client might request a document in a specific format.
- Header values and their details are defined in the relevant RFCs (Request for Comments).
![[Pasted image 20240801230404.png]]
![[Pasted image 20240801230423.png]]
![[Pasted image 20240801230009.png]]
##### HTTP Response Message
An HTTP response message consists of four main components: a status line, header lines, a blank line, and optionally a body. Here is a detailed breakdown of each component:
###### 1. Status Line
The status line is the first line in a response message and includes three fields:
- **Version**: The HTTP version, currently HTTP/1.1.
- **Status Code**: A three-digit code indicating the status of the request:
- **Status Phrase**: A textual description of the status code (e.g., "OK" for 200, "Not Found" for 404).
###### 2. Header Lines
Following the status line are zero or more header lines. Each header line provides additional information from the server to the client. The format for a header line is:
- **Header-Name: Header-Value**
###### 3. Blank Line
A blank line separates the header lines from the body of the response message.
###### 4. Body (Optional)
The body contains the actual content of the response (e.g., HTML, JSON). It is optional and depends on the nature of the response. The body is present unless the response is an error message.

![[Pasted image 20240801230855.png]]

![[Pasted image 20240801232517.png]]

![[Pasted image 20240801232731.png]]

**Conditional Request**
A client can add a condition in its request. In this case, the server will send the requested
web page if the condition is met or inform the client otherwise. 
One of the most common conditions imposed by the client is the time and date the web page is modified. The client can send the header line `If-Modified-Since` with the request to tell the server that *it needs the page only if it is modified after a certain point in time*.

### Cookies
The World Wide Web was originally designed to be **stateless**, meaning each *client-server interaction was independent*. However, modern web applications require maintaining state to support various functionalities such as *e-commerce, restricted access, personalized content*, and *advertising*. To achieve this, the cookie mechanism was devised.
#### Creating and Storing Cookies
1. **Server Stores Information**: When a server receives a request from a client, it stores information about the client, which may include the client's domain name, the contents of the cookie (such as user name and registration number), a timestamp, and other relevant data.
2. **Server Sends Cookie**: The server includes the cookie in its response to the client.
3. **Client Stores Cookie**: Upon receiving the response, the client's browser stores the cookie in a directory sorted by the server's domain name.
#### Using Cookies
When a client sends a request to a server, the browser checks the cookie directory for a cookie from that server. If found, the cookie is included in the request. The server then recognizes the client as a returning user. The browser never reads or discloses the cookie's contents, which are **created and used solely by the server**.
#### Applications of Cookies
1. **E-commerce**: Cookies track items selected by the client and added to an electronic cart. Each time an item is added, the cookie is updated. When the client checks out, the cookie contains all the necessary information to calculate the total charge.
2. **Restricted Access**: Websites requiring registration send a cookie to the client upon registration. On subsequent visits, only clients with the appropriate cookie are granted access.
3. **Web Portals**: When a user selects favorite pages on a portal, a cookie is created. Upon return visits, the cookie informs the server of the user's preferences.
4. **Advertising**: Advertising agencies use cookies to track user interactions with banner ads. Each time a user clicks on a banner, a cookie with the user's ID is sent, allowing the agency to compile a profile of the user's web behavior. This data can be sold to other parties, raising privacy concerns.
#### Controversy and Privacy
The use of cookies by advertising agencies to track user behavior and sell profiles has been controversial. There are calls for regulations to protect user privacy and ensure responsible use of cookies.

![[Pasted image 20240802001657.png]]
##### Initial Request and Response
1. **Client Request**: The shopper's browser (client) sends a request to the BestToys server.
2. **Server Creates Shopping Cart**: The server creates an empty shopping cart for the client and assigns it an ID (e.g., 12343).
3. **Server Response**: The server sends a response message to the client. This message includes:
   - Images of all toys available, each with a link to select the toy.
   - A `Set-Cookie` header line with the value `12343`.

##### Storing the Cookie
4. **Client Stores Cookie**: The client stores the cookie value (12343) in a file named BestToys. The cookie is not revealed to the shopper.

##### Selecting a Toy
5. **Client Request with Cookie**: When the shopper selects a toy and clicks on it, the client sends a new request to the server. This request includes the ID 12343 in the `Cookie` header line.
6. **Server Identifies Client**: The server checks the `Cookie` header, finds the value 12343, and recognizes the returning shopper. It retrieves the shopping cart associated with ID 12343 and adds the selected toy to the cart.

##### Updating the Cart and Requesting Payment
7. **Server Response**: The server sends another response to the shopper, displaying the total price and requesting payment information.
8. **Client Sends Payment Information**: The shopper provides her credit card details and sends another request with the ID 12343 as the cookie value.
9. **Server Processes Order**: The server checks the ID 12343, processes the payment, and sends a confirmation response.

### Web Caching: Proxy Server

A **proxy server** is a computer that stores copies of responses to recent requests to reduce the load on the original server, decrease traffic, and improve latency. When an HTTP client sends a request to a proxy server, it first checks its cache. If the response is not found, the proxy server forwards the request to the original server and stores the incoming response for future use. The client must be configured to access the proxy server instead of the target server.
#### Functioning as Server and Client
The proxy server acts as:
- **Server**: When it has the requested response in its cache, it sends it directly to the client.
- **Client**: When it does not have the requested response, it sends the request to the original server and then forwards the response to the client.
#### Proxy Server Locations
Proxy servers are typically located at the client site and can be arranged in a hierarchy:
1. **Client Computer**: Acts as a small-capacity proxy server for frequently accessed requests.
2. **Company LAN**: A proxy server on the LAN reduces external load.
3. **ISP Network**: An ISP can install a proxy server to manage load from its customers.
#### Cache Update Strategies
The duration for which a response remains in the proxy server cache varies based on:
- **Fixed Schedules**: Some sites update their content at regular intervals (e.g., daily news updates).
- **Headers**: Information such as the last modification time can help the proxy server determine the validity of cached data.

![[Pasted image 20240802002813.png]]
### HTTP Security
HTTP per se does not provide security. However, HTTP can be run over the **Secure**
**Socket Layer (SSL)**. In this case, HTTP is referred to as **HTTPS**. HTTPS provides *confidentiality*, *client and server authentication*, and *data integrity*.

