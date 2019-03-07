nginx 1.1以上版本的upstream已经支持keep-alive的，所以我们可以开启Nginx proxy的keep-alive来减少tcp连接：

upstream http_backend {
 server 127.0.0.1:8080;

keepalive 16;
}

server {
 ...

location /http/ {
 proxy_pass http://http_backend;
 proxy_http_version 1.1;
 proxy_set_header Connection "";
 ...
 }
}

