Files store information. When it is used, this information must be accessed
and read into computer memory.

### 1. Sequential Access
- Information in the file is processed in order, one record after the other. This mode of access is by far the most common; for example, editors and compilers usually access files in this fashion.
- A read operation—read next()—reads the next portion of the file and automatically advances a file pointer, which tracks the I/O location.
- The write operation—write next()—appends to the end of the file and advances to the end of the newly written material (the new end of file). 
![[Pasted image 20240416170616.png]]
### 2. Direct Access
It allows for *rapid reading and writing of fixed-length logical records in no particular order*. Here's a detailed explanation:
1. **Fixed-Length Logical Records**:
   - Direct access involves files made up of *fixed-length logical records*.
   - Programs can read and write these records rapidly in any order, without needing to follow a specific sequence.
2. **Disk Model**:
   - Direct access is based on a disk model of a file, as *disks allow random access to any file block*.
   - Files are viewed as a numbered sequence of blocks or records, and operations can be performed on these blocks in any order.
3. **Advantages**:
   - Direct-access files are useful for immediate access to large amounts of information, such as databases.
   - When a query concerning a particular subject arrives, the system can quickly compute which block contains the answer and read that block directly to provide the desired information.
4. **File Operations**:
   - File operations in direct access must include the block number as a parameter (e.g., read(n) and write(n) for reading and writing block number n).
   - Alternatively, operations like read next() and write next() can be retained, with an additional operation position file(n) to position the file at block number n before reading or writing.
5. **Block Numbering**:
   - The block number provided to the operating system is typically a **relative block number** *relative to the beginning of the file*.
   - This allows the operating system to manage the file's placement on disk and helps prevent unauthorized access to other parts of the file system.
6. **Accessing Records**:
   - To satisfy a request for record N in a file with a fixed logical record length L, the system reads or writes L bytes starting at location L * N within the file.
7. **File Access in Operating Systems**:
   - Some systems allow only sequential or only direct access, and some require that a file be defined as either sequential or direct when it is created.
8. **Simulation of Access Methods**:
   - It is possible to simulate sequential access on a direct-access file by maintaining a variable that defines the current position (cp) in the file.
   - However, simulating direct access on a sequential-access file is inefficient and cumbersome.

### Other Access Methods
Other access methods can be built on top of a direct-access method. Index-based access methods in file systems often involve the use of indexes to improve access efficiency.
1. **Index-based Access**:
   - These methods use an **index**: containing *pointers to various blocks* in the file.
   - To find a record, the index is first searched, and then the pointer is used to directly access the file and locate the record.
2. **Example**:
   - For a retail-price file with UPC codes and prices, the file can be sorted by UPC code.
   - An index can be created with the first UPC in each block, allowing for quick binary searches to find the block containing the desired record.
3. **Index for Large Files**:
   - In cases where the index file becomes too large to be kept in memory, a **secondary index** can be created.
   - The *primary index file contains pointers to secondary index files*, which in turn point to the actual data items.
4. **IBM's ISAM**:
   - IBM's **indexed sequential-access method** **(ISAM)** uses a **master index** that points to **disk blocks** of a secondary index, which then point to the actual file blocks.
   - The file is kept sorted on a defined key, and to find a particular item, a *binary search is first made on the master index* to locate the secondary index block.
   - Once the secondary index block is located, *another binary search* is used to find the block containing the desired record.
   - Finally, this *block is searched sequentially to locate the specific record*.
6. **Efficiency**:
   - These index-based methods significantly reduce the number of I/O operations required to locate a record in a large file, improving access speed.

![[Pasted image 20240416174336.png]]

