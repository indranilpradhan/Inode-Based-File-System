# Inode-Based-File-System

The aim of this project is to implement a Inode Based File
System with added GUI functionalities. In this file system,
we have implemented our own methods for creating a file
system, mounting and unmounting it, creating files,
reading and writing to it and all other basic file operations.
The added GUI feature helps us to navigate through the
file system at any point of time and view the present
contents in it. To summarize, the goal is to implement a
simple file system on top of a virtual disk and understand
the implementation details of file systems.

A blank file has been visualized as the file system.
The file acts as a virtual disk. The mounting of the file system
involves reading metadata information from the virtual disk(file).
The file is divided into various segments:
a) Superblock
b) Data bitmap
c) Inode bitmap
d) Inode array
e) Data Segment(Data Blocks)
a) Superblock - The superblock maintains the metadata, about
the file system.It is a record of the characteristics of a filesystem,
including its size, the block size, the size and location of the inode
tables, the disk block map location, and other metadata. It
basically contains metadata about metadata.In our file system,
the superblock is used to store information about the file
descriptor map and related information about the inode and data
block bitmap. It also contains information about the file descriptors
and corresponding filename mapping.
b) Data bitmap - This segment stores the status of the data
blocks. The data bitmap is used to get free or unallocated data
blocks before file write operation. It tells the status of the data
blocks, wether free or occupied.
c) Inode bitmap - This segment stores information about the
stuatus of inode blocks. The inode bitmap is used to find free
inodes for allocation during file creation. It is a bit map storing the
current status of the inodes being used in the file system, whether
idle or being used.
d) Inodes - An inode in a file system, represents metadata about
a file. All objects in a UNIX system are files; actual files,
directories, devices, and so on. An inode contains essentially
information about ownership (user, group), access mode (read,
write, execute permissions) and file type. Two types of inodes
have been used in our system:
1) Directory inodes
2) File inodes

Structure of inode :

filename - Stores the file or directory name(as per the type of the
inode(file inode/directory inode)).
filepath - The absolute path of the file or directory.
filesize - Stores the size of the file which is pointed by the inode.
block_ptr - The array stores the various data blocks pointed by
the file.
During file write, free blocks are found from data bitmap and are
allocated for file write operation. The location information of the
free blocks which are allocated during the write operation are
stored in the block_ptr array.
inodepointer - The array stores the various inodes pointed by a
particular inode.
i) In case of a Directory Inode, the inodepointer contains location
information of other inodes. The inodes pointed by the directory
inode may be other directories or may be file inodes. Thus the
directory structure of the file system is implemented using the
inodepointer array.
2) In case of a File Inode, the inodepointer contains location
information of other file inodes(required for indirect file read and
write operation).
indirect_data_ptr_present - Flag which tells whether the inode
points to other inodes.
is_directory - Flag which tells whether the inode is file or directory
inode.

File System Functionalities:

● int make_fs(char *diskname); : This function creates a
fresh and empty file system on the virtual disk with name
disk_name. The function returns 0 on success, and -1 when
the disk disk_name could not be created, opened, or
properly initialized.
● int mount_fs(char *disk_name); : This function mounts
a file system that is stored on the virtual disk with name
disk_name. This makes the file system ready for use. The
function returns 0 on success, and -1 when the disk
disk_name could not be opened or when the disk does not
contain a valid file system.
● int unmount_fs(char *disk_name); : This function is
used to unmount the file system from the virtual disk with
name disk_name. As part of this operation, all the
meta-information is written back so that the disk persistently
reflects all changes that were made to the file system.The
function returns 0 on success, and -1 when the disk
disk_name could not be closed or when data could not be
written to the disk. All the open file descriptors are closed
when umount_fs is executed.
● int fs_create(char *name); : This is used to create a file
with the name 'name' in the root directory of the system. The
file is initially empty.
● int fs_open(char *name); : The file specified by name is
opened for reading and writing, and the file descriptor
corresponding to this file is returned to the calling function. If
successful, fs_open returns a non-negative integer, which is
a file descriptor that can be used to subsequently access this
file.
● int fs_close(int filedes); : The file descriptor fildes is
closed. This means that the closed file descriptor can no
longer be used to access the corresponding file. Upon
successful completion, a value of 0 is returned. In case the
file descriptor fildes does not exist or is not open, the
function returns -1.
● int file_read(char *disk_name,int fd); : This function
attempts to read data from the file referenced by the
descriptor fd and present in the disk 'disk_name' into a buffer
pointed to by buf. This function attempts to read all bytes
until the end of the file. Upon successful completion, the
number of bytes that were actually read is returned. In case
of failure, the function returns -1.
● int file_write(char *disk_name, string s, int fd); :
This function attempts to write data 's' to the file referenced
by the descriptor fd that is present in the disk disk_name
from the buffer. The function assumes that the buffer buf
holds at least nbyte bytes.Upon successful completion, the
number of bytes that were actually written is returned. In
case of failure, the function returns -1.
● int fs_delete(char *name); : This function deletes the
file with name name from the root directory of your file
system and frees all data blocks and meta-information that
correspond to that file.
● int create_directory(string directory_name); : This
function is used to create a directory named
'directory_name'. We can later add or remove files to and
from this directory.
● int change_directory(string new_path); : This
function is used to navigate to a new directory located at
'new_path' from the current working directory.
● int copy_files(string filename, string filepath): This
function is used to copy a file named 'filename' to a new
destination path 'filepath'. The contents of the original file
remains same.
● int rename(string old_name,string new_name); :
This function is used to rename a file having name
'old_name' and give it a new name 'new_name'.
● int file_seek(int filedes,int position); : This function
sets the file pointer (the offset used for read and write
operations) associated with the file descriptor fildes to the
argument position. Upon successful completion, a value of 0
is returned. file_seek returns -1 on failure.
● int move(string source, string destination); : This
function is used to move a file present at path 'source' to a
new location having path 'destination'. The file contents
remain the same.
● show_gui(); : This function is used to transfer the control
to GUI mode at any point of time. It lets us view the entire
contents of the file system in GUI mode and navigate
through the directories and view files in them.
Procedure:
● Create file system by pressing 1
● Mount file system by pressing 2
● Follow the process using given choices
