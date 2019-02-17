# Nginx IP白名单 反向代理
指定IP为103.105.17.23\* 进行upsteam双机轮询  
指定IP非103.105.17.23\* 进行upsteam单机不轮询

```
upstream pay.open.web {
    server src1.pay.open.sundayle.com:80;
#    server src2.pay.open.sundayle.com:80;
    check interval=2000 rise=1 fall=3 timeout=3000;
#    ip_hash;
}

upstream pay.open.cluster {
    server src1.pay.open.sundayle.com:80;
    server src2.pay.open.sundayle.com:80;
    check interval=2000 rise=1 fall=3 timeout=3000;
#    ip_hash;
}

    location / {
        proxy_pass http://pay.open.web/;
        proxy_set_header Host pay.open.sundayle.com;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_redirect off;
        if ($remote_addr ~* 103.115.17.23*) {
          proxy_pass http://pay.open.cluster;
        }
    }
```


https://www.cyberciti.biz/faq/nginx-redirect-backend-traffic-based-upon-client-ip-address/