File-System Mounting allows *file systems* to be attached to the *directory structure*, making them accessible to processes. 

1. **Mount Point:** Before a file system can be accessed, it must be mounted. A mount point is a *location within the file structure* where the file system is attached. It's typically an empty directory.

2. **Mount Procedure:** When mounting a file system, the operating system needs the *name of the device* and the *mount point*. Some operating systems also require specifying the file system type, while others can determine it from the device's structures.

3. **File System Verification:** Once the mount point and device are specified, the operating system verifies that the device contains a valid file system. It does this by reading the device directory and confirming its format.

4. **Mounting Process:** After verifying the file system, the operating system notes in its directory structure that a file system is mounted at the specified mount point. This allows the OS to traverse its directory structure, switching among file systems as needed.

5. **Accessing Mounted File Systems:** Once a file system is mounted, you can *access its contents by using the mount point* in file paths. For example, if a file system is mounted at /home, you can access a file in that file system using the path /home/user/file.

6. **Unmounting:** When a file system is no longer needed, it can be unmounted. This restores the file system to its original state, and the *mounted file system's contents are no longer accessible*.

7. **System-Specific Mounting Semantics:** Different operating systems may have specific rules regarding mounting. For example, some systems may disallow mounting over directories with existing files, while others may allow it but obscure the directory's existing files until unmounting.

8. **Example Systems:**
   - **UNIX:** UNIX allows file systems to be mounted anywhere in the directory tree. Mounting commands are explicit, and a system configuration file contains a list of devices and mount points for automatic mounting at boot time.
   - **Windows:** Windows uses drive letters to assign volumes, with volumes having a general graph directory structure associated with the drive letter. More recent versions of Windows allow file systems to be mounted anywhere in the directory tree, similar to UNIX. Windows automatically discovers devices and mounts file systems at boot time.
