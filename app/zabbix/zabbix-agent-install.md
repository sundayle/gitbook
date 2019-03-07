# zabbix-agent 安装
https://www.zabbix.com/download?zabbix=3.0&os_distribution=centos&os_version=7&db=mysql
centos

```
rpm -Uvh https://repo.zabbix.com/zabbix/3.0/rhel/7/x86_64/zabbix-release-3.0-1.el7.noarch.rpm
cp /etc/yum.repos.d/zabbix.repo{,.bak}
sed -i 's#http://repo.zabbix.com/zabbix#https://mirrors.tuna.tsinghua.edu.cn/zabbix/zabbix#' /etc/yum.repos.d/zabbix.repo
sed -i 's#http://repo.zabbix.com/non-supported#https://mirrors.tuna.tsinghua.edu.cn/zabbix/non-supported#' 
/etc/yum.repos.d/zabbix.repo
yum install -y zabbix-agent

cp /etc/zabbix/zabbix_agentd.conf{,.bak}
sed -i 's@^Server=.*@Server=192.168.1.39@' /etc/zabbix/zabbix_agentd.conf 
sed -i 's@^ServerActive=.*@ServerActive=192.168.1.39@' /etc/zabbix/zabbix_agentd.conf 
sed -i 's@^Hostname=@# Hostname=@' /etc/zabbix/zabbix_agentd.conf
sed -i 's@^# HostnameItem=.*@HostnameItem=system.hostname@'  /etc/zabbix/zabbix_agentd.conf

systemctl start zabbix-agent
systemctl enable zabbix-agent
```