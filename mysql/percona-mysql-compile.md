# Percona MySQL 编译
```
apt-get -y install cmake g++ bison libssl-dev libxml2-dev  libncurses5-dev libreadline-dev libnuma-dev libaio-dev

yum -y install gcc gcc-c++ cmake bison ncurses-devel readine-devel libaio-devel
```

## 解决boost1.59依赖问题
下载boost1.59到/usr/local/lib/bootst,编译参数加入
-DDOWNLOAD_BOOST=1 -DWITH_BOOST=/usr/local/lib/boost/

```
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/webserver/mysql \
-DMYSQL_DATADIR=/data/mysql/3306 \
-DENABLED_LOCAL_INFILE=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DEXTRA_CHARSETS=all \
-DDEFAULT_CHARSET=utf8mb4 \
-DDEFAULT_COLLATION=utf8mb4_unicode_ci \
-DDOWNLOAD_BOOST=1 \
-DWITH_BOOST=/usr/local/src/boost_1_59_0
```


## 配置
```
useradd mysql -s /bin/false
mkdir -pv /data/mysql/3306/data
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
cat << EOF > /etc/profile.d/mysql.sh
export PATH=/usr/local/webserver/mysql/bin:$PATH
EOF
source /etc/profile.d/mysql.sh
```
## 初始化
```
使用/etc/my.cnf寝化
mysqld --initialize-insecure
```
```
#无密码： 使用mysqld --initialize 初始化
mysqld --initialize-insecure --user=mysql --basedir=/usr/local/webserver/mysql --datadir=/data/mysql/3306 --log-error=/data/logs/mysql/error.log

#有密码： 密码在/data/logs/mysql/error.log查看 
mysqld --initialize --user=mysql --basedir=/usr/local/webserver/mysql --datadir=/data/mysql/3306 --log-error=/data/logs/mysql/error.log
```

## 激活jemalloc
无需在mysql编译，直接在mysqld_safe修改就行
```
vim /usr/local/webserver/mysql/bin/mysqld_safe

mysqld_ld_preload=/usr/local/jemalloc/lib/libjemalloc.so.2
load_jemalloc=1
```
```
lsof -n | grep jemalloc
```