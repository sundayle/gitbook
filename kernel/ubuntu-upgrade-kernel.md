# Ubuntu 更新内核

## 下载内核
https://kernel.ubuntu.com/~kernel-ppa/mainline/

```
wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.12.9/linux-headers-4.12.9-041209_4.12.9-041209.201708242344_all.deb

wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.12.9/linux-headers-4.12.9-041209-generic_4.12.9-041209.201708242344_amd64.deb

wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.12.9/linux-image-4.12.9-041209-generic_4.12.9-041209.201708242344_amd64.deb
```
## 安装

```
sudo dpkg -i *.deb

reboot
uname -r
```



## 查看内核列表

```
dpkg --get-selections |grep linux-image
```

## 删除内核

```
apt-get remove linux-image
```



## 切换内核

```
#vim /etc/default/grub
#GRUB_DEFAULT = 0

grep -r  "menuentry 'Ubuntu, with" /boot/grub/grub.cfg
grub-set-default 0 
update-grub
```

