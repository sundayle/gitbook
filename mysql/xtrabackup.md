
# 备份
innobackupex --defaults-file=/etc/my.cnf --socket=/tmp/mysql.sock --user=root --password=E2x/LFDq6W7Z3,Q7qV#fcNy+ --parallel=4 --databases="mall_amusic_shop oauth_amusic_shop im_amusic_shop pay_amusic_shop" /root/backup

ls -l /root/backup/
total 4
drwxr-x--- 6 root root 4096 Jan 30 01:01 2019-01-30_00-57-25

# 保持事务一致性

innobackupex --defaults-file=/etc/my.cnf --socket=/tmp/mysql.sock  --user=root --password=E2x/LFDq6W7Z3,Q7qV#fcNy+ --database="mall_amusic_shop im_amusic_shop pay_amusic_shop oauth_amusic_shop" --apply-log /root/backup/2019-01-30_00-57-25/

# 传输
scp -r -P 25680 2019-01-30_00-57-25 xwx@192.168.1.91:/home/xwx/

innobackupex --defaults-file=/etc/my.cnf --socket=/tmp/mysql.sock --user=root --password=E2x/LFDq6W7Z3,Q7qV#fcNy+ --copy-back /home/xwx/2019-01-30_00-57-25/

从库恢复
innobackupex --datadir=/data/mysql/3306 --copy-back /home/xwx/2019-01-30_00-57-25/

修复权限
chown -R mysql.mysql /data/mysql/3306



innobackupex --defaults-file=/etc/my.cnf --socket=/tmp/mysql.sock --user=root --password=E2x/LFDq6W7Z3,Q7qV#fcNy+ --parallel=4 /root/backup


innobackupex --defaults-file=/etc/my.cnf --socket=/tmp/mysql.sock  --user=root --password=E2x/LFDq6W7Z3,Q7qV#fcNy+ --apply-log /root/backup/2019-01-30_01-26-13



grant replication slave on *.* to slave@'192.168.1.9%' identified by 'xwxyyq';


cat xtrabackup_binlog_info 
mysql-bin.000033	71601523	4d83ee2d-11ad-11e9-953c-1866dae7c89c:1-4522979,
6c679363-11a5-11e9-8b86-1866dae7c89c:1-2

reset slave all;
reset master;
SET @@GLOBAL.GTID_PURGED='4d83ee2d-11ad-11e9-953c-1866dae7c89c:1-4522979,6c679363-11a5-11e9-8b86-1866dae7c89c:1-2'

change master to master_host='192.168.1.91',master_user='slave',master_password='xwxyyq',master_port=3306,master_auto_position=1
start slave;
show slave status\G:
