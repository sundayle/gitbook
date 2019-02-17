# Nginx 编译安装
Ubuntu
```
apt-get -y install gcc g++ make libtool openssl libssl-dev libpcre3 libpcre3-dev libgd-dev
```
CentOS
```
yum install gcc make openssl-devel pcre-devel gd-devel
```
下载与安装
```
NGINX_VERSION=1.14.2
INSTALL_PATH=/usr/local/webserver/nginx
NGINX_MODULE_PATH=/usr/local/src/nginx_module

cd ￥NGINX_MODULE_PATH \
    && wget  http://package.sundayle.com/nginx/nginx_module/echo-nginx-module-master.zip
    && wget  http://package.sundayle.com/nginx/nginx_module/headers-more-nginx-module-master.zip
    && wget  http://package.sundayle.com/nginx/nginx_module/nginx_upstream_check_module-master.zip
    && wget  http://package.sundayle.com/nginx/nginx_module/nginx-upload-module-2.2.zip 
    && wget  http://package.sundayle.com/nginx/nginx_module/ngx_cache_purge-2.3.tar.gz

cd /usr/local/src \
    && tar xf "nginx-{{ NGINX_VERSION }}.tar.gz" \
    && unzip -o echo-nginx-module-master.zip \
    && unzip -o headers-more-nginx-module-master.zip \
    && unzip -o nginx_upstream_check_module-master.zip \
    && unzip -o nginx-upload-module-2.2.zip \
    && tar xf ngx_cache_purge-2.3.tar.gz
```
编译参数
```
./configure \
--prefix=$INSTALL_PATH \
--with-file-aio
--with-poll_module
--with-http_realip_module
--with-http_image_filter_module
--with-http_gunzip_module
--with-http_gzip_static_module
--with-http_addition_module
--with-http_sub_module
--with-http_dav_module
--with-http_flv_module
--with-http_mp4_module
--with-http_slice_module
--with-http_random_index_module
--with-http_secure_link_module
--with-http_degradation_module
--with-http_v2_module
--with-http_ssl_module
--with-http_stub_status_module
--with-stream
--with-stream_ssl_module
--with-stream_ssl_preread_module
--with-stream_realip_module
--with-threads
--with-pcre
--error-log-path=/data/logs/nginx/error.log
--http-log-path=/data/logs/nginx/access.log
--with-ld-opt=$JEMALLOC_PATH \
--add-module=$NGINX_MODULE_PATH/ngx_cache_purge-2.3 \
--add-module=$NGINX_MODULE_PATH/ngx_slowfs_cache-1.10 \
--add-module=$NGINX_MODULE_PATH/nginx-rtmp-module-master \
--add-module=$NGINX_MODULE_PATH/nginx_upstream_check_module-master \
--add-module=$NGINX_MODULE_PATH/echo-nginx-module-master \
--add-dynamic-module=$NGINX_MODULE_PATH/nginx-upload-module \
```