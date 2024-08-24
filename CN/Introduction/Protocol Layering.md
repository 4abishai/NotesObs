**Protocol** defines the rules that both the sender and receiver and all intermediate devices need to follow to be able to communicate effectively. 
When communication is simple, we may need only one simple protocol; when the communication is complex, we may need to *divide the task between different layers*, in which case we need a protocol at each layer, or **protocol layering**.

- Protocol layering enables us to divide a complex task into several smaller and simpler tasks.  This is referred to as modularity. **Modularity** in this case means *independent layers*.
- One of the advantages of protocol layering is that it allows us to separate the servic-es from the implementation. A layer needs to be able to receive a set of services from the lower layer and to give the services to the upper layer; we don’t care about how the layer is implemented.
- Communication does not always use only two end systems; there are intermediate systems that need only some layers, but not all layers. If we did not use protocol layering, we would have to make each intermediate system as complex as the end systems, which makes the whole system more expensive.
- One can argue that having a single layer instead of protocol layering makes the job easier. 

# Principles of Protocol Layering
### First Principle
The first principle dictates that *if we want bidirectional communication*, we need to make each layer so that it is able to perform *two opposite tasks*, one in each direction.
Eg- A layer needs to be able to encrypt and decrypt.
### Second Principle
The second important principle that we need to follow in protocol layering is that the *two objects under each layer at both sites should be identical*.

# Logical Connections
It means having layer to layer connection.