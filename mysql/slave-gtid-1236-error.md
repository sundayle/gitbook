# MySQL Gtid 主从同步1236错误

## slave报错
```
 show slave status\G;
 ...
 Last_IO_Errno: 1236
                Last_IO_Error: Got fatal error 1236 from master when reading data from binary log: 'The slave is connecting using CHANGE MASTER TO MASTER_AUTO_POSITION = 1, but the master has purged binary logs containing GTIDs that the slave requires.'
               Last_SQL_Errno: 0
...
```
## 解决
在master执行,查询gtid_purged，记录下改值
```
mysql> show global variables like '%gtid%';
+----------------------------------+------------------------------------------------+
| Variable_name                    | Value                                          |
+----------------------------------+------------------------------------------------+
| binlog_gtid_simple_recovery      | ON                                             |
| enforce_gtid_consistency         | ON                                             |
| gtid_executed                    | 607e7112-8c78-11e7-923d-801844e0bbe8:1-2224356 |
| gtid_executed_compression_period | 1000                                           |
| gtid_mode                        | ON                                             |
| gtid_owned                       |                                                |
| gtid_purged                      | 607e7112-8c78-11e7-923d-801844e0bbe8:1-2137912 |
| session_track_gtids              | OFF                                            |
+----------------------------------+------------------------------------------------+
```
