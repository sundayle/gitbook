# 使用Percona 工具同步不一致数据

## 生成不一致表 percona.checksums
```
pt-table-checksum --nocheck-replication-filters --no-check-binlog-format --databases=mall_music --replicate=percona.checksums h=192.168.1.41,u=pt_checksums,p='pt_checksums',P=3306
```

## 查看不一致信息
```
pt-table-sync --replicate=percona.checksums h=192.168.1.41,u=pt_checksums,p='pt_checksums',P=3306 h=192.168.1.42,u=pt_checksums,p='pt_checksums',P=3306 --print
```

## 同步
```
pt-table-sync --replicate=percona.checksums h=192.168.1.41,u=pt_checksums,p='pt_checksums',P=3306 h=192.168.1.42,u=pt_checksums,p='pt_checksums',P=3306 --execute
```

https://www.sundayhk.com/pt-table-checksum/