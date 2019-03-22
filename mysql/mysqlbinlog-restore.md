# MySQL 通过 mysqlbinlog 恢复数据库

## 查看命令
```
mysqlbinlog -v --base64-output=decode-rows --skip-gtids=true \
--start-datetime='2019-03-12 3:00:00' --stop-datetime='2019-03-12 11:13:00' \
-d mall_amusic_shop mysql-bin.000125
```

## 恢复步骤
### 1.导入昨晚3点全量备份
```
show master status;
create database mall_amusic;
mysql mall_amusic < /data/bak/mysql/mall_amusic_2018_03-12.sql
```

### 2.binlog 重放恢复指定时间段数据
```
mysqlbinlog --skip-gtids=true --start-datetime='2019-03-12 3:00:00' \
--stop-datetime='2019-03-12 11:13:00' -d mall_amusic mysql-bin.000125 > backup.sql
mysql < backup.sql
```

https://www.sundayle.com/mysqlbinlog-gtid/