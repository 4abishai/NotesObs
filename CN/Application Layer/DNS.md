For host to name an IP address:
1. The user passes the host name to the file transfer client.
2. The file transfer client passes the host name to the DNS client.
3. Each computer, after being booted, knows the address of one DNS server. The DNS client sends a message to a DNS server with a query that gives the file transfer server name using the known IP address of the DNS server.
4. The DNS server responds with the IP address of the desired file transfer server.
5. The DNS client passes the IP address to the file transfer server.
6. The file transfer client now uses the received IP address to access the file transfer server.

![[Pasted image 20240807211819.png]]
### Name Space
It is a *binding between names and IP addresses.*
To ensure unambiguous identification, names assigned to machines must come from a name space with complete control over the binding. *Names must be unique*, just like the addresses they correspond to. There are two ways to organize such a name space: flat or hierarchical.

- **Flat Name Space:** 
  - Names are a *sequence of characters without structure*.
  - Names may or may not share a common section, which, if present, has no inherent meaning.
  - The primary disadvantage is its unsuitability for large systems like the Internet because it *requires centralized control* to avoid ambiguity and duplication.

- **Hierarchical Name Space:** 
  - Names consist of several parts, each defining *different levels of the hierarchy*.
  - *Decentralized control* is possible:
    - A central authority assigns the first part of the name.
    - The organization itself can manage the rest of the name, adding suffixes or prefixes to define hosts or resources.
  - Ambiguity is avoided as *even if part of an address is the same, the whole address remains unique*. For instance:
    - If two organizations each have a computer named "caesar," the full names might be "caesar.first.com" and "caesar.second.com," ensuring uniqueness.

### Domain Name Space
*Implementation of hierarchical name space* is implemented through a domain name space, which is structured as an *inverted tree with the root at the top*. This structure allows for systematic and decentralized management of domain names, ensuring uniqueness and clear organization.

The tree can have only 128 levels: level 0 (root) to level 127.
![[Pasted image 20240807232129.png]]
#### Label
- Each node in the tree has a label, which is a string with a maximum of *63 characters*.
- The *root label is a null string* (empty string). 
- DNS requires that *children of a node have different labels*, which guarantees the uniqueness of the domain names.

#### Domain Name

Each node in a domain name space tree has a domain name, composed of a *sequence of labels separated by dots* (.). These names are *read from the node up to the root*, with the last label being the root's label (null). This means a full domain name ends in a dot because the null label represents nothing. 

- **Fully Qualified Domain Name (FQDN):** 
  - A domain name ending in a null label (a dot).
  - Example: `example.com.`

- **Partially Qualified Domain Name (PQDN):** 
  - A domain name not ending in a null label.
  - Used when the *name to be resolved is within the same site as the client*, allowing the resolver to supply the missing part, known as the *suffix*, to create an FQDN.
  - Example: `example` (assuming the resolver knows the domain should be `example.com`).

![[Pasted image 20240807232213.png]]
#### Domain
A domain is a subtree of the domain name space. The name of the domain is the name
of the node at the top of the subtree. 
![[Pasted image 20240807232327.png]]
### Distribution of Name Space

Storing the information contained in the domain name space on a single computer is inefficient and unreliable. Centralized storage places a heavy load on the system and any failure makes the data inaccessible. To address these issues, the information is distributed among many computers called DNS servers. 

#### Distribution Method

`root -> domains -> sub-domains`

1. **Divide into Domains:** 
   - The root stands alone.
   - Create multiple domains (subtrees) corresponding to first-level nodes.

2. **Further Division:** 
   - Large domains can be divided into smaller subdomains.
   - Each server is authoritative for a specific domain or subdomain, regardless of size.
### Hierarchy of Name Servers

Distribution method results in a hierarchy of servers mirroring the hierarchy of domain names. By distributing the information in this way, the system ensures efficiency and reliability, as multiple servers share the load and a single point of failure is avoided.
#### Zone
In the domain name system, the entire domain name hierarchy is divided among multiple servers. The part of the domain name hierarchy that a server is responsible for is called a zone. 

1. **Zone Definition:**
   - A zone is a *contiguous* part of the entire domain name tree.
   - If a server manages a domain without subdividing it, the terms "domain" and "zone" are synonymous.

2. **Zone File:**
   - The server creates a database called a zone file.
   - The **zone file**: contains all the information for every node within that domain.

3. **Subdivided Domains:**
   - If a server subdivides its domain into smaller subdomains and delegates parts of its authority to other servers, "domain" and "zone" no longer refer to the same thing.
   - Lower-level servers store information about nodes in the subdomains, while the original server retains references to these servers.
   - *The original server remains responsible for the zone, but detailed information is maintained by the delegated servers.*
#### Root Server

A root server is a special type of server whose zone *encompasses the entire domain name tree*. 

1. **Role of Root Servers:**
   - Root servers generally *do not store information about specific domains*.
   - They *delegate authority to other servers* and maintain references to these servers.
   
2. **Distribution:**
   - There are several root servers distributed globally, each covering the entire domain name space.
   
By distributing the domain name information in zones and delegating authority, the system ensures efficient and reliable management of the domain name space.

### Primary and Secondary Servers

DNS defines two types of servers to manage zones: primary and secondary servers.
#### Primary Server
- **Responsibilities:**
  - Stores the authoritative zone file.
  - *Responsible for creating, maintaining, and updating the zone file*.
  - Stores the zone file on a local disk.
#### Secondary Server
- **Responsibilities:**
  - Transfers complete information about a zone from another server (primary or secondary).
  - Stores the transferred zone file on its local disk.
  - *Does not create or update the zone file itself*.
  - Receives updates from the primary server when changes are made.
#### Characteristics
- **Authority:**
  - Both primary and secondary servers are authoritative for the zones they serve.
  - *Secondary servers* are not at a *lower level of authority* but *provide redundancy*.

- **Redundancy:**
  - Ensures continuity of service if one server fails, as the other can continue to serve clients.

- **Mixed Roles:**
  - A server can act as a primary server for one zone and a secondary server for another.
  - When referring to a server as primary or secondary, it's important to specify the zone in question.

#### Data Loading

- **Primary Server:** Loads all information from the *local disk file*.
- **Secondary Server:** Loads all information from the *primary server*.

### DNS in the Internet

DNS is a versatile protocol used across different platforms. On the Internet, the domain name space was initially divided into three sections: generic domains, country domains, and the inverse domain. Due to difficulties in managing inverse domains, which were used to find host names from IP addresses, they have been deprecated (see RFC 3425). The focus now is on the remaining two sections.

#### Generic Domains

- **Purpose:**
  - *Defines registered hosts according to their generic behavior*.
  - Each node in this section of the DNS tree represents a *domain, serving as an index* to the domain name space database.
- **Examples:**
  - `.com` for commercial entities.
  - `.org` for organizations.
  - `.edu` for educational institutions.
![[Pasted image 20240808001947.png]]
![[Pasted image 20240808001908.png]]
#### Country Domains

- **Purpose:**
  - Uses *two-character* country abbreviations to organize domains by *country* (e.g., `us` for the United States).
  - *Second-level labels* can represent *organizational* or more specific *national designations*.
- **Structure:**
  - Example: 
	  - The United States uses state abbreviations as subdivisions (e.g., `ca.us` for California).
	  - A full address like `uci.ca.us.` translates to the University of California, Irvine, located in California, United States.
![[Pasted image 20240808002016.png]]

By using these two sections, DNS organizes the domain name space in a structured and easily navigable manner, accommodating the vast and growing number of hosts on the Internet.

### Resolution

**Name-address resolution** refers to the process of mapping a name to an address. DNS operates as a client/server application where a *DNS client*, called a **resolver**, *initiates the resolution process by contacting a DNS server*. The server then attempts to resolve the query or refers the resolver to other servers. The resolver interprets the response and delivers the result to the requesting process. Resolution can be recursive or iterative.
#### Recursive Resolution

In recursive resolution, *a single server handles the complete resolution process on behalf of the client*. Here’s an example:

1. An application on `some.anet.com` wants the IP address of `engineering.mcgraw-hill.com`.
2. The resolver sends the query to the local DNS server, `dns.anet.com`.
3. If `dns.anet.com` doesn’t have the answer, it forwards the query to a root DNS server.
4. The root DNS server forwards the query to a top-level domain (TLD) server responsible for `.com`.
5. The TLD server forwards the query to `dns.mcgraw-hill.com`.
6. `dns.mcgraw-hill.com` provides the IP address of `engineering.mcgraw-hill.com`.
7. The response is sent back through the chain: TLD server → root server → `dns.anet.com` → original client.
![[Pasted image 20240808005107.png]]
#### Iterative Resolution

In iterative resolution, *DNS server responds directly to the resolver with the address of the next server to query*:

1. The resolver on `some.anet.com` queries `dns.anet.com`.
2. `dns.anet.com` responds with the address of a root DNS server.
3. The resolver queries the root DNS server.
4. The root DNS server responds with the address of the TLD server.
5. The resolver queries the TLD server.
6. The TLD server responds with the address of `dns.mcgraw-hill.com`.
7. The resolver queries `dns.mcgraw-hill.com`.
8. `dns.mcgraw-hill.com` responds with the IP address of `engineering.mcgraw-hill.com`.
9. The resolver receives the final IP address.
![[Pasted image 20240808005124.png]]
#### Caching

DNS servers use caching to improve efficiency:

1. When a *DNS server receives a query response*, it stores the information in its cache memory.
2. Future queries for the same mapping can be resolved from the cache, speeding up the process.
3. Cached responses are marked as *unauthoritative* to indicate they’re from the cache, not the original authoritative source.

**TTL (Time to Live):**
- Authoritative servers attach a TTL value to each response.
- TTL defines the duration for which the information can be cached.
- Once the TTL expires, the server must query the authoritative source again.

**Cache Management:**
- Servers periodically check the cache and purge entries with expired TTLs to ensure up-to-date information.

By using recursive and iterative resolution methods, along with caching mechanisms, DNS efficiently *resolves name-to-address mappings* while maintaining accuracy and reliability.

### Resource Records
- The *zone information associated with a server* is implemented as a set of resource re-
cords. 
- Each resource record is a 5-tuple structure consisting of:
	`(Domain Name, Type, Class, TTL, Value)`
	1. **Domain Name**: Identifies the resource record.
	2. **Type**: Defines how the value should be interpreted.
	3. **Class**: Specifies the type of network (typically IN for Internet).
	4. **TTL (Time to Live)**: Indicates the validity period of the information in seconds.
	5. **Value**: Contains the information about the domain name.

![[Pasted image 20240808184445.png]]
### DNS Messages
DNS uses two types of messages to retrieve information about hosts: **query** and **response**. Both types share the same format, which includes the following:
#### Header
1. **Identification Field**: Used by the client to match the response with the query.
2. **Flag Field**: Indicates whether the message is a query or response and includes error status.
3. **Rest Fields**: Four fields that define the number of each record type in the message.
#### Question Section
Contains one or more question records and is included in *both query and response messages*.
#### Answer Section
Contains one or more resource records and is *only present in response messages*.
#### Authoritative Section 
Provides information about one or more authoritative servers for the query.
#### Additional Information Section
Provides additional information that may help the resolver.
![[Pasted image 20240808191251.png]]

### Encapsulation
DNS (Domain Name System) can use either UDP or TCP for communication, both on port 53. UDP is preferred when the response message is under 512 bytes due to the typical packet size limit. If the response is larger, TCP is used instead. 

Two scenarios occur with TCP:
1. If the *resolver knows in advance* that the response will exceed 512 bytes, it *uses TCP*, such as during zone transfers between DNS servers.
2. If the *resolver doesn't know the response size*, it *initially uses UDP*. If the response is truncated (over 512 bytes), the server sets the TC (truncated) bit. The resolver then switches to TCP and resends the request to get the complete response.

### Registrars
New domains are added to the DNS through a registrar, which is a commercial entity accredited by ICANN (the *Internet Corporation for Assigned Names and Numbers*). The process involves:

1. **Verification**: The registrar checks that the requested domain name is unique and not already in use.
2. **Entry**: The registrar adds the domain name to the DNS database.
3. **Fee**: A registration fee is charged for this service.

To register a new domain, an organization must provide:
- The domain name (e.g., `ws.wonderful.com`)
- The IP address of the server (e.g., `200.200.200.5`)

For a new domain like `ws.wonderful.com`, the organization would need to supply the registrar with these details to complete the registration process. 

### DDNS (Dynamic domain name system)
Dynamic Domain Name System (DDNS) addresses the need for automatic updates in DNS records due to frequent changes, such as adding or removing hosts or changing IP addresses. Originally, DNS required manual updates to the master file, which was impractical given the scale of today's Internet.

Here’s how DDNS works:
1. **Dynamic Updates**: When there is a change in a domain's binding (e.g., a new host or updated IP address), this information is sent to a primary DNS server, typically via DHCP (Dynamic Host Configuration Protocol).

2. **Primary Server Updates**: The primary DNS server updates the zone file with the new information.

3. **Secondary Servers**: These are notified of the change either actively or passively:
   - **Active Notification**: The primary server sends a message to secondary servers about the change.
   - **Passive Notification**: Secondary servers periodically check for updates.

4. **Zone Transfer**: Once notified, secondary servers request a full zone transfer to update their records with the latest information.

5. **Security**: DDNS can include authentication mechanisms to prevent unauthorized changes to DNS records.

DDNS helps manage the scalability and dynamic nature of modern networks by automating and securing DNS updates.

#### Security of DNS
DNS is crucial for Internet services but is vulnerable to several types of attacks:

1. **Eavesdropping**: Attackers might read DNS responses to profile users based on the sites they visit. To mitigate this, DNS messages need to be kept confidential.

2. **Spoofing**: Attackers could intercept and alter DNS responses to redirect users to malicious sites. This can be prevented with message origin authentication and integrity checks.

3. **Denial-of-Service (DoS)**: Attackers might flood a DNS server to overwhelm or crash it. Protection against DoS attacks is addressed through various measures, including the caching system which helps shield upper-level servers.

To enhance DNS security, DNSSEC (DNS Security Extensions) provides:
- **Message Origin Authentication**: Ensures that DNS responses come from a legitimate source.
- **Message Integrity**: Guarantees that DNS responses have not been altered in transit.

However, DNSSEC does not offer confidentiality for DNS messages or specific protection against DoS attacks.