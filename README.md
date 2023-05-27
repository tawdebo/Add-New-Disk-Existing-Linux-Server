# Add-New-Disk-Existing-Linux-Server

How to Add a New Disk to an Existing Linux Server


As system administrators, we would have got requirements wherein we need to configure raw hard disks to the existing servers as part of upgrading server capacity or sometimes disk replacement in case of disk failure.

In this article, I will take you through the steps by which we can add the new raw hard disk to an existing Linux server such as RHEL/CentOS or Debian/Ubuntu.

Important: Please note that the purpose of this article is to show only how to create a new partition and doesn’t include partition extension or any other switches.

I have added a hard disk of 20GB capacity to be mounted as a /data partition.

fdisk is a command line utility to view and manage hard disks and partitions on Linux systems.

# 

```
fdisk -l
```

This will list the current partitions and configurations.

![fidisk](https://www.tecmint.com/wp-content/uploads/2017/03/Find-Linux-Partition-Details.png)


After attaching the hard disk of 20GB capacity, the fdisk -l will give the below output.

```
fdisk -l
```
![fidisk2](https://www.tecmint.com/wp-content/uploads/2017/03/Find-New-Partition-Details.png)


New disk added is shown as /dev/xvdc. If we are adding physical disk it will show as /dev/sda based of the disk type. Here I used a virtual disk.

To partition a particular hard disk, for example /dev/xvdc.

```
fdisk /dev/xvdc
```

![fidisk3](https://www.tecmint.com/wp-content/uploads/2017/03/Create-New-Partition-in-Linux.png)

Commonly used fdisk commands.

# n – Create partition
# p – print partition table
# d – delete a partition
# q – exit without saving the changes
# w – write the changes and exit.

Here since we are creating a partition use n option.

Create either primary/extended partitions. By default we can have upto 4 primary partitions.

![fdisk4](https://www.tecmint.com/wp-content/uploads/2017/03/Create-Primary-Partition.png)

Give the partition number as desired. Recommended to go for the default value 1.

![fdisk5](https://www.tecmint.com/wp-content/uploads/2017/03/Assign-a-Partition-Number.png)


Give the value of the first sector. If it is a new disk, always select default value. If you are creating a second partition on the same disk, we need to add 1 to the last sector of the previous partition.

![fdisk6](https://www.tecmint.com/wp-content/uploads/2017/03/Assign-Sector-to-Partition.png)

Give the value of the last sector or the partition size. Always recommended to give the size of the partition. Always prefix + to avoid value out of range error.

![fdisk7](https://www.tecmint.com/wp-content/uploads/2017/03/Assign-Partition-Size.png)

Save the changes with "W" and exit.

![fidisk8](https://www.tecmint.com/wp-content/uploads/2017/03/Save-Partition-Changes.png)


Now format the disk with mkfs command.

```
mkfs.ext4 /dev/xvdc1
```

![format](https://www.tecmint.com/wp-content/uploads/2017/03/Format-New-Partition.png)

Once formatting has been completed, now mount the partition as shown below.

```
mount /dev/xvdc1 /data
```

Make an entry in /etc/fstab file for permanent mount at boot time.

```
/dev/xvdc1	/data	ext4	defaults     0   0
```


