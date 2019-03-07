```
FROM php:7.1.26-fpm-stretch

LABEL maintainer="shaopenghk@qq.com"

#ENV APT_REPO='mirrors.163.com'
ENV APT_REPO='mirrors.aliyun.com'

RUN set -x \
	&& sed -ri 's/(deb|security).debian.org/'${APT_REPO}'/g' /etc/apt/sources.list \
	&& apt-get update \
	&& apt-get install -y \
		--no-install-recommends \
		--no-install-suggests \
		tzdata \
	&& cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone

# other
RUN apt-get install -y \
		--no-install-recommends \
		--no-install-suggests \
		libmcrypt-dev \
		libpng-dev \
		libxml2-dev \
	&& docker-php-ext-install \
		exif \
		gd \
		gettext \
		mcrypt \
		pcntl \
		xmlrpc \
		shmop \
		sysvsem \
		soap

# mysqli
RUN docker-php-ext-install -j$(nproc) mysqli 

# redis
RUN pecl install redis && docker-php-ext-enable redis 

### imagick
RUN apt-get install -y \
		--no-install-recommends \
		--no-install-suggests \
		libmcrypt-dev \
		libmagickwand-dev \
	&& export MAGICKWAND_VER=$(dpkg -s libmagickwand-dev | grep Version|awk -F : '{print $3}'|cut -c1-5)\
	&& ln -sv /usr/lib/x86_64-linux-gnu/ImageMagick-$MAGICKWAND_VER/bin-q16/MagickWand-config /usr/bin \
	#&& ln -sv /usr/lib/x86_64-linux-gnu/ImageMagick-6.9.7/bin-q16/MagickWand-config /usr/bin \
	&& pecl install imagick \
	&& docker-php-ext-enable imagick 

# zbarcode
RUN apt-get install -y \
		--no-install-recommends \
		--no-install-suggests \
		wget \
		unzip \
		zbar-tools \
		libzbar-dev \
	&& mkdir -p /usr/src/php/ext \
	&& wget -O /usr/src/gophp7.zip https://github.com/jorissteyn/php-zbarcode/archive/gophp7.zip \
	&& unzip /usr/src/gophp7.zip -d /usr/src/php/ext/ \
	&& docker-php-ext-install php-zbarcode-gophp7

# amqp
RUN apt-get install -y \
		--no-install-recommends \
		--no-install-suggests \
		librabbitmq-dev \
		libssh-dev \
	&& docker-php-ext-install \
		bcmath \
		sockets \
	&& pecl install amqp \
	&& docker-php-ext-enable amqp

# yaf 
RUN pecl install yaf \ 
	&& docker-php-ext-enable yaf

# swoole
RUN pecl install swoole \ 
	&& docker-php-ext-enable swoole 

# clean
RUN apt-get remove -y unzip wget tzdata \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/* \
 	&& rm -rf /usr/src/* \
	&& rm -rf /tmp/pear/*

# custom
RUN ln -sv /usr/local/etc/php /etc/php \
	&& ln -sv /usr/local/etc/php-fpm.conf /etc/php/ \
	&& ln -sv /usr/local/etc/php-fpm.d /etc/php \
	&& mkdir -p /var/log/php \
	&& useradd -s /bin/nologin -u 1001 www \
	&& chown www.www /var/log/php
```