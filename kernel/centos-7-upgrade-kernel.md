# CentOS 7 升级内核

## 查看当前内核版本
```
uname -r
3.10.0-957.el7.x86_64
```
## 导入仓库源
```
sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
sudo rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
```

# 安装指定版本 4.19
```
yum --enablerepo="elrepo-kernel" list --showduplicates | sort -r | grep kernel-ml.x86_64
kernel-ml.x86_64                            4.20.0-1.el7.elrepo        elrepo-kernel
kernel-ml.x86_64                            4.19.12-1.el7.elrepo       elrepo-kernel

yum --enablerepo="elrepo-kernel" install kernel-ml-4.19.12-1.el7.elrepo.x86_64 kernel-ml-devel-4.19.12-1.el7.elrepo.x86_64 -y
```
# 传统BIOS引导

```
egrep ^menuentry /boot/grub2/grub.cfg | cut -f 2 -d \'

CentOS Linux (4.19.12-1.el7.elrepo.x86_64) 7 (Core)
CentOS Linux (3.10.0-957.el7.x86_64) 7 (Core)
CentOS Linux (0-rescue-4de75e64d2d54ea49d12a4b730b2e839) 7 (Core)
```

```
grub2-set-default 0
grub2-mkconfig -o /boot/grub2/grub.cfg
```

# UEFI引导
```
egrep ^menuentry /etc/grub2-efi.cfg | cut -f 2 -d \' 
grub2-set-default 0
grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg
```

https://access.redhat.com/solutions/1605183


```
yum groupinstall -y "Development Tools" 
yum install -y ncurses-devel qt-devel

https://www.kernel.org/
wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.19.12.tar.xz
tar xf  linux-4.19.12.tar.xz 
cd linux-4.19.12
```

# 查找清除旧内核 
rpm -qa | grep kernel



