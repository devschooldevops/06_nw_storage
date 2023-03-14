# Linux File Systems

## Data storage types
- **Block Level**
  - offered directly to the Operating System as raw devices
  - controlled by the OS, which can create partitions and file systems on them
  - cannot be shared across servers
  - types of block-level storage solutions: **DAS** (Direct Attached Storage), **SAN** (Storage Area Network) systems
  - **DAS** (Direct Attached Storage):
    - a DAS is anything that is directly attached to a server or computer
    - a DAS device could be a single internal hard disk or or multiple external hard disks (JBOD)
    - there are no networking layers between computer and storage
    - protocols: **SATA**, **SCSI**, **SAS**
    - for redundancy and/or performance disks can be clustered in **RAID**
  - **SAN** (Storage Area Network):
    - an enterprise system that provides access to consolidated block-level data storage
    - devices exposed by SANs appear to the operating system as locally-attached devices
    - comprised by dedicated hardware managed by a specialized software
    - protocols: **FCP** (SCSI over Fiber Channel), **iSCSI** (SCSI over TCP/IP), **FCoE**
- **File Level**
  - data is stored as files and presented to OSes as a hierarchical directories structure
  - file access management features, such as ownership and permissions
  - can be shared across servers
  - protocols: **NFS**, SMB/CIFS
  - devices that offer file level access are called **NAS** (Network Attached Systems)
- **Object Level**
  - an approach to address and manipulate data storage as discrete units, called objects
  - keeps the blocks of data that make up a file together and adds all of its associated metadata to that file
  - also adds extended metadata to the file and eliminates the hierarchical structure used in file storage, placing everything into a flat address space, called a storage pool
  - it is generally slower than a file or block storage system, but it is highly scalable
  - Amazon Simple Storage Service (Amazon S3), OpenStack Swift, Ceph


## Disks, partitions
- **disks** are just a raw physical/virtual mean to store data, they lack organization
- that organization comes in the name of **partition**
- a **partition** is a logical form of boundary, it is used to divide the disk in logical units
- partitions store data, but where are partitions stored?
- partitions are stored in what’s called a **partition table**
- partition tables store the data associated with partitions, where a partition starts, where a partition ends, etc
- however, partitions are not enough to store data in an ordered manner
- to do that we need a **file system**
- a file system takes care of storing pieces of data - **files**
- files themselves are just a bunch of data that are stored through the file system, which resides in a partition, which is recorded in a partition table, all of this, inside a disk

## Logical Volume Manager
- **LVM** is a storage abstraction layer that allows for a very flexible management of block-level devices
- provides features like the ability to add disk space to a logical volume and its filesystem while that filesystem is mounted and active
- allows for the collection of multiple physical hard drives and partitions into a single volume group which can then be divided into logical volumes.
- terminology:
  - **physical volumes**: physical disks, or disk partitions
  - **volume groups**: seen as a *"virtual partition"* which comprises an arbitrary number of physical volumes
  - **logical volumes**: contained in the volume groups they can be bigger than any single physical volume you might have. These will be formatted with a file system


## File systems
- a **file system** is a structured representation of data and a set of metadata describing this data
- it is applied to the storage during the **format** operation
- common file system types: ext3, **ext4**, **xfs**, fat, ntfs; nfs, smbfs/cifs


## Exercises
- add two virtual disks to the VM
- create standard partition, format it, mount it, created a persistent mount
```bash
# step 1: create partition on new disk

# with sudo or as root
parted

# check what we have (hopefully extra disks apart from sda)
(parted) print devices

(parted) select /dev/sXY

# create partition table for disk
(parted) mklable gpt

(parted) print # no partitions now

(parted) mkpart primary 0% 15%   # raw partition

(parted) quit
```
```bash
# step 2: format
mkfs -t ext4 /dev/sXY
```
```bash
# step 3: mounting
mkdir -p /mnt/sXY

mount -t auto /dev/sXY /mnt/sXY

df -hT  # the new device should show up as mounted and formatted
# at this point it is ready to use, we can go to /mnt/sXY and use it
```
```bash
# step 4: automount
# find out uuid
lsblk -o UUID,NAME

# add new entry in /etc/fstab, use above uuid
vim /etc/fstab

# restart and test
```
- create a LVM backed file system
```bash
# step 1: create partition on new disk

# with sudo or as root
fdisk /dev/sXY

# create empty partition table
o

# create full disk partition
n  # with defaults

# make LVM partition
t # then input hex code 8e, you can also check all the codes with L

# persist changes
w
```
```bash
# step 2: create PVs, VGs and LVs
# add physical volume (PV) to LVM
pvcreate /dev/sXY

# check
pvscan
pvs
pvdisplay /dev/sXY

# create volume group (VG)
vgcreate ops /dev/sXY

# extend VG
vgextend ops /dev/sXZ

# check
vgscan
vgs
vgdisplay ops

# create logical volumes (LV)
lvcreate --size 400M --name first ops
lvcreate --size 400M --name second ops
lvcreate --size 400M --name third ops

# check
lvscan
lvs
lvdisplay ops/first
lvdisplay ops/second
lvdisplay ops/third
```
```bash
# step 3: format
mkfs -t ext4 /dev/ops/first
mkfs -t ext4 /dev/ops/second
mkfs -t ext4 /dev/ops/third
```
```bash
# step 4: mounting
mkdir -p /mnt/first
mkdir -p /mnt/second
mkdir -p /mnt/third

mount -t auto /dev/ops/first /mnt/first
mount -t auto /dev/ops/second /mnt/second
mount -t auto /dev/ops/third /mnt/third

df -hT  # the new device should show up as mounted and formatted
# at this point it is ready to use, we can go to /mnt/* and use it
```
```bash
# step 5: automount
# find out uuid
lsblk -o UUID,NAME

# add new entry in /etc/fstab, use above uuid
vim /etc/fstab

# restart and test
```