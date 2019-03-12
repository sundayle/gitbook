```
FROM php:7.1.26-fpm-stretch

LABEL maintainer="shaopenghk@qq.com"

ENV APT_REPO='mirrors.163.com'

### mysqli
RUN set -x \
    && sed -ri 's/(deb|security).debian.org/'${APT_REPO}'/g' /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y \
          --no-install-recommends \
          --no-install-suggests \
          libmcrypt-dev \
          libpng-dev \
          libxml2-dev \
          tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone \
    && docker-php-ext-install \
           mysqli \
           soap \
           sockets \
           exif \
           gd \
           gettext \
           mcrypt \
           pcntl \
           shmop \
           sysvsem \
           xmlrpc 

### imagick
RUN set -x \
    && apt-get install -y \
           --no-install-recommends \
           --no-install-suggests -y \
           libmcrypt-dev \
           libmagickwand-dev \
    && export MAGICKWAND_VER=$(dpkg -s libmagickwand-dev | grep Version|awk -F : '{print $3}'|cut -c1-5)\
    && ln -sv /usr/lib/x86_64-linux-gnu/ImageMagick-$MAGICKWAND_VER/bin-q16/MagickWand-config /usr/bin \
    && pecl install imagick \
    && docker-php-ext-enable imagick 

### zbarcode
RUN set -x \
    && apt-get install -y \
         --no-install-recommends \
         --no-install-suggests -y \
         wget \
         unzip \
         zbar-tools \
         libzbar-dev \
    #&& export MAGICKWAND_VER=$(dpkg -s libmagickwand-dev | grep Version|awk -F : '{print $3}'|cut -c1-5)\
    #&& ln -sv /usr/lib/x86_64-linux-gnu/ImageMagick-$MAGICKWAND_VER/bin-q16/MagickWand-config /usr/bin \
    #&& ln -sv /usr/lib/x86_64-linux-gnu/ImageMagick-6.9.7/bin-q16/MagickWand-config /usr/bin \
    && mkdir -p /usr/src/php/ext \
    && wget -O /usr/src/gophp7.zip https://github.com/jorissteyn/php-zbarcode/archive/gophp7.zip \
    && unzip /usr/src/gophp7.zip -d /usr/src/php/ext/ \
    && docker-php-ext-install php-zbarcode-gophp7

### redis
RUN set -x \
    && pecl install redis \
    && docker-php-ext-enable redis \
### amqp
    && apt-get install -y \
          --no-install-recommends \
          --no-install-suggests -y \
    && apt-get install -y \
         librabbitmq-dev \
         libssh-dev \
    && docker-php-ext-install \
         bcmath \
         sockets \
    && pecl install amqp \
    && docker-php-ext-enable amqp \
### yaf
    && pecl install yaf \ 
    && docker-php-ext-enable yaf \
### swoole
    && pecl install swoole \ 
    && docker-php-ext-enable swoole \
### clean
    && apt-get remove -y unzip wget tzdata \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /usr/src/* \
    && rm -rf /tmp/pear/*
​```s
```