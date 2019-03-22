# Docker 中文乱码

临时修改
```
LANG=C.UTF-8

echo "LANG=C.UTF-8" >> /etc/profile
source /etc/profile
```

Dockerfile
```
ENV LANG C.UTF-8
```