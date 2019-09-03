Based on [this tutorial](https://www.tecmint.com/extend-and-reduce-lvms-in-linux/).

```
$ ▶ su -
Password: 
[root@localhost ~]# pvs
  PV         VG     Fmt  Attr PSize   PFree
  /dev/sda2  fedora lvm2 a--  <15.00g    0 
  /dev/sda4  fedora lvm2 a--   <4.00g    0 
[root@localhost ~]# vgs
  VG     #PV #LV #SN Attr   VSize  VFree
  fedora   2   2   0 wz--n- 18.99g    0 
[root@localhost ~]# lvs
  LV   VG     Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root fedora -wi-ao---- 17.39g                                                    
  swap fedora -wi-ao----  1.60g                                                    
[root@localhost ~]# fdisk -l /dev/sda
Disk /dev/sda: 40 GiB, 42949672960 bytes, 83886080 sectors
Disk model: VBOX HARDDISK   
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x08a03abb

Device     Boot    Start      End  Sectors Size Id Type
/dev/sda1  *        2048  2099199  2097152   1G 83 Linux
/dev/sda2        2099200 33554431 31455232  15G 8e Linux LVM
/dev/sda4       33554432 41943039  8388608   4G 8e Linux LVM
```

Extension of the root LVM partition of a Linux system installed on an Oracle VirtualBox machine. 

The virtual disk has been extended by 20 GB (40690 MB). The operation was performed on Windows command with the following instruction:
```
> VBoxManage modifyhd "C:\Users\flaudanum\VirtualBox VMs\VM-Fedora-fla\VM_Fedora_SGD-LAP-081-disk001.vdi" --resize 40960
0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
```


```
$ su -
Password: 
[root@localhost ~]# fdisk /dev/sda

Welcome to fdisk (util-linux 2.33.2).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): p
Disk /dev/sda: 40 GiB, 42949672960 bytes, 83886080 sectors
Disk model: VBOX HARDDISK   
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x08a03abb

Device     Boot    Start      End  Sectors Size Id Type
/dev/sda1  *        2048  2099199  2097152   1G 83 Linux
/dev/sda2        2099200 33554431 31455232  15G 8e Linux LVM
/dev/sda4       33554432 41943039  8388608   4G 8e Linux LVM

Command (m for help): n
Partition type
   p   primary (3 primary, 0 extended, 1 free)
   e   extended (container for logical partitions)
Select (default e): p

Selected partition 3
First sector (41943040-83886079, default 41943040): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (41943040-83886079, default 83886079): 

Created a new partition 3 of type 'Linux' and of size 20 GiB.

Command (m for help): t
Partition number (1-4, default 4): 3
Hex code (type L to list all codes): 8e

Changed type of partition 'Linux' to 'Linux LVM'.

Command (m for help): p
Disk /dev/sda: 40 GiB, 42949672960 bytes, 83886080 sectors
Disk model: VBOX HARDDISK   
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x08a03abb

Device     Boot    Start      End  Sectors Size Id Type
/dev/sda1  *        2048  2099199  2097152   1G 83 Linux
/dev/sda2        2099200 33554431 31455232  15G 8e Linux LVM
/dev/sda3       41943040 83886079 41943040  20G 8e Linux LVM
/dev/sda4       33554432 41943039  8388608   4G 8e Linux LVM

Partition table entries are not in disk order.

Command (m for help): w
The partition table has been altered.
Syncing disks.

[root@localhost ~]# fdisk -l /dev/sda
Disk /dev/sda: 40 GiB, 42949672960 bytes, 83886080 sectors
Disk model: VBOX HARDDISK   
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x08a03abb

Device     Boot    Start      End  Sectors Size Id Type
/dev/sda1  *        2048  2099199  2097152   1G 83 Linux
/dev/sda2        2099200 33554431 31455232  15G 8e Linux LVM
/dev/sda3       41943040 83886079 41943040  20G 8e Linux LVM
/dev/sda4       33554432 41943039  8388608   4G 8e Linux LVM

Partition table entries are not in disk order.
[root@localhost ~]# pvcreate /dev/sda3
  Physical volume "/dev/sda3" successfully created.
[root@localhost ~]# pvs
  PV         VG     Fmt  Attr PSize   PFree 
  /dev/sda2  fedora lvm2 a--  <15.00g     0 
  /dev/sda3         lvm2 ---   20.00g 20.00g
  /dev/sda4  fedora lvm2 a--   <4.00g     0 
[root@localhost ~]# vgextend fedora /dev/sda3
  Volume group "fedora" successfully extended
[root@localhost ~]# vgs
  VG     #PV #LV #SN Attr   VSize   VFree  
  fedora   3   2   0 wz--n- <38.99g <20.00g
[root@localhost ~]# pvscan
  PV /dev/sda2   VG fedora          lvm2 [<15.00 GiB / 0    free]
  PV /dev/sda4   VG fedora          lvm2 [<4.00 GiB / 0    free]
  PV /dev/sda3   VG fedora          lvm2 [<20.00 GiB / <20.00 GiB free]
  Total: 3 [<38.99 GiB] / in use: 3 [<38.99 GiB] / in no VG: 0 [0   ]
[root@localhost ~]# lvdisplay
  --- Logical volume ---
  LV Path                /dev/fedora/swap
  LV Name                swap
  VG Name                fedora
  LV UUID                ngsTI4-om3D-FeQK-EAip-aBRu-ckRq-abyfvw
  LV Write Access        read/write
  LV Creation host, time localhost-live, 2019-01-16 12:20:54 +0100
  LV Status              available
  # open                 2
  LV Size                1.60 GiB
  Current LE             410
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:1
   
  --- Logical volume ---
  LV Path                /dev/fedora/root
  LV Name                root
  VG Name                fedora
  LV UUID                DB1bIy-7B2x-HGJs-rMg0-TFf4-YZgB-ijw81y
  LV Write Access        read/write
  LV Creation host, time localhost-live, 2019-01-16 12:20:54 +0100
  LV Status              available
  # open                 1
  LV Size                17.39 GiB
  Current LE             4452
  Segments               2
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0
   
[root@localhost ~]# vgdisplay
  --- Volume group ---
  VG Name               fedora
  System ID             
  Format                lvm2
  Metadata Areas        3
  Metadata Sequence No  6
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                3
  Act PV                3
  VG Size               <38.99 GiB
  PE Size               4.00 MiB
  Total PE              9981
  Alloc PE / Size       4862 / 18.99 GiB
  Free  PE / Size       5119 / <20.00 GiB
  VG UUID               wfOPZk-IKZc-YLMo-jA1j-5OL7-Nhdm-nOipao
   
[root@localhost ~]# lvextend -l +5119 /dev/fedora/root
  Size of logical volume fedora/root changed from 17.39 GiB (4452 extents) to <37.39 GiB (9571 extents).
  Logical volume fedora/root successfully resized.
[root@localhost ~]# resize2fs /dev/fedora/root
resize2fs 1.44.6 (5-Mar-2019)
Filesystem at /dev/fedora/root is mounted on /; on-line resizing required
old_desc_blocks = 3, new_desc_blocks = 5
The filesystem on /dev/fedora/root is now 9800704 (4k) blocks long.

[root@localhost ~]# lvdisplay
  --- Logical volume ---
  LV Path                /dev/fedora/swap
  LV Name                swap
  VG Name                fedora
  LV UUID                ngsTI4-om3D-FeQK-EAip-aBRu-ckRq-abyfvw
  LV Write Access        read/write
  LV Creation host, time localhost-live, 2019-01-16 12:20:54 +0100
  LV Status              available
  # open                 2
  LV Size                1.60 GiB
  Current LE             410
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:1
   
  --- Logical volume ---
  LV Path                /dev/fedora/root
  LV Name                root
  VG Name                fedora
  LV UUID                DB1bIy-7B2x-HGJs-rMg0-TFf4-YZgB-ijw81y
  LV Write Access        read/write
  LV Creation host, time localhost-live, 2019-01-16 12:20:54 +0100
  LV Status              available
  # open                 1
  LV Size                <37.39 GiB
  Current LE             9571
  Segments               3
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0
   
[root@localhost ~]# 
```