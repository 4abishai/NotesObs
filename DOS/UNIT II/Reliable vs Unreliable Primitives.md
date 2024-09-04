### Reliable vs Unreliable Primitives

1. **Initial Assumption**:
   - The initial assumption is that when a client sends a message, the server will always receive it. However, reality is more complex, as messages can be lost. This introduces complications in the semantics of the message-passing model.

2. **Blocking Primitives**:
   - Consider a situation where blocking primitives are used. When the client sends a message, it is suspended until the message has been sent.
   - If the client is restarted during this process, there is no guarantee the message was delivered, and the message might be lost.

3. **Three Approaches to the Problem**:
   - **Approach 1**: 
	   - The first approach is to redefine the semantics of the `send` function to be unreliable. The system then gives no guarantee that messages will be delivered. 
	   - This is the simplest solution but sacrifices reliability.
   
   - **Approach 2**: 
	   - Reliable communication can be implemented by introducing an acknowledgment mechanism. 
	   - The sender waits for an acknowledgment to ensure the message was received.
	   - Same goes for server.
	   - This guarantees delivery but introduces complexity and potential delays. 
	   - *Acknowledgement goes from kernel to kernel* server and client never sees ACK.
   
  ![[Pasted image 20240826223842.png]] 
   - **Approach 3**: 
	   - The third approach takes advantage of the fact that client-server communication often follows a request-reply model.
	   - Instead of sending a separate acknowledgment for the message, *the reply itself acts as an implicit acknowledgment*.
	   - *The sender remains blocked until the reply arrives*. If the reply is lost, the client can resend the request. This approach works well for certain types of requests, like reading a block of data.
	 **Handling Lost Messages**:
	   - If a request's reply is lost (such as a file block request), the client can repeat the request, and the server will resend the block. No significant damage is done, and only minimal time is lost.
	 **High-Cost Computations**:
	   - For requests that require extensive computation on the server's part, it may be inefficient to discard the computed answer from the server if the reply (acting as ACK) is not received by the client. In such cases, an explicit acknowledgment from the client’s kernel to the server's kernel can be used, ensuring that the server does not discard the computed answer prematurely.
	   ![[Pasted image 20240828115529.png]]
	   