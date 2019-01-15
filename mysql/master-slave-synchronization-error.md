# 主从同步不一致

```

        Last_SQL_Errno: 1197
        Last_SQL_Error: Coordinator stopped because there were error(s) in the worker(s). The most recent failure being: Worker 4 failed executing transaction '607e7112-8c78-11e7-923d-801844e0bbe8:1992764' at master log mysql-bin.000767, end_log_pos 258072145. See error log and/or performance_schema.replication_applier_status_by_worker table for more details about this failure or others, if any.
```
# 查看详情
```
mysql> select * from performance_schema.replication_applier_status_by_worker where LAST_ERROR_NUMBER=1197\G;
*************************** 1. row ***************************
         CHANNEL_NAME: 
            WORKER_ID: 4
            THREAD_ID: NULL
        SERVICE_STATE: OFF
LAST_SEEN_TRANSACTION: 607e7112-8c78-11e7-923d-801844e0bbe8:1992764
    LAST_ERROR_NUMBER: 1197
   LAST_ERROR_MESSAGE: Worker 4 failed executing transaction '607e7112-8c78-11e7-923d-801844e0bbe8:1992764' at master log mysql-bin.000767, end_log_pos 258072145; Could not execute Write_rows event on table mall_amusic_shop.ben_logistics_notify_log; Multi-statement transaction required more than 'max_binlog_cache_size' bytes of storage; increase this mysqld variable and try again, Error_code: 1197; Writing one row to the row-based binary log failed, Error_code: 1534; handler error HA_ERR_RBR_LOGGING_FAILED; the event's master log mysql-bin.000767, end_log_pos 258072145
 LAST_ERROR_TIMESTAMP: 2019-01-03 13:32:08
1 row in set (0.00 sec)

ERROR: 
No query specified
```

# 跳过错误
```
stop slave;
set gtid_next = '607e7112-8c78-11e7-923d-801844e0bbe8:1992764';
begin;commit;
set gtid_next=automatic;
start slave;
show slave status \G;
```



               Last_SQL_Errno: 1864
               Last_SQL_Error: Cannot schedule event Rows_query, relay-log name ./store42-relay-bin.001148, position 253381985 to Worker thread because its size 16777219 exceeds 16777216 of slave_pending_jobs_size_max.

```
stop slave;
set global slave_pending_jobs_size_max=20000000;
start slave;
```

https://yq.aliyun.com/sqlarticle/68208


```
               Last_SQL_Errno: 1197
               Last_SQL_Error: Could not execute Write_rows event on table mall_amusic_shop.ben_order_suit_product_detail; Multi-statement transaction required more than 'max_binlog_cache_size' bytes of storage; increase this mysqld variable and try again, Error_code: 1197; Writing one row to the row-based binary log failed, Error_code: 1534; handler error HA_ERR_RBR_LOGGING_FAILED; the event's master log mysql-bin.000768, end_log_pos 75178914
```

```       
#调整max_binlog_cache_size为2G   2048*1024*1024
set global max_binlog_cache_size=2147483648;
```







