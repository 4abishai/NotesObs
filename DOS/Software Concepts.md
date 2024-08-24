### 1. Network Operating System
- Typically consist of **loosely-coupled software** on **loosely-coupled hardware**.
- Each user has a workstation with its own operating system.
- Commands usually run locally on the workstation.
- Remote login is possible using commands like "rlogin machine".
- File copying between machines is done with commands like "rcp".
#### Evolution to Shared File Systems:
- The primitive nature of direct remote access led to more convenient solutions.
- Shared global file systems accessible from all workstations were introduced.
- *File servers accept requests from client machines to read and write files*.

#### File Server Structure:
- *File servers maintain hierarchical file systems with a root directory*.
- Workstations can import or mount these file systems.
- Clients can mount servers in different places in their local file systems.
- *Different clients can have different views of the file system*.

#### Network Operating System Characteristics:
- Manages individual workstations and file servers.
- Handles communication between machines.
- Machines may run different operating systems.
- Requires agreement on message formats for communication.
- Characterized by **high autonomy of individual machines** and **few system-wide requirements**.

### 2. True Distributed Systems
#### True Distributed Systems:
- Aim to create the illusion of a *single timesharing system across a network of computers*.
- Sometimes referred to as having a **single-system image** or acting like a **virtual uniprocessor**.
- Users should not need to be aware of multiple CPUs in the system.

#### Key Characteristics:
- *Single, global interprocess communication* mechanism for all processes.
- Uniform process management across all machines.
- *Consistent system calls* available on all machines.
- *File system that looks the same* everywhere.
- *Identical kernels* running on all CPUs for easier coordination.
- Each kernel can control its local resources (e.g., memory management, scheduling).
- Avoids centralization of authority where possible.

#### Challenges and Requirements:
- No current system fully achieves the ideal of a true distributed system.
- Requires careful design of system calls and interfaces for a distributed environment.
- File naming and visibility should be consistent across locations.

#### Contrast with Network Operating Systems:
- Network operating systems allow each machine to operate independently.
- True distributed systems aim for a unified, coherent system image across all machines.

### 3. 