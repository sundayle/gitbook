# CentOS7 php curl 微信支付接口 超时

## 修改resolv.conf 

## 添加options single-request-reopen

```
cat << EOF> /etc/resolv.conf
options single-request-reopen
options timeout:1 attempts:1 rotate

nameserver 114.114.114.114
nameserver 119.29.29.29
EOF
```

https://www.sundayle.com/linux-php-curl-timeout/

