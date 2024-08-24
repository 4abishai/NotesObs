Electronic mail (e-mail) allows users to exchange messages.
Client/server programming is implemented using some **intermediate computers (servers)**.

The users run only client programs when they want and the intermediate servers apply the client/server paradigm.

![[Pasted image 20240802023739.png]]


![[Pasted image 20240802023754.png]]



- MTA client/server program is a push program.
- MAA client/server program is a pull program.

![[Pasted image 20240802024046.png]]

### User Agent (UA)

The user agent (UA) is the first component of an electronic mail system, providing a service to simplify the process of sending and receiving messages. As a software package, the UA helps users compose, read, reply to, and forward messages, as well as manage local mailboxes on their computers.

#### Types of User Agents:
1. **Command-Driven User Agents:** 
   - Originated in the early days of electronic mail.
   - Accept one-character commands from the keyboard to perform tasks (e.g., `r` to reply, `R` to reply to all).
   - Examples include mail, pine, and elm.

2. **GUI-Based User Agents:**
   - Modern UAs with graphical components like icons, menu bars, and windows.
   - Enable interaction through both keyboard and mouse, making services easy to access.
   - Examples include Eudora and Outlook.

**Sending Mail:**
- Users create mail that resembles postal mail, consisting of an envelope and a message.
- The envelope typically includes sender and receiver addresses and other information.
- The message is divided into a header (containing sender, receiver, subject, etc.) and a body (containing the actual content).
![[Pasted image 20240802024950.png]]

**Receiving Mail:**
- The UA notifies the user when new mail arrives.
- Users can view a summary list of messages, including sender address, subject, and time sent/received.
- Messages can be selected and displayed in full.

**Addresses:**
- Email addresses consist of a local part and a domain name, separated by an `@` sign.
- The local part refers to the user mailbox where received mail is stored.
- The domain name is associated with mail servers or exchangers within an organization, derived from the DNS database or a logical name.
![[Pasted image 20240802025041.png]]

**Mailing List or Group List:**
- An alias representing multiple email addresses.
- The system checks the recipient's name against the alias database and sends separate messages to each address in the mailing list.

### Simple Mail Transfer Protocol (SMTP)

Email communication relies on three instances of the client/server paradigm to function correctly. These instances can be categorized into message transfer agents (MTAs) and message access agents (MAAs).

**Three Client/Server Applications in Email:**
1. **First MTA:** Between the sender's client (user agent) and the sender's mail server.
2. **Second MTA:** Between the sender's mail server and the receiver's mail server.
3. **MAA:** Between the receiver's mail server and the receiver's client (user agent).

**SMTP (Simple Mail Transfer Protocol):**
- SMTP is the formal protocol that *defines the communication between MTA clients and servers on the Internet*.
- It is used twice in the email process:
  1. Between the sender's client and the sender's mail server.
  2. Between the sender's mail server and the receiver's mail server.
- SMTP specifies the format for sending commands and responses between these entities.

**Additional Protocols:**
- While SMTP handles the transfer of messages between servers and from the client to the sender's server, another protocol is required to retrieve the email from the receiver's server to their client (e.g., POP3 or IMAP).

#### SMTP Commands and Responses

In SMTP, the communication between an MTA client and an MTA server is accomplished through a series of commands and responses. Commands are sent from the client to the server, while responses are sent from the server to the client. *Each command or response ends with a carriage return and line feed (CRLF) to signify the end of the line*.

**Commands:**
- Commands follow the format: `Keyword: argument(s)`.
- They consist of a keyword followed by zero or more arguments.
- SMTP defines 14 commands, which include basic operations like starting a mail transaction, specifying the sender and recipient, and terminating the connection.

![[Pasted image 20240802025853.png]]

**Responses:**
- Responses are sent from the server to the client in the format of a three-digit code, optionally followed by additional textual information.
- The three-digit code indicates the status of the previous command.
![[Pasted image 20240802025928.png]]

#### Mail Transfer Phases

##### 1. Connection Establishment
After a client has made a TCP connection to the well known port 25, the SMTP server starts the connection phase. This phase involves the following three steps:
1. The server sends code 220 (service ready) to tell the client that it is ready to receive mail. If the server is not ready, it sends code 421 (service not available).
2. The client sends the HELO message to identify itself, using its domain name address. This step is necessary to inform the server of the domain name of the client.
3. The server responds with code 250 (request command completed) or some other code depending on the situation.

##### 2. Message Transfer
After a connection has been established between the SMTP client and server, a single message between a sender and one or more recipients can be exchanged. This phase involves eight steps. Steps 3 and 4 are repeated if there is more than one recipient.
1. The client sends the MAIL FROM message to introduce the sender of the message. It includes the mail address of the sender (mailbox and the domain name). This step is needed to give the server the return mail address for returning errors and reporting messages.
2. The server responds with code 250 or some other appropriate code.
3. The client sends the RCPT TO (recipient) message, which includes the mail address of the recipient.
4. The server responds with code 250 or some other appropriate code.
5. The client sends the DATA message to initialize the message transfer.
6. The server responds with code 354 (start mail input) or some other appropriate message.
7. The client sends the contents of the message in consecutive lines. Each line is terminated by a two-character end-of-line token (carriage return and line feed). The message is terminated by a line containing just one period.
8. The server responds with code 250 (OK) or some other appropriate code.

##### 3. Connection Termination
After the message is transferred successfully, the client terminates the connection. This phase involves two steps.
1. The client sends the QUIT command.
2. The server responds with code 221 or some other appropriate code.

![[Pasted image 20240802030422.png]]

### Message access agent: POP and IMAP
-  The direction of the bulk data is from the server to the client. 
- The third stage uses a message access agent.
- SMTP is used for the first and second stages of mail delivery, as it is a push protocol that sends messages from the client to the server. However, SMTP is not used in the third stage because this stage requires a pull protocol, where the client retrieves messages from the server.

#### POP3 (Post Office Protocol)
POP3 (Post Office Protocol, version 3) is a simple mail retrieval protocol with limited functionality. It involves client software installed on the recipient's computer and server software on the mail server. When a user wants to download email, the client connects to the server on TCP port 110, sends a username and password, and can then list and retrieve messages. 

POP3 operates in two modes:

1. **Delete mode:** Mail is deleted from the server after retrieval, typically used when accessing mail from a permanent computer.
2. **Keep mode:** Mail remains on the server after retrieval, used for accessing mail from different locations, such as a laptop, allowing for later retrieval and organization. 

-  It does not allow the user to organize her mail on the server.
- The user cannot have different folders on the server. 
#### IMAP4 (Internet Messaging Access Protocol)
IMAP4 provides the following extra functions:
- A user can check the e-mail *header* prior to downloading.
- A user can search the *contents of the e-mail* for a specific string of characters prior to downloading.
- A user can *partially download* e-mail. This is especially useful if bandwidth is limited and the e-mail contains multimedia with high bandwidth requirements.
- A user can *create, delete, or rename mailboxes* on the mail server.
- A user can create a hierarchy of mailboxes in a *folder* for e-mail storage.

### MIME (Multimedia Internet Main Extensions)
Electronic mail has a simple structure and can only send messages in NVT 7-bit ASCII format. This limits its use to languages using the Latin alphabet and prevents it from sending binary files, video, or audio data. 

To address these limitations, Multipurpose Internet Mail Extensions (MIME) is used as a supplementary protocol. MIME allows non-ASCII data to be sent via email by transforming it into NVT ASCII data at the sender's site, which is then sent through the Internet. At the receiving site, the data is transformed back to its original format. MIME can be thought of as a set of software functions that convert non-ASCII data to ASCII data and vice versa.

![[Pasted image 20240807201614.png]]

#### MIME defines five headers:
![[Pasted image 20240807201646.png]]

##### Content-Type:
Depending on the subtype, the header may contain other parameters. MIME allows seven different types of data.
![[Pasted image 20240807201817.png]]

##### Content-Transfer-Encoding:
This header defines the method used to encode the messages into 0s and 1s for transport.
![[Pasted image 20240807201925.png]]

**1. Base64 encoding**: Data, as a string of bits, are first divided into 6-bit chunks. Each 6-bit section
is then converted into an ASCII character.
	![[Pasted image 20240807202110.png]]
	![[Pasted image 20240807202128.png]]
 **2. Quoted-Printable**: Used when  the data -consist mostly ofASCII characters with a small non-ASCII portion.
	![[Pasted image 20240807202413.png]]
##### Content-Description:
This header defines whether the body is image, audio, or video.

### Web-Based Mail
