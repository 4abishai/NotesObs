### Transmission Delay
- Delay caused by *putting the bits in a packet on the line* one by one. 
- If the *first bit* of the packet is put on the line at time t1 and the *last bit* is put on the line at time t2, transmission delay of the packet is (t2 − t1).
- Definitely, the transmission delay is longer for a longer packet and shorter if the sender can transmit faster.

$$\text{Delay}_{\text{tr}} = \frac{\text{packet length}}{\text{transmission rate}}$$
### Propagation Delay
- Propagation delay is the time it takes for *a bit to travel from point A to point B in the transmission media*. 
- The propagation delay for a packet-switched network depends on the *propagation delay of each network (LAN or WAN)*. The propagation delay depends on the *propagation speed of the media*, which is 3 × 10^8 m/s in a vacuum and normally much less in a wired medium; it also depends on the *distance of the link*.

$$\text{Delay}_{\text{pg}} = \frac{\text{distance}}{\text{propagation speed}}$$
### Processing Delay
- The processing delay is the time required for a router or a destination host to *receive a packet from its input port*, *remove the header*, perform an *error-detection procedure*, and *deliver the packet to the output port* (in the case of a router) or *deliver the packet to the upper-layer protocol* (in the case of the destination host).

$$\text{Delay}_{\text{pr}} = \text{ time required to process a packet in a router or a destination host}$$

### Queuing Delay
- The queuing delay for a packet in a router is measured as the *time a packet waits in the input queue and output queue of a router*. 
- Queuing delay can normally happen in a router. A router has an input queue connected to each of its input ports to store packets waiting to be processed; the router also has an output queue connected to each of its output ports to store packets waiting to be transmitted.

$$
\text{Delay}_{\text{qu}} = \text{time a packet waits in input and output queues in a router}
$$

### Total Delay
- Assuming equal delays for the sender, routers, and receiver, the total delay (source-to-destination delay) a packet encounters can be calculated if we know the *number of routers*, n, in the whole path.
- If we have n routers, we have *(n + 1) links*. Therefore, we have *(n + 1) transmission delays* related to n routers and the source, *(n + 1) propagation delays* related to (n + 1) links, *(n + 1) processing delays* related to n routers and the destination, and only n queuing delays related to n routers.

$$
\text{Total delay} = (n + 1) (\text{delay}_{\text{tr}} + \text{delay}_{\text{pg}} + \text{delay}_{\text{pr}}) + (n) (\text{delay}_{\text{qu}})
$$
