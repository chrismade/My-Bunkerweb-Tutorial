### CoreOS - add persistant volume

This step is not necessary but strongly recommended for disaster reovery - as described above every CoreOS install completely overwrites the
entire boot disk - hence it is recommended to have a second disk to hold the data - which is separated from the boot disk.
This 'data' disk can also be a high-quality USB Disk - only use one which allows a high number of write operations e.g. SanDisk Ultra (Extreme).

On virtual machine just configure a second virtual disk, size should be 30GB

Either way (second disk is a real harddisk, high-quality USB Stick or a virtual disk) assuming this data disk appears as device /dev/sdb
- in case of doubt check disk in output of `sudo dmesg`

**Partitioning**

enter `sudo fdisk /dev/sdb` then add a primary partition - on a blank device this goes by 'n'(ew) > 1 (first partition) > 'p'(rimary) and accept suggested 
start cylinder and size - you may want to review your setting with 'p'(rint) - when it is OK press 'w'(rite) to commit the change and write 
this data to the disk

**create filesystem**

enter `sudo mkfs /dev/sdb1` to create a standard filesystem on this data disk - the 1 in the device name refers to the partition number 1 you just created

**create mount directory**

enter `sudo mkdir /var/disk1` to create an empty directory to which the data disk will be mounted. It is important to locate this in `/var/`
because most parts of the boot disk is read-only and hence does not allow creation of directories.

**automount data disk at boot**

create a file `/etc/fstab` (find sample in folder 'etc' here in this repo) with the following content:

`/dev/sdb1 /var/disk1 ext4 defaults 0 0`

e.g. with

`sudo echo '/dev/sdb1 /var/disk1 ext4 defaults 0 0' >> /etc/fstab`

or your favorite text editor (e.g. vi, vim, nano)

**test mount**

now mount the new connection to the data disk with

`sudo mount /var/disk1`

check if the second disk is added correctly - e.g. by

`df -k | grep sd`

should show that `/dev/sda` (boot disk) as well as `/dev/sdb` (data disk) is correctly added to the system.

create a new directory on this data disk

`sudo mkdir /var/disk1/containers`

which will host all persistent data of our containers - and ...

**change ownership**

and change ownership to 'core' which is the default unprivileged user in CoreOS - which will also run all containerized workloads in order to
avoid use of 'root'

`sudo chown core /var/disk1/containers`

**SELINUX attribute**

CoreOS has SELINUX enabled by default - we need to run a special command to tell SELINUX that volumes for containers will be created under this path

`chcon -t container_file_t -R /var/disk1/containers/`

this will (hopefully) be inherited to any newly created file and folders - if not, re-apply this command if you see issues in container logfiles,
especially write permissions denied. Background is that 'root' containers will be started in the UID of the regular user `core` but non-root containers
will be mapped to another UID e.g. 568528 which finds then shared folders without sufficient permissions (mainly 'write' is missing)

