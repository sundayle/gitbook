# CentOS 7 Firewalld
```
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone=public --add-port=443/tcp --permanent
firewall-cmd --zone=public --add-port=25680/tcp --permanent
firewall-cmd --zone=public --add-rich-rule="rule family="ipv4" source address=192.168.1.0/24 accept" --permanent
firewall-cmd --zone=public --add-rich-rule="rule family=ipv4 source firewall-cmd --zone=public --add-rich-rule="rule family=ipv4 source address=113.105.17.232/29 accept" --permanent
```
https://www.sundayhk.com/firewalld/