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