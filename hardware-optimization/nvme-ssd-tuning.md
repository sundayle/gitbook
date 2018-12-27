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
```
systemctl disable irqbalance
systemctl stop irqbalance

```


https://itpeernetwork.intel.com/tuning-performance-intel-optane-ssds-linux-operating-systems/#gs.WVTfE3I