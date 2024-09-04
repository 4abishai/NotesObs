### 1. Network Operating System
- Typically consist of **loosely-coupled software** on **loosely-coupled hardware**.
- A Network Operating System is designed to *manage and coordinate resources in a networked environment*. It enables computers to *communicate and share resources* like files, printers, and applications across a network.
	- Allows multiple users to print to the same printer.
	
![[Pasted image 20240828114121.png]]
### Characteristics:
- **Security:** It controls access to network resources by managing user accounts and permissions.
- **Communication Services:** Provides networking protocols (like TCP/IP) for communication between devices.
- Each user has a workstation with its own operating system.
- Commands usually run locally on the workstation.
- Remote login is possible using commands like "rlogin machine".
- File copying between machines is done with commands like "rcp".
- Machines may run different operating systems.
- Requires agreement on message formats for communication.
- Characterized by **high autonomy of individual machines** and **few system-wide requirements**.
#### Evolution to Shared File Systems:
- The primitive nature of direct remote access led to more convenient solutions.
- **Shared global file systems** accessible from all workstations were introduced.
- *File servers accept requests from client machines to read and write files*.

##### File Server:
- *File servers maintain hierarchical file systems with a root directory*.
- Workstations can import or mount these file systems.
- Clients can mount servers in different places in their local file systems.
- *Different clients can have different views of the file system*.
### 2. True Distributed Systems
#### True Distributed Systems:
- *A collection of independent computers that appear to users as a single coherent system.*
- **Loosely coupled software** on **tightly coupled hardware**.
- Sometimes referred to as having a **single-system image** or acting like a **virtual uniprocessor**.
- No single computer holds all the control, and each node contributes to the system's overall functionality.
#### Characteristics:
- *Single, global interprocess communication* mechanism for all processes.
- *Uniform process management*.
- *Consistent system calls*.
- *File system that looks the same* everywhere.
- *Identical kernels* running on all CPUs for easier coordination.
- Each kernel can control its local resources (e.g., memory management, scheduling).

#### Challenges and Requirements:
- No current system fully achieves the ideal of a true distributed system.
- Requires careful design of system calls and interfaces for a distributed environment.
- File naming and visibility should be consistent across locations.
### 3. Multiprocessor Time-Sharing System:
- A multiprocessor time-sharing system is a computing environment where multiple processors share the same memory and work together to execute multiple tasks simultaneously.
- **Tightly-coupled** software on **tightly-coupled hardware.**
![[Pasted image 20240828114155.png]]
#### Characteristics:
- *Centralized design.*
- List of all processes ready to run, kept in shared memory called **ready queue**.
- If a process blocks (e.g., process B), the CPU must find another process to run.
- Execution initially slow due to cache being full of previous process data.
- The file system includes a single, unified block cache shared across all CPUs.

![[Pasted image 20240828114330.png]]