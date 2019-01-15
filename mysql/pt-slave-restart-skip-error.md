# 使用Percona工具pt-slave-restart跳过错误

在从库执行

```
mysql> set global slave_parallel_workers=0;
```
```
pt-slave-restart -h localhost -uroot -pmysql -S /tmp/mysql.sock
#pt-slave-restart -h localhost -uroot -pmysql -S /tmp/mysql.sock --error-numbers=1062
```
执行进程
```
2019-01-15T23:33:48 h=localhost,u=root store42-relay-bin.000019     2618841 1062 
2019-01-15T23:33:48 h=localhost,u=root store42-relay-bin.000019     2626901 1062 
2019-01-15T23:33:48 h=localhost,u=root store42-relay-bin.000019     2629369 1062 
2019-01-15T23:33:48 h=localhost,u=root store42-relay-bin.000019     2631201 1062 
```
查看执行结果
```
show slave status\G;
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 192.168.1.41
                  Master_User: slave
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000784
          Read_Master_Log_Pos: 3163796
               Relay_Log_File: store42-relay-bin.000020
                Relay_Log_Pos: 35503
        Relay_Master_Log_File: mysql-bin.000784
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
... 
1 row in set (0.00 sec)

ERROR: 
No query specified
```
```
mysql> set global slave_parallel_workers=8;
```

跳过错误成功后，使用pt-table-checksum + pt-table-sync 同步不一致数据