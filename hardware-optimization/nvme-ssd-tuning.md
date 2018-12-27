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