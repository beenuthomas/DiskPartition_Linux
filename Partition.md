# Disk Partition in Linux
Disk partitioning is the process of dividing a physical disk into one or more logical areas, called partitions, which users can manage independently. This process is a part of disk formatting. When a partition is created, the disk stores information about the location and size of each partition in a partition table. This allows each partition to be recognized by the operating system as a separate logical disk, enabling users to read and write data on those disks. The main advantage of disk partitioning is that each partition can have its own file system and be managed separately.

## Why do we need it?

- To upgrade the Hard Disk (to incorporate a new Hard Disk into the system)
- Dual Booting (Multiple Operating Systems on the same system)
- Efficient disk management
- Ensure backup and security.
- Work with different File Systems using the same system.

# Disk Formatting
Disk formatting is the process of preparing data-storage devices—such as hard drives, floppy disks, and flash drives—for initial use. This process is often necessary when a new operating system is being installed or when there are space issues that require additional storage capacity for data. 

The disk formatting procedure includes three main steps: low-level formatting, partitioning, and high-level formatting.

#### 1. Low-level Formatting
Low-level formatting is a type of physical formatting that involves marking the cylinders and tracks on a blank hard disk. Following this process, tracks are divided into sectors with the addition of sector markers. Nowadays, low-level formatting is typically performed by hard disk manufacturers.
If you perform low-level formatting on a hard disk that contains data, all the information will be erased irretrievably. Some users choose this type of formatting to protect their privacy, but it can also lead to damage and shorten the lifespan of the hard disk.
For these reasons, low-level formatting is not recommended for general users.

#### 2. Partitioning
Partitioning, as the name implies, refers to the process of dividing a hard disk into one or more sections, known as partitions. This procedure can be carried out by users and can significantly impact the disk's performance.

#### 3. High-level Formatting
High-level formatting is the process of preparing a hard disk for use by writing specific information to it, including file system details, cluster size, partition labels, and more. This process is typically done when erasing the disk and reinstalling an operating system.

Firstly, high-level formatting clears any existing data on the hard disk, generates boot information, and initializes the File Allocation Table (FAT). Additionally, it identifies and marks any logical bad sectors if a partition already exists.

This type of formatting is initiated by the user and does not generally harm the hard disk. It can be easily performed using tools like the Windows Disk Management snap-in or the diskpart command in the Administrator console.

High-level formatting can also help resolve various issues such as file system errors, corrupted hard drives, and developing bad sectors.

# How to create, format, and mount a Linux partition?

### Creating a new 250M partition in the disk /dev/sda

```
root@beenu:~# fdisk /dev/sda                                                                  ## Enter to the targeted harddisk
Command (m for help): n                                                                       ## Type n for create new partition
Partition number (4-128, default 4):                                                          ## Select partition number, the default is the next number of the last created partition
First sector (33558528-41943006, default 33558528):                                           ## default is recommended
Last sector, +/-sectors or +/-size{K,M,G,T,P} (33558528-41943006, default 41940991): +250M    ## Enter the targeted partition size in K,M,G,T,P

Created a new partition 4 of type 'Linux filesystem' and of size 250 MiB.
Command (m for help): w                                                                       ## Write the changes
The partition table has been altered.
Syncing disks.

root@beenu:~# 
```
### Format the newly created partition /dev/sda4
Formatting a Linux partition prepares the storage space for use by creating a file system. This allows the operating system to organize and store data in a structured way. Formatting usually takes place during the initial setup or when reconfiguring a partition for reuse. Common Linux file systems include ext, ext2, ext3, ext4, and xfs.
```
root@beenu:~# mkfs.ext3 /dev/sda4    ## Here it formating with the ext3 filesystem

```
### Mount Partition
Mounting and unmounting are essential tasks in any Linux operating system. They allow users to access and manage file systems on various storage devices. 

**Mounting:** Mounting is the process of attaching a storage device or partition to a directory, known as a mount point. This enables users to access and manage the contents of that storage device within the computer system.

**Unmounting:** Unmounting is the reverse of the mounting process. It involves detaching the storage device or partition from the computer system, making its content inaccessible until it is mounted again.

#### Creating a Mount Point
A mount point is a directory in the Linux system where the partition will be attached. Create a mount point using the mkdir command in Linux.

**Syntax 1:** mount `<partition>` `<mount point> `       ## Temporary Mount
```
root@beenu:~#  mkdir /mnt/mypartition                                          ## Create a directory(Mount point)
root@beenu:~#  mount /dev/sda4 /mnt/mypartition                                ## Mount partition to the targeted directory. umount command can be used to unmount it
root@beenu:~#  df -h                                                           ## To list all mounted filesystems
```
**Syntax 2:** `<device>` `<mount point> ` `<filesystem>` `<options>` `<dump>` `<fsck> `          ## Permanent Mount, which will persist after reboot
```
root@beenu:~# sudo vim  /etc/fstab
/dev/sda4 /mnt/mypartition ext3 defaults  0 0                                                        ## Write and quit(:wq) from vim editor
root@beenu:~# sudo mount -a                                                                          ## Mount all devices in the fstab
root@beenu:~# df -h
```

### Creating a file in Mount point directory

```
root@beenu:~# cd /mnt/mypartition                          ## To change directory to /mnt/mypartition
root@beenu:/mnt/mypartition# vim address                   ## Create a file called address
root@beenu:/mnt/mypartition# cat address                   ## To see the content in the address file
ABC Company
Kakkanad
Kochi
root@beenu:/mnt/mypartition# cd
root@beenu:~# ls /mnt/mypartition                           ## To list the contents
address  lost+found
root@beenu:~# df -h

```
**To reboot the system the command is `reboot`.**
