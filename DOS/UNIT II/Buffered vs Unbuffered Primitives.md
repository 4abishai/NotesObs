![[Pasted image 20240826202827.png]]
## Unbuffered Primitives:
   
   - An address refers to a specific process.
   - A call receive(addr, &m) tells the kernel:
     - The calling process is listening to address addr.
     - It's prepared to receive one message sent to that address.
   - A single message buffer (m) is provided to hold the incoming message.
   - When a message arrives, the receiving kernel copies it to the buffer and unblocks the receiving process.

#### Problems with Unbuffered Primitives:
   - *Works fine if the server calls receive before the client sends another message*.
   - Issues arise when send is done before receive:
     - The server's kernel doesn't know which process (if any) is using the address in the newly arrived message.
     - It doesn't know where to copy the message.

#### Implementation Strategies for Unbuffered Primitives:
**a. Discard and Retry**:
- Discard the message if no receive is pending.
- Let the client time out and retransmit.
- The client may need to try several times before succeeding.
- Risk: *The client's kernel may give up after multiple failures*, falsely concluding the server has crashed or the address is invalid.
   
**b. Temporary Buffering:**
- The receiving kernel keeps incoming messages around for a short while.
- *A timer is started when an unwanted message arrives.*
- If the timer expires before a suitable receive happens, the unwanted message is discarded.
- This reduces the chance of discarding messages but introduces the problem of managing prematurely arriving messages.

## Buffered Primitives (Mailboxes):
   
   - A new data structure called a mailbox is defined.
   - A process interested in receiving messages tells the kernel to create a mailbox.
   - It specifies an address to look for in network packets.
   - *All incoming messages with same address are put in the mailbox.*
   - *A receive call now removes one message* from the mailbox or blocks if none is present.
   - This technique is referred to as a buffered primitive.

#### Advantages and Limitations of Mailboxes:
   - Mailboxes appear to eliminate race conditions caused by discarded messages and clients giving up.
   - *However, when a mailbox is full, the kernel must choose between keeping the message around or discarding it.*
   - This presents the same choices as in the unbuffered case, just at a different point.
	   - When space becomes available, the sender is allowed to try again.
