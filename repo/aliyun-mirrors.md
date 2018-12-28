# CentOS 7 阿里云YUM和EPEL
```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

#设置yum源为阿里云
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

#添加epel源
wget -P /etc/yum.repos.d/ http://mirrors.aliyun.com/repo/epel-7.repo

#清理缓存并生成新的缓存
yum clean all 
yum makecache
```