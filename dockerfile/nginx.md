# Nginx Dockerfile

## Dockerfile
```
FROM debian:stretch-slim

LABEL maintainer="shaopenghk@qq.com"

ENV NGINX_VERSION 1.14.2 
ENV USER www
ENV GROUP www
RUN set -x \
  && sed -ri 's/(deb|security).debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list \
  && apt-get update \
  && apt-get install  --no-install-recommends --no-install-suggests -y \
     gnupg1 \
     gcc \
     make \
     libpcre3-dev \
     libssl-dev \
     libgd-dev \
     zlib1g-dev \
     libgeoip-dev\
     wget \
     curl \
     patch \
     tzdata \
     unzip \
     procps \
  && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
  && useradd --system -d /usr/local/nginx -s /sbin/nologin www \
  \
  && mkdir -p /usr/src \
  && cd /usr/src \
  && wget http://package.sundayle.com/nginx/nginx-$NGINX_VERSION.tar.gz \
  && wget http://package.sundayle.com/nginx/nginx_module/echo-nginx-module-master.zip \
  && wget http://package.sundayle.com/nginx/nginx_module/headers-more-nginx-module-master.zip \
  && wget http://package.sundayle.com/nginx/nginx_module/nginx_upstream_check_module-master.zip \
  && wget http://package.sundayle.com/nginx/nginx_module/nginx-upload-module-2.2.zip \
  && wget http://package.sundayle.com/nginx/nginx_module/ngx_cache_purge-2.3.tar.gz \
  \
  && unzip -o echo-nginx-module-master.zip \
  && unzip -o headers-more-nginx-module-master.zip \
  && unzip -o nginx_upstream_check_module-master.zip \
  && unzip -o nginx-upload-module-2.2.zip \
  && tar xf ngx_cache_purge-2.3.tar.gz \
  && tar xf nginx-$NGINX_VERSION.tar.gz \
  \
  && cd /usr/src/nginx-$NGINX_VERSION \
  && sed -i -e 's#'$NGINX_VERSION'##g' -e 's#nginx/#xws#g' -e 's#"NGINX"#"XWS"#g' src/core/nginx.h \
  && sed -i 's#nginx<#xws<#g' src/http/ngx_http_special_response.c \ 
  && sed -i 's#Server: nginx#Server: xws#g' src/http/ngx_http_header_filter_module.c \
  && patch -p1 < ../nginx_upstream_check_module-master/check_1.14.0+.patch \
  && ./configure \
		--sbin-path=/usr/sbin/nginx \
		--conf-path=/etc/nginx/nginx.conf \
		--modules-path=/usr/lib/nginx/modules \
		--error-log-path=/var/log/nginx/error.log \
		--http-log-path=/var/log/nginx/access.log \
		--pid-path=/var/run/nginx.pid \
		--lock-path=/var/run/nginx.lock \
		--user=$USER \
		--group=$GROUP \
		--with-file-aio \
		--with-poll_module \
		--with-http_realip_module \
		--with-http_image_filter_module \
		--with-http_gunzip_module \
		--with-http_gzip_static_module \
		--with-http_addition_module \
		--with-http_sub_module \
		--with-http_dav_module \
		--with-http_flv_module \
		--with-http_mp4_module \
		--with-http_slice_module \
		--with-http_random_index_module \
		--with-http_secure_link_module \
		--with-http_degradation_module \
		--with-http_ssl_module \
		--with-http_v2_module \
		--with-http_stub_status_module \
		--with-http_slice_module \
		--with-http_geoip_module \
		--with-http_auth_request_module \
		--with-stream \
		--with-stream_ssl_module \
		--with-stream_ssl_preread_module \
		--with-stream_realip_module \
		--with-threads \
		--with-pcre \
		--add-module=./../ngx_cache_purge-2.3 \
		--add-module=./../nginx_upstream_check_module-master \
		--add-module=./../echo-nginx-module-master \
		--add-module=./../headers-more-nginx-module-master \
		--add-module=./../nginx-upload-module-2.2 \
	&& make -j$(getconf _NPROCESSORS_ONLN) \
	&& make install \
	&& strip /usr/sbin/nginx* \
	&& mkdir /etc/nginx/conf.d/ \
	&& touch /usr/local/nginx/html/favicon.ico \
    && apt-get remove --purge --auto-remove -y \
     gnupg1 apt-transport-https ca-certificates \
     curl wget gcc make patch tzdata unzip \
  && apt-get clean \
	&& rm -rf /usr/src/* \
  && rm -rf /var/lib/apt/lists/* \
	&& ln -sf /dev/stdout /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log

COPY nginx.conf /etc/nginx/nginx.conf
COPY default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
STOPSIGNAL SIGTERM
CMD ["nginx", "-g", "daemon off;"]
```
## nginx.conf
```
user  www;
worker_processes  8;  

error_log  /var/log/nginx/error.log	error;
pid        /var/run/nginx.pid;

worker_rlimit_nofile 65535;

events {
    worker_connections  10240;
}

http {
    limit_conn_zone $binary_remote_addr zone=perip:10m;
    limit_conn_zone $server_name zone=perserver:10m;
    include       mime.types;
    include       conf.d/*.conf;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log main;

    server_tokens off;
    sendfile        on;
    tcp_nopush      on;
    tcp_nodelay     on;
    keepalive_timeout  30;

    server_names_hash_bucket_size 256;
    client_header_buffer_size 32k;
    large_client_header_buffers 4 32k;
    client_max_body_size 250m;
	  client_body_temp_path client_body_temp 1 2;

    gzip on;
    gzip_min_length  1k;
    gzip_buffers     4 16k;
    gzip_http_version 1.1;
    gzip_comp_level 3;
    gzip_types text/plain application/javascript text/css application/xml application/x-javascript;
    gzip_vary on;


    fastcgi_connect_timeout 300;
    fastcgi_send_timeout    300;
    fastcgi_read_timeout 300;
    fastcgi_buffer_size 128k;
    fastcgi_buffers 4 128k;
    fastcgi_busy_buffers_size   128k;
    fastcgi_temp_file_write_size 128k;
    fastcgi_intercept_errors on;

    fastcgi_temp_path fastcgi_temp;
    fastcgi_cache_path fastcgi_cache levels=1:2 keys_zone=fastcgi_cache_one:100m inactive=1d max_size=10g;

    proxy_redirect off;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 

    proxy_connect_timeout   90;
    proxy_read_timeout      90;
    proxy_send_timeout      90;
    proxy_buffer_size       16k;
    proxy_buffers           4 64k;
    proxy_busy_buffers_size 128k;
    proxy_temp_file_write_size  128k;
    proxy_headers_hash_max_size 51200;
    proxy_headers_hash_bucket_size 6400;

    proxy_temp_path     proxy_temp 1 2;
    proxy_cache_path    proxy_cache   levels=1:2 keys_zone=proxy_cache_one:100m inactive=1d max_size=10g;

    open_file_cache max=204800 inactive=20s;
    open_file_cache_valid 30s;
    open_file_cache_min_uses 1;
    open_file_cache_errors on;
}
```
## default.conf
```
cat default.conf 
server {
    listen 80;
    server_name _;
    root html;
}
```

