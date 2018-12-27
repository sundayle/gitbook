# Nginx 编译安装
Ubuntu
```
apt-get -y install gcc g++ make libtool openssl libssl-dev libpcre3 libpcre3-dev libgd-dev
```
CentOS
```
yum install gcc make openssl-devel pcre-devel gd-devel
```
编译参数
```
INSTALL_PATH=/usr/local/webserver/nginx
NGINX_MODULE_PATH=/usr/local/src/nginx_module
jem

./configure --prefix=$INSTALL_PATH \
--with-file-aio \
--with-poll_module \
--with-http_realip_module \
--with-http_image_filter_module \
--with-http_gzip_static_module \
--with-http_addition_module \
--with-http_sub_module \
--with-http_dav_module \
--with-http_flv_module \
--with-http_slice_module \
--with-http_mp4_module \
--with-http_random_index_module \
--with-http_secure_link_module \
--with-http_degradation_module \
--with-http_ssl_module \
--with-http_stub_status_module \
--with-http_v2_module \
--with-pcre \
--with-ld-opt=/usr/local/webserver/jemalloc/lib/libjemalloc.so.2 \
--add-module=$NGINX_MODULE_PATH/ngx_cache_purge-2.3 \
--add-module=$NGINX_MODULE_PATH/ngx_slowfs_cache-1.10 \
--add-module=$NGINX_MODULE_PATH/nginx-rtmp-module-master \
--add-module=$NGINX_MODULE_PATH/nginx_upstream_check_module-master \
--add-module=$NGINX_MODULE_PATH/echo-nginx-module-master \
--add-dynamic-module=$NGINX_MODULE_PATH/nginx-upload-module \
--error-log-path=/data/logs/nginx/error.log 
```