# 主从同步不一致

```
                   Last_Errno: 1197
                   Last_Error: Coordinator stopped because there were error(s) in the worker(s). The most recent failure being: Worker 4 failed executing transaction '607e7112-8c78-11e7-923d-801844e0bbe8:1992420' at master log mysql-bin.000767, end_log_pos 68191221. See error log and/or performance_schema.replication_applier_status_by_worker table for more details about this failure or others, if any.
```

```
STOP SLAVE;
SET @@SESSION.GTID_NEXT = '607e7112-8c78-11e7-923d-801844e0bbe8:1992420';
BEGIN; COMMIT;
SET @@SESSION.GTID_NEXT = AUTOMATIC;
START SLAVE;
```