# Nginx 灰色发布

灰度发布是指在黑与白之间，能够平滑过渡的一种发布方式。AB test就是一种灰度发布方式，让一部分用户继续用A，一部分用户开始用B，如果用户对B没有什么反对意见，那么逐步扩大范围，把所有用户都迁移到B上面来。

灰度发布可以保证整体系统的稳定，在初始灰度的时候就可以发现、调整问题，以保证其影响度

# 基于Cookie

## IF判断实现
```
upstream api37 {
    server 192.168.1.37:8080 max_fails=1 fail_timeout=30;
}

upstream api47 {
    server 192.168.1.47:8080 max_fails=1 fail_timeout=30;
}

upstream default {
    server 192.168.1.41:8080 max_fails=1 fail_timeout=30;
    server 192.168.1.42:8080 max_fails=1 fail_timeout=30;
}

server {
  listen 80;
  server_name  api.sundayle.com;

  set $up_server "default";
    if ($http_cookie ~* "version=V1"){
        set $group api37;
    }

    if ($http_cookie ~* "version=V2"){
        set $group api47;
    }

  location / {                       
    proxy_pass http://$up-server;
    proxy_set_header   Host             $host;
    proxy_set_header   X-Real-IP        $remote_addr;
    proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
  }
 }
```

## MAP匹配实现
```
upstream api37 {
    server 192.168.1.37:8080 max_fails=1 fail_timeout=30;
}

upstream api47 {
    server 192.168.1.47:8080 max_fails=1 fail_timeout=30;
}

upstream default {
    server 192.168.1.41:8080 max_fails=1 fail_timeout=30;
    server 192.168.1.42:8080 max_fails=1 fail_timeout=30;
}

map $COOKIE_version $up-server {
~*V1$ api37;
~*V2$ api47;
default default;
}

server {
  listen 80;
  server_name  api.sundayle.com;

  location / {                       
    proxy_pass http://$up-server;
    proxy_set_header   Host             $host;
    proxy_set_header   X-Real-IP        $remote_addr;
    proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
  }
 }
```

# 基于IP绑定
```
upstream api37 {
    server 192.168.1.37:8080 max_fails=1 fail_timeout=30;
}

upstream api47 {
    server 192.168.1.47:8080 max_fails=1 fail_timeout=30;
}

upstream default {
    server 192.168.1.41:8080 max_fails=1 fail_timeout=30;
    server 192.168.1.42:8080 max_fails=1 fail_timeout=30;
}

server {
  listen 80;
  server_name  api.sundayle.com;
  
  set $up-server default;
    if ($remote_addr ~ "192.168.10.251") {
      set $up-server api37;
    }

  location / {                       
    proxy_pass http://$up-server;
    proxy_set_header   Host             $host;
    proxy_set_header   X-Real-IP        $remote_addr;
    proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
  }
 }
```