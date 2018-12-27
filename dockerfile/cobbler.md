# Docker Cobbler
```
FROM centos:latest
ENV ALIYUN_REPO http://mirrors.aliyun.com/repo/Centos-7.repo
ENV ALIYUN_EPEL http://mirrors.aliyun.com/repo/epel-7.repo

RUN mv /etc/yum.repos.d/CentOS-Base.repo{,.backup} \
&& curl -o /etc/yum.repos.d/CentOS-Base.repo $ALIYUN_REPO \
&& curl -o /etc/yum.repos.d/epel.repo $ALIYUN_EPEL \
&& yum update \


```