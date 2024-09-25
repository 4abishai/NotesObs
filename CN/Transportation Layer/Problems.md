![[Pasted image 20240907002045.png]]

![[Pasted image 20240907003057.png]]
![[Pasted image 20240907003116.png]]

![[Pasted image 20240907003558.png]]

![[Pasted image 20240907004825.png]]

![[Pasted image 20240907010054.png]]
![[Pasted image 20240907010019.png]]
![[Pasted image 20240907013703.png]]
![[Pasted image 20240907013724.png]]

![[Pasted image 20240907014730.png]]
![[Pasted image 20240907014810.png]]

![[Pasted image 20240907032559.png]]
![[Pasted image 20240907032542.png]]

![[Pasted image 20240907043908.png]]
![[Pasted image 20240907043932.png]]

![[Pasted image 20240907132723.png]]
![[Pasted image 20240907132737.png]]
![[Pasted image 20240907133224.png]]
![[Pasted image 20240907133317.png]]
![[Pasted image 20240907135126.png]]
![[Pasted image 20240907135136.png]]
### Conditional GET
**Conditional-GET** is an HTTP feature that helps save bandwidth. Here’s how it works:
1. **Initial Request**: The client fetches a resource and receives headers like `Last-Modified` or `ETag`.
2. **Subsequent Requests**: The client requests the resource with `If-Modified-Since` or `If-None-Match` headers to check if the resource has changed.
3. **Server Response**:
   - **304 Not Modified**: If the resource hasn’t changed, the server only responds with a status code, saving bandwidth.
   - **200 OK**: If the resource has changed, the server sends the updated resource.

**Benefits**: Reduces unnecessary data transfer and speeds up loading times.

### Piggybacking
1. **Data and ACK Combined**: When a device (e.g., a sender) has data to send and needs to acknowledge received data, it sends both the acknowledgment (ACK) and new data in the same message.
2. **Efficiency**: Instead of sending a separate acknowledgment packet and then a data packet, piggybacking combines these into a single packet, reducing the number of packets transmitted over the network.
3. **Usage**: This technique is often used in protocols like TCP, where ACKs and data packets can be combined to reduce overhead and improve throughput.

![[Pasted image 20240907173109.png]]
1. **Propagation Time**: This is the time it takes for a signal to travel from the sender to the receiver over a physical medium. close to the speed of light in the medium).
2. **Transmission Time**: This is the time required to push all the packet's bits onto the wire. It depends on the size of the packet and the bandwidth of the link.

![[Pasted image 20240907173208.png]]
