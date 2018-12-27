# CentOS 7 升级内核
## 查看当前内核版本
```
[root@db43 ~]# uname -r
3.10.0-957.el7.x86_64
```

```
sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
sudo rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
```
# 安装指定版本 4.19
```
[root@db43 ~]# yum --enablerepo="elrepo-kernel" list --showduplicates | sort -r | grep kernel-ml.x86_64
kernel-ml.x86_64                            4.20.0-1.el7.elrepo        elrepo-kernel
kernel-ml.x86_64                            4.19.12-1.el7.elrepo       elrepo-kernel

```

