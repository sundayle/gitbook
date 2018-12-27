Linux 软RAID


```
[root@db43 ~]# lsblk 
NAME        MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
nvme0n1     259:0    0 745.2G  0 disk  
├─nvme0n1p1 259:1    0    40G  0 part  
│ └─md127     9:127  0    40G  0 raid1 /
├─nvme0n1p2 259:2    0    16G  0 part  
│ └─md126     9:126  0    16G  0 raid1 [SWAP]
├─nvme0n1p3 259:3    0   501M  0 part  
│ └─md125     9:125  0   501M  0 raid1 /boot/efi
└─nvme0n1p4 259:4    0 688.7G  0 part  
nvme1n1     259:5    0 745.2G  0 disk  
├─nvme1n1p1 259:6    0    40G  0 part  
│ └─md127     9:127  0    40G  0 raid1 /
├─nvme1n1p2 259:7    0    16G  0 part  
│ └─md126     9:126  0    16G  0 raid1 [SWAP]
├─nvme1n1p3 259:8    0   501M  0 part  
│ └─md125     9:125  0   501M  0 raid1 /boot/efi
└─nvme1n1p4 259:9    0 688.7G  0 part  
```
创建RAID1
```
mdadm -C /dev/md124 -l1 -n2 /dev/nvme0n1p4 /dev/nvme1n1p4
```