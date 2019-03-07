```
FROM debian:stretch-slim

LABEL maintainer="shaopenghk@qq.com"

# Docker Build Arguments
ARG RESTY_VERSION="1.13.6.2"
ARG NGINX_VERSION="1.13.6"
ARG RESTY_LUAROCKS_VERSION="2.4.4"
ARG RESTY_J="38"
#ARG RESTY_J="$(getconf _NPROCESSORS_ON)"
ARG NGINX_CUSTOM="EnjoyMusic"
ENV USER www
ENV GROUP www
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
    --add-module=/tmp/ngx_cache_purge \
    --add-module=/tmp/nginx_upstream_check \
    --add-module=/tmp/nginx-sticky-module-ng"

RUN set -x \
  && sed -ri 's/http:\/\/(deb|security).debian.org/http:\/\/mirrors.163.com/g' /etc/apt/sources.list \
  && apt-get update \
  && apt-get install  \
        --no-install-recommends \
        --no-install-suggests -y \
        build-essential \
        ca-certificates \
        curl \
        gettext-base \
        libgd-dev \
        libgeoip-dev \
        libncurses5-dev \
        libperl-dev \
        libreadline-dev \
        libxslt1-dev \
        make \
        perl \
        unzip \
        zlib1g-dev \
        libpcre3-dev \
        libssl-dev \
        tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone \
    && useradd -u 1001 -s /sbin/nologin www \
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
    && ./configure -j${RESTY_J} ${RESTY_CONFIG_OPTIONS} \
    && make -j${RESTY_J} \
    && make -j${RESTY_J} install \
    && cd /tmp \
    && rm -rf /tmp/* \
    && apt-get remove --purge --auto-remove -y \
         gnupg1 apt-transport-https ca-certificates \
         curl wget gcc make patch tzdata unzip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && ln -sv /usr/local/openresty/nginx/conf /etc/nginx \
    && mkdir -p /etc/nginx/conf.d \
    && mkdir -p /var/log/nginx 
    
# Add additional binaries into PATH for convenience
ENV PATH=$PATH:/usr/local/openresty/luajit/bin:/usr/local/openresty/nginx/sbin:/usr/local/openresty/bin

# Copy nginx configuration files
COPY nginx.conf /etc/nginx/nginx.conf
COPY default.conf /etc/nginx/conf.d/default.conf

CMD ["/usr/local/openresty/bin/openresty", "-g", "daemon off;"]

STOPSIGNAL SIGQUIT

```