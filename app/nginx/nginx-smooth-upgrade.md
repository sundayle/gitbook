```
mv /usr/local/webserver/nginx/sbin/nginx{,.bak}
cp /usr/local/src/nginx-1.12.1/objs/nginx /usr/local/webserver/nginx/sbin/nginx

nginx -V
nginx -t

使用新版本的Nginx文件启动服务，之后平缓停止原有Nginx进
kill -USR2 `cat /usr/local/webserver/nginx/logs/nginx.pid`
kill -WINCH `cat /usr/local/webserver/nginx/logs/nginx.pid.oldbin`

ps -aux | grep nginx
```

http://kgdbfmwfn.blog.51cto.com/5062471/1708258