# openresty 安装
```
 ./configure --prefix=/usr/local/webserver/openresty \
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
 --with-compat \
 --error-log-path=/data/logs/nginx/error.log \
 --http-log-path=/data/logs/nginx/access.log \
 --add-module=/usr/local/src/nginx_module/ngx_cache_purge-2.3 \
 --add-module=/usr/local/src/nginx_module/nginx-upload-module-2.2 \
 --add-module=/usr/local/src/nginx_module/nginx_upstream_check_module-master 
```

```
/usr/local/src/openresty-1.13.6.2/bundle/nginx-1.13.6
patch -p1 < /usr/local/src/nginx_module/nginx_upstream_check_module-master/check_1.12.1+.patch


gmake -j8 && gmake install



sed -i -e 's#1.13.6##g' -e 's#openresty/#sunday#g' -e 's#"NGINX"#"Sunday"#g' src/core/nginx.h
sed -i 's#>openresty<#>sunday<#g' src/http/ngx_http_special_response.c
sed -i 's#Server: openresty#Server: sunday#g' src/http/ngx_http_header_filter_module.c
```