# CentOS 7 升级内核

## 查看当前内核版本
```
[root@db43 ~]# uname -r
3.10.0-957.el7.x86_64
```
## 导入仓库源
```
sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
sudo rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
```

# 安装指定版本 4.19
```
[root@db43 ~]# yum --enablerepo="elrepo-kernel" list --showduplicates | sort -r | grep kernel-ml.x86_64
kernel-ml.x86_64                            4.20.0-1.el7.elrepo        elrepo-kernel
kernel-ml.x86_64                            4.19.12-1.el7.elrepo       elrepo-kernel

[root@db43 ~]# yum --enablerepo="elrepo-kernel" install kernel-ml-4.19.12-1.el7.elrepo.x86_64 -y
```

```
[root@db43 etc]# sudo egrep ^menuentry /boot/grub2/grubenv | cut -f 2 -d \'
CentOS Linux (4.19.12-1.el7.elrepo.x86_64) 7 (Core)
CentOS Linux (3.10.0-957.el7.x86_64) 7 (Core)
CentOS Linux (0-rescue-4de75e64d2d54ea49d12a4b730b2e839) 7 (Core)
```
```
sudo grub2-set-default 0
grub2-mkconfig -o /boot/grub2/grub.cfg
```


# 错误解决
```
grub2-editenv: error: invalid environment block.

grub2-mkconfig -o /boot/grub2/grubenv 
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-4.19.12-1.el7.elrepo.x86_64
Found initrd image: /boot/initramfs-4.19.12-1.el7.elrepo.x86_64.img
Found linux image: /boot/vmlinuz-3.10.0-957.el7.x86_64
Found initrd image: /boot/initramfs-3.10.0-957.el7.x86_64.img
Found linux image: /boot/vmlinuz-0-rescue-4de75e64d2d54ea49d12a4b730b2e839
Found initrd image: /boot/initramfs-0-rescue-4de75e64d2d54ea49d12a4b730b2e839.img
done
sudo grub2-set-default 0
grub2-mkconfig -o /boot/grub2/grubenv 
```
