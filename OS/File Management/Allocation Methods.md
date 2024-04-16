### Contiguous allocation
Contiguous allocation is a method of allocating disk space for files where each file occupies a set of *contiguous blocks* on the disk. This method offers advantages such as *minimal disk seeks* and *fast access times* for both *sequential and direct access*. However, it also comes with some challenges, including the *need to find contiguous space* for new files and the issue of *external fragmentation*.

1. **Definition**: Contiguous allocation requires that each file occupy a set of contiguous blocks on the disk. For a file that starts at location b and is n blocks long, it occupies blocks b, b + 1, b + 2, ..., b + n − 1.

2. **Accessing Contiguous Files**: Accessing a file that has been allocated contiguously is straightforward. For sequential access, the file system remembers the *disk address of the last block referenced and reads the next block when needed*. For direct access to block i of a file that starts at block b, the system can immediately access block b + i.

3. **Finding Space for New Files**: One challenge of contiguous allocation is finding space for a new file. The system's free space management determines how this task is accomplished. Strategies like *first fit* and *best fit* are commonly used to select a free hole (contiguous space) from the set of available holes.

4. **External Fragmentation**: Contiguous allocation suffers from external fragmentation, where free disk space is broken into small pieces. External fragmentation becomes a problem when the *largest contiguous chunk is insufficient for a request*, leading to storage being fragmented into a number of small holes.

5. **Defragmentation**: To address external fragmentation, some systems use defragmentation techniques. One approach is to *copy the entire file system onto another disk, freeing up the original disk completely*. This compacts all free space into one contiguous space, solving the fragmentation problem.

6. **Space Estimation**: Determining how much space is needed for a file can be challenging. If too little space is allocated, the file cannot be extended in place, leading to possible program termination or the need to find a larger hole and copy the file contents.

7. **Modified Contiguous Allocation**: Some systems use a modified contiguous-allocation scheme where a *contiguous chunk of space is allocated initially*. If more space is needed, another chunk called an **extent** is added. This approach helps reduce internal fragmentation and optimize performance.

![[Pasted image 20240417012457.png]]
**Note**: Compacting these disks may take hours and may be necessary on a weekly basis. Some systems require that this function be done **off-line**, with the *file system unmounted*. During this **down time**, normal system operation generally cannot be permitted, so such compaction is avoided at all costs on production machines. Most modern systems that need defragmentation can perform it **on-line** during normal system operations, but the performance penalty can be substantial.
### Linked Allocation
Linked allocation is a file allocation method where each file is *represented as a linked list of disk blocks*, which can be scattered anywhere on the disk. This method solves the problem of external fragmentation and allows files to grow *dynamically* without needing to preallocate space. However, it is mainly suitable for *sequential access* files and requires additional *disk space for storing pointers*.

1. **File Representation**: In linked allocation, each file is represented as a linked list of disk blocks. *The directory entry for each file contains pointers to the first and last blocks of the file.* These pointers are not visible to the user.

2. **Creating a New File**: To create a new file, a new entry is added to the directory with a pointer to the first block set to null. As data is written to the file, free blocks are allocated and linked to the end of the file.

3. **Accessing Files**: For sequential access, the system follows the pointers from block to block. However, *direct access* to a specific block in the middle of a file requires starting from the beginning and traversing the linked list, which can be *inefficient*.

4. **Pointer Overhead**: Linked allocation requires additional space for storing pointers. For example, if each pointer requires 4 bytes in a 512-byte block, then about 0.78 percent of the disk space is used for pointers instead of file data.

5. **Cluster Allocation**: To reduce pointer overhead, some systems use clusters, which are *multiples of blocks*. By allocating clusters instead of individual blocks, the percentage of disk space used for pointers decreases.

6. **Reliability**: Linked allocation can be *less reliable* because files are linked together by pointers, and if a *pointer is lost or damaged*, it can result in linking into the wrong location or file. Using doubly linked lists or storing additional information in each block can mitigate this issue but increases overhead.

7. **File Allocation Table (FAT)**: An alternative to traditional linked allocation is the use of a file-allocation table (FAT), which is a table indexed by block number. *Each entry in the table contains the block number of the next block in the file*. This method was used by MS-DOS and is efficient for disk space allocation but can result in a *significant number of disk head seeks if the FAT is not cached*.

![[Pasted image 20240417020057.png]]

### Indexed Allocation
Indexed allocation is a file allocation method that addresses the issue of direct access efficiency in linked allocation. In linked allocation, the pointers to blocks are scattered all over the disk, making direct access inefficient. Indexed allocation *centralizes these pointers into an index block*, allowing for more efficient direct access.

1. **File Representation**: In indexed allocation, *each file has its own index block*, which is an array of disk-block addresses. The i-th entry in the index block points to the i-th block of the file. *Directory entry for each file contains the address of its index block*.
![[Pasted image 20240417015018.png]]
2. **Creating a New File**: When a file is created, all pointers in its index block are set to null. As data is written to the file, free blocks are allocated and their *addresses are added to the index block*.

3. **Direct Access**: To access a specific block in the file, the system uses the pointer in the corresponding entry in the index block. This allows for *efficient direct access* to any block in the file.

4. **Pointer Overhead**: Indexed allocation suffers from pointer overhead, as the index block consumes disk space. For files with only a few blocks, the *overhead of the index block can be significant compared to linked allocation*.

5. **Index Block Size**: Determining the size of the *index block* is crucial. If it is too small, it may not be able to hold enough pointers for a large file. Different strategies are used to address this issue, such as linked schemes, multilevel indexes, and combined schemes.
   - **Linked Scheme**: *Index blocks are linked together*, with each block containing a set of disk-block addresses. This allows for scalability to handle large files.
   - **Multilevel Index**: Uses multiple levels of index blocks, where the *first-level index points to second-level index blocks, and so on*. This allows for very large files to be accessed efficiently.
   - **Combined Scheme**: Combines *direct pointers in the file's inode* with *indirect pointers in the index block*. This allows small files to be accessed without an index block, while large files can use the index block for indirect access.

6. **Performance**: Indexed allocation suffers from some performance issues similar to linked allocation. Index blocks can be cached in memory, but *data blocks may still be spread all over the disk*, leading to potential performance bottlenecks.

![[Pasted image 20240417020035.png]]

### Performance
The selection of an allocation method in a file system is crucial for determining both storage efficiency and data-block access times, which are critical performance criteria. Different methods are suitable for different access patterns, such as sequential or random access.

1. **Contiguous Allocation**: This method offers efficient access for both sequential and direct access. Only one disk access is required to retrieve a block, making it ideal for sequential access. Direct access is also efficient because the disk address of any block can be calculated immediately. However, finding contiguous space for large files can be challenging, leading to fragmentation issues.

2. **Linked Allocation**: Linked allocation is suitable for sequential access but inefficient for direct access, as accessing the i-th block may require i disk reads. Therefore, it's not recommended for applications requiring direct access. Some systems support both access patterns by using contiguous allocation for direct access files and linked allocation for sequential access files.

3. **Indexed Allocation**: Indexed allocation allows for efficient direct access by centralizing pointers in an index block. However, *accessing large files may require reading multiple index blocks, impacting performance*. Keeping the index block in memory can improve performance but requires significant memory space. *Combining contiguous allocation with indexed allocation is a common approach, where contiguous allocation is used for small files and indexed allocation for large files.*

4. **Hybrid Approaches**: Many systems use hybrid approaches to optimize performance. For example, they may automatically *switch from contiguous allocation to indexed allocation for large files*. This approach leverages the efficiency of contiguous allocation for small files while ensuring scalability for large files.

5. **Optimizations**: File systems employ various optimizations to improve performance, such as *caching index blocks in memory* and *dynamically adjusting allocation methods* based on *file size and access patterns*. Given the speed gap between CPUs and disks, optimizing disk-head movements is critical, and file systems may incorporate thousands of extra instructions to minimize seek times.
