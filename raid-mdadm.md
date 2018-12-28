# Linux 软RAID


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

## 创建RAID1
```
mdadm -C /dev/md124 -l1 -n2 /dev/nvme0n1p4 /dev/nvme1n1p4
```
## 查看进度
```
mdadm -D /dev/md124
cat /proc/mdstat 
Personalities : [raid1] 
md124 : active raid1 nvme1n1p4[1] nvme0n1p4[0]
      722022208 blocks super 1.2 [2/2] [UU]
      [===>.................]  resync = 15.2% (110058496/722022208) finish=12.1min speed=838161K/sec
      bitmap: 6/6 pages [24KB], 65536KB chunk

md125 : active raid1 nvme1n1p3[1] nvme0n1p3[0]
      512960 blocks super 1.0 [2/2] [UU]
      bitmap: 0/1 pages [0KB], 65536KB chunk

md126 : active raid1 nvme1n1p2[1] nvme0n1p2[0]
      16758784 blocks super 1.2 [2/2] [UU]
      
md127 : active raid1 nvme1n1p1[1] nvme0n1p1[0]
      41942016 blocks super 1.2 [2/2] [UU]
      bitmap: 0/1 pages [0KB], 65536KB chunk

unused devices: <none>
```
## 格式化
```
mkfs.xfs -f /dev/md124
meta-data=/dev/md124             isize=512    agcount=4, agsize=45126388 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=180505552, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=88137, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
```
## 挂载
```
mdkir /data
mount /dev/md124 /data
```
## fstab
```
blkid /dev/md124
/dev/md124: UUID="2f595714-9533-4580-9b7e-b8563dcdcd5f" TYPE="xfs" 

UUID=2f595714-9533-4580-9b7e-b8563dcdcd5f /data         xfs defaults,noatime,nobarrier    0 0
```
## mdadm.conf
```
mdadm -Ds > /etc/mdadm.conf
```
## 邮件报警
```
echo "MAILADDR admin@sundayle.com" >> /etc/mdadm.conf
systemctl start mdmonitor
systemctl status mdmonitor
```
# 模拟故障
设置故障
```
mdadm --manage --set-faulty /dev/md124 /dev/nvme0n1p4 
```
查看邮件 有报警信息

故障移除
mdadm /dev/md124 --remove /dev/nvme0n1p4
mdadm --manage /dev/md124 --add /dev/nvme0n1p4



# NVME PCIE P3700 故障恢复

制作CentOS 7.5 系统启动盘
进入恢复模式
mdadm --examine /dev/nvme0n1p1

挂载单个磁盘使用 --run
mdadm --assemble /dev/md0 dev/nvme0n1p1 --run
mount /dev/md0 /mnt
拷贝数据

组装阵列
mdadm --assemble /dev/md0 /dev/nvme0n1p1 /dev/nvme1n1p1


https://serverfault.com/questions/404586/reading-off-of-mdadm-drives-after-server-died

https://www.digitalocean.com/community/tutorials/how-to-manage-raid-arrays-with-mdadm-on-ubuntu-16-04
