The file system on disk contains important information such as how to boot an operating system, the total number of blocks, the number and location of free blocks, the directory structure, and individual files. These structures play a crucial role in managing the file system and optimizing performance.

1. **Boot Control Block:** This block, located at the beginning of a volume, contains information needed to boot an operating system from that volume. It is typically the first block of a volume and may be empty if the disk does not contain an operating system. Examples include the boot block in UFS and the partition boot sector in NTFS.

2. **Volume Control Block:** This block, located per volume, contains details such as the number of blocks in the partition, block sizes, free-block count and pointers, and free-FCB count and pointers. Examples include the superblock in UFS and the information stored in the master file table in NTFS.

3. **Directory Structure:** This structure, per file system, organizes files. In UFS, this includes *file names* and associated *inode* numbers. In NTFS, it is stored in the *master file table*.

4. **Per-File FCB:** This block contains detailed information about each file, including a *unique identifier number for association with a directory entry.* In NTFS, this information is stored within the master file table, which uses a relational database structure.
![[Pasted image 20240416233516.png]]

In-memory information is used for file-system management and performance improvement through caching. Structures include:

- **In-memory Mount Table:** Contains information about each mounted volume.
- **In-memory Directory-Structure Cache:** Holds directory information of recently accessed directories.
- **System-Wide Open-File Table:** Contains a copy of the FCB of each open file, along with other information.
- **Per-Process Open-File Table:** Contains a pointer to the appropriate entry in the system-wide open-file table, along with other information.
- **Buffers:** Hold file-system blocks when they are being read from or written to disk.

To create a new file, an application program calls the logical file system, which knows the format of directory structures. The logical file system allocates a new FCB (or uses a preallocated one) and updates the directory structure with the new file name and FCB.

When opening a file, the open() system call searches the system-wide open-file table to see if the file is already in use. If not, it searches the directory structure and copies the FCB into the system-wide open-file table. An entry is made in the per-process open-file table, and file operations are performed through this entry. All file operations are then performed via this pointer. UNIX systems refer to it as a **file descriptor**; Windows refers to it as a **file handle**.

Closing a file removes the entry from the per-process table and decrements the open count in the system-wide table. When all users close the file, updated metadata is copied back to the disk-based directory structure, and the system-wide entry is removed.

Caching is important for performance, with most systems keeping file information in memory to avoid disk I/O. Systems like BSD UNIX use caches extensively, resulting in high cache hit rates and improved performance.

![[Pasted image 20240416233647.png]]

### Partitions and Mounting
A disk can be divided into multiple partitions, each of which can be "**raw**," containing no file system, or "**cooked**," containing a file system. Raw partitions are used where no file system is needed, such as for UNIX *swap space* or databases that format data to suit their needs. Raw disk can also hold information for disk *RAID* systems or RAID configuration information.

*Boot information can be stored in a separate partition, typically as a sequential series of blocks loaded into memory at boot time*. This boot loader knows enough about the file-system structure to find and load the kernel and start it executing. It can also support dual-booting, allowing multiple operating systems to be installed on a single system, with the boot loader determining which one to boot.

The **root partition**, *containing the operating-system kernel* and sometimes other system files, is *mounted at boot time*. Other volumes can be automatically mounted at boot or manually mounted later. During mounting, the *operating system verifies that the device contains a valid file system* by reading the device directory and checking its format. If the format is invalid, the partition may need its consistency checked and possibly corrected.

In Microsoft Windows, each volume is mounted in a separate namespace denoted by a letter and a colon (e.g., C: or D:). The operating system records the file system's mount point by placing a *pointer to the file system in a field of the device structure* corresponding to the drive letter. This allows the system to traverse the directory structures on that device to find specified files or directories.

In UNIX systems, file systems can be mounted at any directory. Mounting is implemented by *setting a flag in the in-memory copy of the inode for that directory*, indicating that it is a mount point. A field then points to an entry in the mount table, indicating which device is mounted there. The *mount table entry contains a pointer to the superblock of the file system on that device*, enabling the operating system to switch seamlessly among file systems of varying types.

### Virtual File Systems (VFS)

Modern operating systems must support multiple types of file systems, including network file systems like NFS, and allow users to seamlessly navigate between them. One way to achieve this is through *object-oriented techniques*, which simplify, organize, and modularize the implementation of file systems.

The file system implementation in such systems typically consists of three major layers:

1. **File-System Interface**: This layer is based on system calls like open(), read(), write(), and close(), as well as file descriptors.

2. **Virtual File System (VFS) Layer**: The VFS layer serves two key functions:
   - It *separates file-system-generic operations from their implementations*, defining a clean interface. This allows different file-system implementations to coexist on the same machine, enabling transparent access to different types of file systems mounted locally.
   - It provides a mechanism for *uniquely representing a file throughout a network*. The VFS achieves this using a file-representation structure called a "**vnode**". 

3. **File-System Type or Remote-File-System Protocol Layer**: This layer implements *file-system-specific operations* to handle *local requests* according to their file-system types and calls the *appropriate protocol procedures* for remote requests.

In Linux, the VFS architecture defines four main object types:
- **Inode object**: Represents an individual file.
- **File object**: Represents an open file.
- **Superblock object**: Represents an entire file system.
- **Dentry object**: Represents an individual directory entry.

For each object type, the VFS defines a set of operations that can be implemented. Each *object contains a pointer to a function table*, listing the addresses of functions that implement the defined operations for that object type.

For example, the file object has operations like open(), close(), read(), write(), and mmap(). An implementation of the file object for a specific file type must implement each function specified in the definition of the file object.

The VFS layer can perform operations on these objects by calling the appropriate function from the object's function table, without needing to know in advance the specific type of object it is dealing with. *This allows the VFS to handle different file system types and remote file systems seamlessly.* The VFS does not know, or care, whether an inode represents a disk file, a directory file, or a remote file. VFS software layer will call that function without caring how the data are actually read.

![[Pasted image 20240417001708.png]]
