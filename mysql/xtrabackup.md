# xtrabackup 备份 不停机主从复制、同步
## 安装
```
apt-get install percona-xtrabackup-24
```
## 备份
```
#备份指定库
innobackupex --defaults-file=/etc/my.cnf --socket=/tmp/mysql.sock --user=root --password=xxx --parallel=4 --databases="mall_amusic_shop oauth_amusic_shop im_amusic_shop pay_amusic_shop" /root/backup

#备份所有库
innobackupex --defaults-file=/etc/my.cnf --socket=/tmp/mysql.sock --user=root --password=xxx --parallel=4  /root/backup
```
```
ls -l /root/backup/
total 4
drwxr-x--- 6 root root 4096 Jan 30 01:01 2019-01-30_00-57-25
```

## 保持事务一致性
```
innobackupex --defaults-file=/etc/my.cnf --socket=/tmp/mysql.sock  --user=root --password=xxx --apply-log /root/backup/2019-01-30_00-57-25/
```

## 传输
```
scp -r -P 25680 2019-01-30_00-57-25 xwx@192.168.1.91:/home/xwx/
```

innobackupex --defaults-file=/etc/my.cnf --socket=/tmp/mysql.sock --user=root --password=xxx --copy-back /home/xwx/2019-01-30_00-57-25/

## 从库恢复
```
innobackupex --datadir=/data/mysql/3306 --copy-back /home/xwx/2019-01-30_00-57-25/
```

## 修复权限
```
chown -R mysql.mysql /data/mysql/3306
```

## 主库授权
```
grant replication slave on *.* to slave@'192.168.1.9%' identified by 'xwxyyq';
```
## 从库查看xtrabackup_binlog_info gtid_purged
```
cat /data/mysql/3306/xtrabackup_binlog_info 
mysql-bin.000033	71601523	4d83ee2d-11ad-11e9-953c-1866dae7c89c:1-4522979,
6c679363-11a5-11e9-8b86-1866dae7c89c:1-2
```
## 从库修改GTID_PURGE
```
reset slave all;
reset master;
SET GLOBAL GTID_PURGED='4d83ee2d-11ad-11e9-953c-1866dae7c89c:1-4522979,6c679363-11a5-11e9-8b86-1866dae7c89c:1-2'
```
## 从库连接主库
```
change master to master_host='192.168.1.91',master_user='slave',master_password='xwxyyq',master_port=3306,master_auto_position=1
start slave;
show slave status\G;
```

https://wsgzao.github.io/post/xtrabackup/