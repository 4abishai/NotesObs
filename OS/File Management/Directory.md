### Storing files on Storage Devices
Storage devices like hard disks, optical disks, and solid-state disks.
1. **Storage Devices**: Files are stored on **random-access storage devices**, including hard disks, optical disks, and solid-state disks. These devices can be used for storing file systems either in their entirety or by subdividing them.
2. **Partitioning**: Partitioning involves *dividing a storage device into smaller parts, each of which can hold a separate file system*. Partitioning is useful for limiting the sizes of individual file systems, accommodating *multiple file-system* types on the same device, or leaving part of the device available for other uses.
3. **RAID Sets**: Storage devices can be collected together into RAID (Redundant Array of Independent Disks) sets, which *provide protection from the failure of a single disk*. Disks can be subdivided and collected into RAID sets to improve performance and reliability.
4. **Volumes**: Any *entity containing a file system* is known as a volume. A volume may be a subset of a device, a whole device, or multiple devices linked together into a RAID set. Each volume can store multiple operating systems, allowing a system to boot and run more than one operating system.
5. **Directory**: Each *volume that contains a file system must have a directory* that contains information about the files in that volume. The directory records information such as the name, location, size, and type of each file on that volume. [[File Concept#Role of directories in managing files]]

### Directory Structure
When considering a particular directory structure, we need to keep in mind
the operations that are to be performed on a directory:
1. **Search for a file.**
2. **Create a file.**
3. **Delete a file.**
4. **List a directory.**
5. **Rename a file.** Renaming a file may also allow its position within the directory structure to be changed.
6. **Traverse the file system.** For reliability, it is a good idea to save the contents and structure of the entire file system at regular intervals. Often, we do this by copying all files to magnetic tape. This technique provides a backup copy in case of system failure. In addition, if a file is no longer in use, the file can be copied to tape and the disk space of that file released for reuse by another file.le

#### Single-Level Directory
 - All files are contained in the same directory, which is easy to support and understand.
 - Simple to implement.
 - File names must be unique.
 - ![[Pasted image 20240416191611.png]]

#### Two-Level Directory
- In the two-level directory structure, each user has his own user file directory (UFD). 
- The UFDs have similar structures, but each lists only the files of a single user. 
- When a user job starts or a user logs in, the system’s master file directory (MFD) is searched. *Each entry in MFD points to the UFD for that user*.
- Different users may have files with the same name, as long as all the file names within each UFD are unique. 
- The program creates a new UFD and adds an entry for it to the MFD. The execution of this program might be restricted to system administrators.
- This structure effectively isolates one user from another. **Isolation** is an advantage when the users are completely independent but is a disadvantage when the users want to cooperate on some task and to access one another’s files.
- *User name* and a *file name* define a **path name**. To name a file uniquely, a user must know the path name of the file desired.
- Copying all the system files into each user's UFD would waste a significant amount of space. To avoid this, a special user directory is defined to contain the system files, and the operating system searches this directory when necessary.
- The sequence of directories searched when a file is named is called the **search path**. The search path can be extended to contain an unlimited list of directories to search when a command name is given.
- **Grouping** problem: It does not allow creation of sub-groups.
- ![[Pasted image 20240416192759.png]]

#### Tree-Level Directory
- There are more than two levels of directory.
- The tree has a root directory, and every file in the system has a unique path name.
- A directory (or subdirectory) contains a set of files or subdirectories.
- One bit in each directory entry defines the entry as a file (0) or as a subdirectory (1).
- In a typical operating system, each process has a current directory where most of the files of interest to that process are located. When a process references a file, the operating system first searches the current directory for that file `open()`. If the file is not found in the current directory, the user can either specify a path to the file or change the current directory to the one containing the file. This change of directory is done using a system call `change_directory()` that takes the directory name as a parameter and sets it as the new current directory. Once the current directory is changed, all subsequent file operations (like opening files) will search in the new current directory unless a full path is specified.
- Path names can be of two types: absolute and relative. An **absolute path** name begins at the root and follows a path down to the specified file, giving the directory names on the path. A **relative path** name defines a path from the current directory. If the current directory is root/spell/mail, then the relative path name prt/first refers to the same file as does the absolute path name root/spell/mail/prt/first.
- Suppose the directory to be deleted is not empty but contains several files or subdirectories. One of two approaches can be taken. 
	- Some systems will not delete a directory unless it is empty. Thus, to delete a directory, the user must first delete all the files in that directory. If any subdirectories exist, this procedure must be applied recursively to them, so that they can be deleted also. This approach can result in a substantial amount of work. 
	- An alternative approach, such as that taken by the UNIX rm command, is to provide an option: when a request is made to delete a directory, all that directory’s files and subdirectories are also to be deleted. 
- **Shared subdirectory** cannot be created here.
- ![[Pasted image 20240416194955.png]]

#### Acyclic-Graph Directory
- A tree structure prohibits the sharing of files or directories. An acyclic graph—that is, a graph with no cycles—allows directories to share subdirectories and files.
-  The UFD of each team member will contain this directory of shared files as a subdirectory.
- Shared files and subdirectories can be implemented using **links** in UNIX systems. A link is essentially a pointer to another file or subdirectory. 
	1. **Hard Links:** A hard link is a directory entry that points directly to the *physical location of a file on disk*. Hard links essentially create multiple directory entries (links) that all point to the same physical file. This means that changes to the file through one hard link will be reflected in all other hard links to the same file. Hard links cannot cross filesystem boundaries and cannot link to directories.
	
	2. **Symbolic Links (Symlinks):** A symbolic link, also known as a symlink or soft link. Unlike hard links, symbolic links are indirect pointers; they contain the *path of the target file or directory* rather than pointing directly to its physical location. Symbolic links can cross filesystem boundaries and can link to directories.
	When a reference to a file is made, the operating system searches the directory. If the directory entry is marked as a link, the operating system uses the path name contained in the link to locate the real file or directory. 
- Another common approach to implementing shared files is simply to duplicate all information about them in both sharing directories. Problem with duplicate directory entries is maintaining consistency when a file is modified.
- It is complex structure. 
- A file may now have multiple absolute path names. Consequently, distinct file names may refer to the same file.
- ![[Pasted image 20240416203102.png]]
##### Deletion problem:
Deletion of shared file might leave dangling pointers to the now-nonexistent file.
Approaches to handle file deletion in systems with shared files:
1. **Remove File Entry Only:** In systems where sharing is implemented by *symbolic links*, only the link is removed when a file is deleted. The space for the file is deallocated, leaving the links dangling. To handle this, the system can either search for and remove these dangling links immediately or leave them until an attempt is made to use them. If an attempt is made to use a dangling link, the access is treated as if the file does not exist.
2. **Keep Reference Count:** Another approach is to preserve the file until all references to it are deleted. This is done by keeping a count of the number of references to a file. The file is deleted when its **reference count** reaches 0.
   - **UNIX Approach:** UNIX uses this reference-counting approach for *hard links*. Each file has an *inode* (file information block) that contains a reference count. When the count reaches 0, the file can be safely deleted.
