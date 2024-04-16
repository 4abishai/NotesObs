
-  **Storage Media**: Computers use various storage media like magnetic disks, tapes, and optical disks to store information. These storage devices are nonvolatile, meaning they retain data even when the system is powered off.

- **Logical View**: The *operating system provides a uniform logical view of stored information*, abstracting from the physical properties of storage devices. This abstraction makes it easier for users and applications to interact with data.

### What is File?
-  A file is a named *collection of related information recorded on secondary storage*. 
- It is the *smallest allotment of logical secondary storage*, and data cannot be written to secondary storage unless they are within a file.

#### Types of Files
Files can represent programs (source and object forms) and data. Data files can be numeric, alphabetic, alphanumeric, or binary. Files can be free form, like text files, or rigidly formatted.

#### Structure of Files 
A file is a sequence of bits, bytes, lines, or records, and its meaning is defined by its creator and user. Each type of file has a defined structure based on its content type.

1. **Text file**: It is a sequence of characters organized into lines.
2. **Source file**: It is a sequence of functions with declarations and executable statements.
3. **Executable Files**: It contains *code sections* that a loader can *bring into memory and execute*. These files are essential for running programs on a computer system.

### File Attributes
A file’s attributes vary from one operating system to another but typically
consist of these:
1. **Name**. The symbolic file name is the only information kept in *human-readable* form.
2. **Identifier**. This *unique tag*, usually a *number*, *identifies the file within the file system*; it is the *non-human-readable name* for the file.
3. **Type**. This information is needed for systems that support different types of files.
4. **Location**. This information is a pointer to a device and to the location of the file on that device.
5. **Size**. The current size of the file (in bytes, words, or blocks) and possibly the *maximum allowed* size are included in this attribute.
6. **Protection**. *Access-control information* determines who can do reading, writing, executing, and so on.
7. **Time, date, and user identification**. This information may be kept for creation, last modification, and last use. These data can be useful for protection, security, and usage monitoring.

#### Role of directories in managing files
1. **Directory Structure**: The directory structure is used to keep track of information about all files stored on secondary storage. It provides a way to organize and locate files.
2. **Directory Entry**: Each file in a directory has a directory entry, which typically includes the *file's name* and a *unique identifier*. The *identifier helps locate other attributes* of the file.
- In systems with many files, the size of the directory itself can be several megabytes.
- Like files, directories must be *nonvolatile*. They are brought into memory as needed, typically piecemeal to conserve memory.

### File Operations

1. **Creating a file**. Two steps are necessary to create a file. First, space in the file system must be found for the file. Second, an entry for the new file must be made in the directory.
2. **Writing a file**. To write a file, we make a system call specifying both the name of the file and the information to be written to the file. Given the name of the file, the system searches the directory to find the file’s location. The system must keep a **write pointer** to the location in the file where the next write is to take place. The write pointer must be updated whenever a write occurs.
3. **Reading a file**. To read from a file, we use a system call that specifies the name of the file and where (in memory) the next block of the file should be put. Again, the directory is searched for the associated entry, and the system needs to keep a **read pointer** to the location in the file where the next read is to take place. Once the read has taken place, the read pointer is updated. Because a process is usually either reading from or writing to a file, the current operation location can be kept as a per-process **current- file-position pointer**. _Both the read and write operations use this same pointer, saving space and reducing system complexity_.
4. **Repositioning within a file**. The directory is searched for the appropriate entry, and the _current-file-position pointer is repositioned to a given value_. Repositioning within a file need not involve any actual I/O . This file operation is also known as a **file seek**.
5. **Deleting a file**. To delete a file, we search the directory for the named file. Having found the associated directory entry, _we release all file space,_ so that it can be reused by other files, and _erase the directory entry_.
6. **Truncating a file**. The user may want to erase the contents of a file but keep its attributes. Rather than forcing the user to delete the file and then recreate it, this function allows all _attributes to remain unchanged_—except for file length—but lets the file be reset to length zero and its _file space released_.

#### Management of open files

1. **Open() System Call**: 
   - Before a file can be accessed, it must be opened using the open() system call.
   - The open() system call typically takes the *file name* and *access mode* as arguments.
   - Access modes include *read-only, write-only, read-write, append*, etc.
2. **Open-file Table**:
   - The operating system maintains an open-file table, which is a data structure containing information about all open files.
   - Each entry in the open-file table corresponds to an open file and contains information such as file position, access mode, etc.
   - Entries in the open-file table are *indexed by an integer value*, often called a file descriptor or file handle.
3. **File Descriptor**:
   - When a file is opened, the open() system call returns a file descriptor, which is *used to identify the file in subsequent operations*.
   - File descriptors are used by other system calls (read(), write(), close()) to operate on the file without needing to search the directory.
4. **File Operations**:
   - Once a file is open, operations such as reading, writing, and seeking can be performed using the file descriptor.
   - For example, the read() and write() system calls take the file descriptor as an argument to specify the file to read from or write to.
5. **Close() System Call**:
   - When a file is no longer needed, it should be closed using the close() system call.
   - Closing a file *removes its entry from the open-file* table and *releases any resources* associated with the file.
6. **Implicit File Opening**:
   - Some systems automatically open a file when it is first referenced by a program.
   - The file remains open until the program terminates or explicitly closes the file.
7. **Concurrency**:
   - In multi-process environments, the operating system maintains a per-process table and a system-wide table for open files.
   - The **per-process table** tracks files opened by each process, while the **system-wide table** contains global file information.
   - When a file is opened by multiple processes, each process gets its own entry in the per-process table pointing to the same entry in the system-wide table.
8. **File Status**:
   - The system-wide table contains additional information about each file, such as the file's location on disk.
   - The per-process table contains additional information about each file, such as access permissions.
9. **Open Count**:
   - The open-file table typically maintains an open count for each file to track how many processes have the file open.
   - When a file is closed, the open count is decremented, and if it reaches zero, the file is no longer in use and its resources are freed.
10. **Disk location of the file**. The information needed to locate the file on disk is kept in memory so that the system does not have to read it from disk for each operation.

**Note**:
- File locks allow one process to lock a file and prevent other processes from gaining access to it.
	**Shared Lock**: A shared lock is akin to a reader lock in that several processes can acquire the lock concurrently.
	**Exclusive Lock**:  An exclusive lock behaves like a writer lock; only one process at a time can acquire such a lock.
- If the locking scheme is mandatory, the operating system ensures locking integrity. For advisory locking, it is up to software developers to ensure that locks are appropriately acquired and released.  Windows operating systems adopt mandatory locking, and UNIX systems employ advisory locks.
### File Type
1. **Recognizing File Types**:
   - Operating systems may recognize and support different file types to allow for specific operations on files.
   - Recognizing file types can prevent errors, such as trying to output the binary-object form of a program, which can produce garbage if the file type is not recognized.

2. **File Naming Conventions**:
   - A common technique for implementing file types is to include the type as part of the file name, using a name and an extension separated by a period.

3. **File Extensions**:
   - The extension in a file name indicates the file type and the type of operations that can be performed on the file.
   - For example, files with .com, .exe, or .sh extensions can be executed. .com and .exe files are binary executable files, while .sh files are shell scripts.

4. **Application-specific Extensions**:
   - Application programs use extensions to indicate the file types they can process.
   - For example, Java compilers expect source files to have a .java extension, and Microsoft Word expects its files to end with a .doc or .docx extension.

5. **Optional Extensions**:
   - File extensions are not always required. Users can specify a file without an extension, and the application will look for a file with the given name and the expected extension.
   - These extensions serve as hints to applications about the file type and are not enforced by the operating system.

#### How are file types managed in MAC OS X and UNIX
**Mac OS X**:
- Each file in Mac OS X has a type, such as .app for applications.
- Additionally, each file has a creator attribute containing the name of the program that created it.
- The creator attribute is set by the operating system during the create() call, and its use is enforced and supported by the system.
- For example, a file created by a word processor would have the word processor's name as its creator.
- When a user opens a file by double-clicking its icon, the operating system automatically invokes the associated program (e.g., a word processor) to open and load the file for editing.

**UNIX**:
- In UNIX, some files have a crude magic number stored at the beginning to indicate their type, such as executable programs, shell scripts, or PDF files.
- However, not all files have magic numbers, so system features cannot solely rely on this information.
- UNIX does not record the name of the creating program like Mac OS X does with the creator attribute.
- UNIX allows file-name-extension hints, but these extensions are not enforced or relied upon by the operating system; they are primarily used to aid users in identifying file contents.
- The decision to use or ignore file extensions is left to the application's programmer.

### File Structure
1. **Internal Structure of Files**
   - File types can indicate the internal structure of a file, matching the expectations of programs that read them.
2. **Operating System Requirements**:
   - Certain files must conform to a required structure understood by the operating system.
3. **Multiple File Structures**:
   - Supporting multiple file structures in an operating system can lead to increased size and complexity of the operating system.
4. **Disadvantages**:
   - Defining multiple file structures may lead to problems *when new applications require information structured in ways not supported by the operating system*.
   - For example, if a system supports only text and binary files, creating an encrypted file may require circumventing or misusing the file-type mechanism.
5. **Minimal Number of File Structures**:
   - Some operating systems, like UNIX and Windows, impose and support a minimal number of file structures.
   - UNIX, for example, considers each file to be a sequence of 8-bit bytes without interpreting their meaning, providing maximum flexibility but little support.
6. **Support for Executable Files**:
   - All operating systems must support at least one file structure for executable files so that programs can be loaded and run.

### Internal File Structure
This passage explains the internal structure of files in terms of logical and physical records, block sizes, and packing techniques:

1. **Block Size and Disk I/O**:
   - Disk systems have a well-defined **block size**, *typically determined by the size of a sector*.
   - All disk input/output (I/O) operations are performed in units of one block, and all blocks are the same size.

2. **Logical Records and Packing**:
   - Logical records within a file may vary in length, and their *length may not match the size of a physical block*.
   - Packing multiple logical records into a physical block is a common solution to this problem.
   - For example, UNIX treats all files as streams of bytes, with each byte individually addressable by its offset from the beginning or end of the file.

3. **Packing Techniques**:
   - The file system automatically packs and unpacks bytes into physical disk blocks, such as 512 bytes per block, as necessary.
   - The logical record size, physical block size, and packing technique determine how many logical records are in each physical block.

4. **Block-based I/O**:
   - All basic I/O functions in the operating system operate in terms of blocks, not individual bytes.
   - The conversion from logical records to physical blocks is a relatively simple software problem.

5. **Internal Fragmentation**:
   - Disk space is always allocated in blocks, which can lead to wasted space at the end of a file.
   - For example, if each block is 512 bytes and a file is 1,949 bytes, it would be allocated four blocks (2,048 bytes), with 99 bytes wasted.
   - This wasted space is called internal fragmentation, and all file systems suffer from it to some extent.
   - Larger block sizes lead to greater internal fragmentation because more space is wasted when files do not align perfectly with block boundaries.
