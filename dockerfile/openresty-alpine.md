```
# Dockerfile - alpine
# https://github.com/openresty/docker-openresty

ARG RESTY_IMAGE_BASE="alpine"
ARG RESTY_IMAGE_TAG="3.8"

FROM ${RESTY_IMAGE_BASE}:${RESTY_IMAGE_TAG}

LABEL maintainer="shaopenghk@qq.com"

ARG RESTY_VERSION="1.13.6.2"
ARG NGINX_VERSION="1.13.6"
ARG NGINX_CUSTOM="EnjoyMusic"
ARG USER=www
ARG GROUP=www
ARG RESTY_CONFIG_OPTIONS="\
    --with-file-aio \
    --with-poll_module \
    --with-http_addition_module \
    --with-http_auth_request_module \
    --with-http_dav_module \
    --with-http_flv_module \
    --with-http_geoip_module=dynamic \
    --with-http_gunzip_module \
    --with-http_gzip_static_module \
    --with-http_image_filter_module=dynamic \
    --with-http_mp4_module \
    --with-http_random_index_module \
    --with-http_realip_module \
    --with-http_secure_link_module \
    --with-http_degradation_module \
    --with-http_slice_module \
    --with-http_auth_request_module \
    --with-http_ssl_module \
    --with-http_stub_status_module \
    --with-http_sub_module \
    --with-http_v2_module \
    --with-http_xslt_module=dynamic \
    --with-compat \
    --with-md5-asm \
    --with-pcre-jit \
    --with-sha1-asm \
    --with-stream \
    --with-stream_ssl_module \
    --with-stream_ssl_preread_module \
    --with-threads \
    --error-log-path=/var/log/nginx/error.log \
    --http-log-path=/var/log/nginx/access.log \
    --add-module=/tmp/ngx_cache_purge \
    --add-module=/tmp/nginx_upstream_check \
    --add-module=/tmp/nginx-sticky-module-ng"

# 1) Install apk dependencies
# 2) Download and untar OpenSSL, PCRE, and OpenResty
# 3) Build OpenResty
# 4) Cleanup

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
    && apk add --no-cache --virtual .build-deps \
        build-base \
        curl \
        gd-dev \
        geoip-dev \
        libxslt-dev \
        linux-headers \
        make \
        perl-dev \
        readline-dev \
        zlib-dev \
        unzip \
        tzdata \
    && apk add --no-cache \
        gd \
        geoip \
        libgcc \
        libxslt \
        pcre-dev \
        openssl-dev \
        zlib \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone \
    && adduser -D -s /bin/false -u 1001 www \
    && cd /tmp \
    && curl -fSL -o nginx_upstream_check.zip http://package.sundayle.com/nginx/openresty_module/nginx_upstream_check_module.zip \
    && unzip -j -o -d nginx_upstream_check nginx_upstream_check.zip \
    && curl -fSL -o nginx-sticky-module-ng.zip http://package.sundayle.com/nginx/openresty_module/nginx-sticky-module-ng.zip \
    && unzip -j -o -d nginx-sticky-module-ng nginx-sticky-module-ng.zip \
    && curl -fSL -o ngx_cache_purge.zip http://package.sundayle.com/nginx/openresty_module/ngx_cache_purge.zip \
    && unzip -j -o -d ngx_cache_purge ngx_cache_purge.zip \
    && curl -fSL http://package.sundayle.com/nginx/openresty-${RESTY_VERSION}.tar.gz -o openresty-${RESTY_VERSION}.tar.gz \
    && tar xzf openresty-${RESTY_VERSION}.tar.gz \
    && cd /tmp/openresty-${RESTY_VERSION} \
    && sed -i -e 's#'${NGINX_VERSION}'##g' -e 's#openresty/#'${NGINX_CUSTOM}'#g' -e 's#"NGINX"#"'${NGINX_CUSTOM}'"#g' -e 's#".2"##g' bundle/nginx-${NGINX_VERSION}/src/core/nginx.h \
    && sed -i 's#>openresty<#>'${NIGNX_CUSTOM}'<#g' bundle/nginx-${NGINX_VERSION}/src/http/ngx_http_special_response.c \
    && sed -i 's#Server: openresty#Server: '${NIGNX_CUSTOM}'#g' bundle/nginx-${NGINX_VERSION}/src/http/ngx_http_header_filter_module.c \
    && ./configure -j$(getconf _NPROCESSORS_ONLN) ${RESTY_CONFIG_OPTIONS} \
    && make -j$(getconf _NPROCESSORS_ONLN) \
    && make -j$(getconf _NPROCESSORS_ONLN) install \
    && cd /tmp \
    && rm -rf /tmp/* \
    && apk del .build-deps \
    && ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log \
    && ln -sv /usr/local/openresty/nginx/conf /etc/nginx \
    && mkdir /etc/nginx/conf.d

# Add additional binaries into PATH for convenience
ENV PATH=$PATH:/usr/local/openresty/luajit/bin:/usr/local/openresty/nginx/sbin:/usr/local/openresty/bin

# Copy nginx configuration files
COPY nginx.conf /usr/local/openresty/nginx/conf/nginx.conf
COPY conf.d/default.conf /etc/nginx/conf.d/default.conf

CMD ["/usr/local/openresty/bin/openresty", "-g", "daemon off;"]

# Use SIGQUIT instead of default SIGTERM to cleanly drain requests
# See https://github.com/openresty/docker-openresty/blob/master/README.md#tips--pitfalls
STOPSIGNAL SIGQUIT
```

