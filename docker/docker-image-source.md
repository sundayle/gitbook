# 镜像国内源加速
## Debian stretch
#ENV APT_REPO='mirrors.aliyun.com'
ENV APT_REPO='mirrors.163.com'


```
RUN set -x \
    && sed -ri 's/(deb|security).debian.org/'$APT_REPO'/g' /etc/apt/sources.list \
```