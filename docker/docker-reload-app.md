# PHP-FPM Reload
php-fpm是一个进程管理器，它支持USER2信号，用于重新加载配置文件

在容器内命令
```
kill -USR2 1
```
在容器外命令
```
docker exec -it <mycontainer> kill -USR2 1
Complete example:

docker run -d --name test123 php:7.1-fpm-alpine
docker exec -it test123 ps aux
docker exec -it test123 kill -USR2 1
docker exec -it test123 ps aux
```