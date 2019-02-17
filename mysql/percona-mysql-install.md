# Percona MySQL 二进制安装
## 安装
```
useradd -s /bin/false mysql
mkdir -pv /data/mysql/3306
mkdir -pv /data/logs/mysql
chown -R mysql.mysql /data/logs/mysql/
chown -R mysql.mysql /data/mysql/3306/data

cp /usr/local/webserver/mysql/support-files/mysql.server /etc/init.d/mysql
chmod +x /etc/init.d/mysql
update-rc.d mysql defaults
update-rc.d mysql enable
```
## 导入环境变量
```
cat << EOF >> /etc/profile.d/mysql.sh
export PATH=/usr/local/webserver/mysql/bin:$PATH
EOF
source /etc/profile.d/mysql.sh
```
## 初始化 

```
#无密码： 使用mysqld --initialize 初始化
mysqld --initialize-insecure --user=mysql --basedir=/usr/local/webserver/mysql --datadir=/data/mysql/3306 --log-error=/data/logs/mysql/error.log --pid-file=/data/mysql/3306/mysql.pid --socket=/data/mysql/3306/mysql.sock

#有密码： 密码在/data/logs/mysql/error.log查看 
mysqld --initialize --user=mysql --basedir=/usr/local/webserver/mysql --datadir=/data/mysql/3306 --log-error=/data/logs/mysql/error.log --pid-file=/data/mysql/3306/mysql.pid --socket=/data/mysql/3306/mysql.sock
```
## 修改用户密码
必须使用alter user 重置密码：
```
mysql> show databases;
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '********'
```
修改密码
```
update mysql.user set authentication_string=password('E2x/LFDq6W7Z3,Q7qV#fcNy+') where user='root' and Host = 'localhost'
```

ps: mysql5.7 密码是 authentication_string 
openssl rand -base64 18

## 授权

创建用户或权限
```
GRANT ALL PRIVILEGES ON copyright.* TO 'copyright'@'113.105.17.23%' IDENTIFIED BY 'C5#jM5xZ9YGsw&4oJjOjizsWzyqP*iaSvtdC7il0+QU' WITH GRANT OPTION;
```


## 导入数据
mysql> create database cp;
exit
mysql -uroot -hlocalhost -p cp < cp.sq

