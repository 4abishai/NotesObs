![[Pasted image 20240826201032.png]]
### Blocking Primitives (Synchronous Primitives):
   - These are message-passing primitives where the *calling process is suspended* until the operation is complete.
   - **Send operation**:
     - When a process calls send, it specifies a destination and a buffer to send.
     - The process is *blocked (suspended) until the message has been completely sent*.
     - The instruction following the send call is not executed until the message transmission is complete.
   - **Receive operation**:
     - The *process remains suspended in receive until a message arrives*, even if it takes hours.
     - In some systems, the receiver can specify from whom it wishes to receive, remaining blocked until a message from that specific sender arrives.

### Nonblocking Primitives (Asynchronous Primitives)
- These are primitives that return control to the caller immediately, before the operation is complete.
- **Send operation**:
	- If send is nonblocking, it returns control to the caller immediately, before the message is sent.
 
- ***Advantage***: The sending process can *continue computing in parallel* with the message transmission, improving *CPU utilization*.
   
- ***Disadvantage***:
	- The sender cannot modify the message buffer until the message has been sent.
	- *Risk of overwriting the message* during transmission if the buffer is modified too soon.
	- The sending process has *no idea when the transmission is done*, making it difficult to know when it's safe to reuse the buffer.
#### Solutions to the nonblocking send problem:
   **a. Kernel copy method:**
  - The *kernel copies the message to an internal kernel buffer* before returning control to the process.
  - From the sender's perspective, this behaves like a blocking call.
  - The process is free to reuse the buffer immediately after regaining control.
  - ***Disadvantage***: Every outgoing message must be copied from user space to kernel space, which can be inefficient.
   
**b. Interrupt method:**
- The system *interrupts the sender when the message has been sent*.
- This informs the process that the buffer is available again.
- No copy is required, saving time and memory.
-  ***Disadvantage***:
	- User-level interrupts are difficult to program and subject to race conditions.
	- Makes programs hard to debug and can lead to irreproducible errors.

### Preferred choices:
   - Under normal conditions, blocking send is considered the best choice:
     - Simple to understand and implement.
     - Doesn't require kernel buffer management.
     - Generally faster if no copy is required.
   - If overlapping processing and transmission are essential, nonblocking send with copying is recommended.

### Alternative view of synchronous vs. asynchronous primitives:
   - Some authors define a synchronous primitive as one where the sender is blocked until the receiver has accepted the message and sent an acknowledgement.
   - In this view, everything else is considered asynchronous.
   - This definition focuses on the complete end-to-end transmission, including receiver acknowledgement.
### Nonblocking receive:
   - Returns control almost immediately, telling the kernel where the buffer is.
   - **Challenges**: How does the caller know when the operation has completed?
   - **Solutions**:
	 - Provide an explicit ***wait primitive*** that allows the receiver to block when it wants to.
	 - Offer a ***test primitive*** to allow the receiver to poll the kernel to check on the status.
	 - Implement a ***conditional_receive***, which either gets a message or signals failure, but in any event returns immediately.
	 - ***Interrupts*** can also be used to signal completion of a receive operation.
	
### Timeouts:
   - In systems where send calls block, there's a risk of infinite blocking if no reply comes.
   - To prevent this, many systems allow the caller to specify a time interval within which it expects a reply.
   - If no reply arrives within that interval, the send call terminates with an error status.