## list (existing) partitions: `fdisk`

### output to terminal

```console
vincent@:~$ sudo fdisk /dev/nvme0n1 -l
Disk /dev/nvme0n1: 953,87 GiB, 1024209543168 bytes, 2000409264 sectors
Disk model: PCIe-8 SSD 1TB                          
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 9F534724-1415-4812-AC47-BA5EDC73F98D

Device             Start       End   Sectors  Size Type
/dev/nvme0n1p1      2048    499711    497664  243M EFI System
/dev/nvme0n1p2    499712 128499711 128000000   61G Linux filesystem
/dev/nvme0n1p3 128499712 256499711 128000000   61G Linux filesystem
```

### output to `.sfdisk` file

```console
vincent@:~$ sudo fdisk /dev/nvme0n1

Welcome to fdisk (util-linux 2.39.3).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

This disk is currently in use - repartitioning is probably a bad idea.
It's recommended to umount all file systems, and swapoff all swap
partitions on this disk.

Command (m for help): O

Enter script file name: nvme0n1.sfdisk

Script successfully saved.
```

#### Example output

```console
vincent@:~$ cat nvme0n1.sfdisk 
label: gpt
label-id: 9F534724-1415-4812-AC47-BA5EDC73F98D
device: /dev/nvme0n1
unit: sectors
first-lba: 34
last-lba: 2000409230
sector-size: 512

/dev/nvme0n1p1 : start=        2048, size=      497664, type=C12A7328-F81F-11D2-BA4B-00A0C93EC93B, uuid=F9987897-79D5-48AB-807A-BECCEC5F9F5C
/dev/nvme0n1p2 : start=      499712, size=   128000000, type=0FC63DAF-8483-4772-8E79-3D69D8477DE4, uuid=F4070703-C1C7-4B32-9173-194DF6CCBED1
/dev/nvme0n1p3 : start=   128499712, size=   128000000, type=0FC63DAF-8483-4772-8E79-3D69D8477DE4, uuid=B68CB181-7968-4269-B940-5499E98ED944
```

## list mounting points

```console
vincent@:~$ lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
nvme0n1     259:0    0 953,9G  0 disk 
├─nvme0n1p1 259:1    0   243M  0 part /boot/efi
├─nvme0n1p2 259:2    0    61G  0 part /
└─nvme0n1p3 259:3    0    61G  0 part /home
```
