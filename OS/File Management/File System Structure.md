Disks provide the bulk of secondary storage for file systems. They are convenient for this purpose because they can be rewritten in place and can directly access any block of information they contain. This allows for easy sequential or random access to files, with *minimal movement of the read-write heads*.

*I/O transfers between memory and disk are performed in units of blocks*, with each *block containing one or more sectors*. Sector sizes typically range from 32 bytes to 4,096 bytes, with 512 bytes being the usual size.

File systems are designed to provide efficient and *convenient access to disk storage*. Designing a file system involves defining how the file system looks to the user (*file attributes, operations allowed, directory structure*) and creating *algorithms and data structures* to map the logical file system onto physical secondary-storage devices.

File systems are typically composed of several layers. One common design is a **layered structure** where each level uses the *features of lower levels to create new features for higher levels*.

1. **I/O Control Level:** This level consists of **device drivers** and **interrupt handlers** that transfer information between *main memory and the disk system*. Device drivers translate high-level commands into low-level, hardware-specific instructions used by the hardware controller.
2. **Basic File System:** This level issues *generic commands to the device driver to read and write physical blocks on the disk*. It manages *memory buffers* and *caches* that hold file-system, directory, and data blocks.
3. **File Organization Module:** It *translates logical block addresses to physical block addresses* for the basic file system to transfer. It also includes the **free-space manager**, which tracks *unallocated blocks*.
4. **Logical File System:** This level manages *metadata information* (except file contents). It *provides file-organization modules with the information they need*. It manages directory structure via *file-control blocks* (FCBs) or *inodes* and is responsible for file protection.

Using a layered structure *minimizes code duplication* and allows for multiple file systems to use the same I/O control and basic file-system code. However, layering can introduce overhead, potentially decreasing performance.

Many file systems exist today, with most operating systems supporting more than one. Examples include ISO 9660 for CD-ROMs, UFS for UNIX, FAT, FAT32, and NTFS for Windows, and ext3 and ext4 for Linux. Additionally, there are distributed file systems where a file system on a server is mounted by client computers across a network.

File-system research remains active, with projects like Google File System (GFS) designed for high-performance access from many clients across a large number of disks, and FUSE (Filesystem in Userspace) allowing user-level development and execution of file systems.

![[Pasted image 20240416225308.png]]