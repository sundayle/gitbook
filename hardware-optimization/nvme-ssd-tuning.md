# NVME PCIE SSD 优化
```
cat /etc/fstab

/dev/md124 /data         xfs defaults,noatime,nobarrier    0 0
```

```
hdparm -Tt --direct /dev/nvme0n1p4
/dev/nvme0n1p4:
 Timing O_DIRECT cached reads:   5894 MB in  2.00 seconds = 2949.45 MB/sec
 Timing O_DIRECT disk reads: 8964 MB in  3.00 seconds = 2987.50 MB/sec
```

https://itpeernetwork.intel.com/tuning-performance-intel-optane-ssds-linux-operating-systems/#gs.WVTfE3I

## 将CPU置于性能模式
```
for CPUFREQ in /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor; do [ -f $CPUFREQ ] || continue; echo -n performance > $CPUFREQ; done

cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```
## 禁用irqbalance
在版本4.8之前的内核中，irq平衡没有得到有效管理，因为它现在由内置的Linux nvme驱动程序实现。因此，如果你使用的是比4.8更旧的内核，请关闭irqbalance服务,以便尽可能地进行最佳的io处理。
```
systemctl disable irqbalance
systemctl stop irqbalance
```


https://itpeernetwork.intel.com/tuning-performance-intel-optane-ssds-linux-operating-systems/#gs.WVTfE3I

## R730XD PCIE 固态 风扇狂转解决
```
yum install OpenIPMI OpenIPMI-tools
chkconfig ipmi on  # << optional for the task
service ipmi start  # << optional for the task
```

Query Dell's Third-Party PCIe card based default system fan response:
```
ipmitool raw 0x30 0xce 0x01 0x16 0x05 0x00 0x00 0x00

# response like below means Disabled
16 05 00 00 00 05 00 01 00 00

# response like below means Enabled
16 05 00 00 00 05 00 00 00 00
```
Jets off or Set Third-Party PCIe Card Default Cooling Response Logic To Disabled:
```
ipmitool raw 0x30 0xce 0x00 0x16 0x05 0x00 0x00 0x00 0x05 0x00 0x01 0x00 0x00 
```

Jets on or Set Third-Party PCIe Card Default Cooling Response Logic To Enabled:
```
ipmitool raw 0x30 0xce 0x00 0x16 0x05 0x00 0x00 0x00 0x05 0x00 0x00 0x00 0x00 
```
https://www.dell.com/community/PowerEdge%E6%9C%8D%E5%8A%A1%E5%99%A8%E8%AE%A8%E8%AE%BA%E5%8C%BA/DELL-R730xd-%E5%8A%A0%E8%A3%85PCIE%E5%9B%BA%E6%80%81%E7%A1%AC%E7%9B%98-%E9%A3%8E%E6%89%87%E9%97%AE%E9%A2%98/m-p/5276074