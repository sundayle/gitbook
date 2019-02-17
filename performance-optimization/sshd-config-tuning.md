```
vi /etc/ssh/sshd_config
GSSAPIAuthentication no
#通用安全服务应用程序接口(GSSAPI) 是为了让程序能够访问安全服务的一个应用程序接口，取消这个认证。
UseDNS no
#DNS反向解析，设置为no
```