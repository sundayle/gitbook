# MySQL命令
mysql 5.7版本修改root密码 *特别提醒注意的一点是，新版的mysql数据库下的user表中已经没有Password字段了，password字段已经变更为authentication_string

方式一：
update mysql.user set authentication_string=password('%.BFb2lQ6G!g') where user='root' and Host = 'localhost';
alter user 'root'@'localhost' identified by '%.BFb2lQ6G!g';

alter user 'root'@'localhost' identified by 'Star-xwx';
mysql> flush privileges;

注意 密码需要复杂点，不然会报错
方式二：
set password for 'root'@'localhost'=password('%.BFb2lQ6G!g')
alter user 'root'@'localhost' identified by '%.BFb2lQ6G!g';

注意：如果只想设置简单密码需要修改两个全局参数：
mysql> set global validate_password_policy=0;
mysql> set global validate_password_length=1;

创建数据库
CREATE DATABASE xiaoyaoji CHARACTER SET = utf8mb4 COLLATE =  utf8mb4_unicode_ci;
grant all on xiaoyaoji.* to 'xwxxyj'@'localhost' identified by 'mixboy';
grant all on xiaoyaoji.* to 'xwxxyj'@'127.0.0.1' identified by 'mixboy';

导入数据库
mysql -uxwxxyj -hlocalhost -p xiaoyaoji < xiaoyaoji-0609.sql


查看
show processlist;
show full processlist;

show global status like 'Max_used_connections'
show global status like '%connections%'

授权及创建用户：
grant all on db_name.* to user_name@'host' identified by 'password';


授权：
grant all privileges on testDB.* to test@localhost identified by '1234';
grant select,delete,update,create,drop on *.* to test@"%" identified by "1234";

revoke 跟 grant 的语法差不多，只需要把关键字 “to” 换成 “from”
GRANT PROXY ON ''@'' TO 'gitlab'@'localhost' WITH GRANT OPTION 

revoke proxy on ''@'' from gitlab@localhost;

flush privileges;

查看用户权限
show grants
show grants for www_xwx_cn@localhost;

删除用户：
Delete FROM user Where User='test' and Host='localhost';

删除用户及权限
drop user user_name@ localhost;

修改密码：
update mysql.user set authentication_string=password('new_password') where user='root' and Host ='localhost';

update mysql.user set authentication_string=password('p6EH79LGVL7ZvtjG') where user='copyright' and Host ='localhost';

ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement

set password = password('123456');


创建带. www.xwx.cn
create database `www.xwx.cn`;

创建指定字符
CREATE DATABASE `xwx` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;


show engine innodb status\G

关闭密码复杂
在/etc/my.cnf配置文件中增加
[mysqld]
validate_password=off
更改密码策略为LOW
或
set global validate_password_policy=0;
更改密码长度
set global validate_password_length=0;


show engine innodb status;


update mysql.user set authentication_string=password('Star-xwx') where user='root' and Host ='localhost';