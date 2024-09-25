1. **Packetizing**: 
   - Encapsulates payloads from the upper layers into packets.
   - Adds headers with source/destination addresses.
   - Decapsulates packets at the destination and delivers the payload.
   - Handles fragmentation if the packet is too large for transmission.

2. **Routing**:
   - Determines the best path for packets from source to destination.
   - Uses routing protocols to coordinate routing tables before communication occurs.

3. **Forwarding**:
   - Moves packets across networks by using forwarding tables.
   - Uses the packet's destination address to determine the appropriate output interface for packet forwarding.

4. **Error Control**:
   - Provides limited error control through a header checksum, but not for the full packet.
   - ICMP is used to report errors such as unknown headers or discarded packets.

5. **Flow Control**:
   - The network layer in the Internet does not directly handle flow control.

6. **Congestion Control**:
   - The network layer does not handle congestion control directly in the Internet.

7. **Quality of Service (QoS)**:
   - QoS is crucial for supporting multimedia communication (audio, video, etc.).

8. **Security**:
   - The network layer was originally designed without security.
   - Modern networks have additional layers like IPSec to ensure security and transform the connectionless network service into a more secure, connection-oriented service.