## Dockerfile
```
#
# Dockerfile for shadowsocks-libev
#

FROM alpine:3.8
LABEL maintainer="kev <noreply@datageek.info>, Sah <contact@leesah.name>"

ENV SERVER_ADDR 0.0.0.0
ENV SERVER_ADDR_IPV6 ::0
ENV SERVER_PORT 8388
ENV PASSWORD=
ENV METHOD      aes-256-gcm
ENV TIMEOUT     300
ENV DNS_ADDRS    8.8.8.8,8.8.4.4
ENV ARGS=

RUN set -ex \
 && sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
 # Build environment setup
 && apk add --no-cache --virtual .build-deps \
      autoconf \
      automake \
      build-base \
      c-ares-dev \
      libev-dev \
      libtool \
      libsodium-dev \
      linux-headers \
      mbedtls-dev \
      pcre-dev \
      git \
 # Download 
 && mkdir -p /usr/src \
 && cd /usr/src \
 && git clone https://github.com/shadowsocks/shadowsocks-libev.git \
 && cd shadowsocks-libev \
 && git submodule update --init \
 # Disable auto block ip
 # or acl/local.acl add white_list
 && sed -i 's#// Avoid block local plugins#return;// Avoid block local plugins#' src/server.c \
 # Build & install
 && ./autogen.sh \
 && ./configure --prefix=/usr --disable-documentation \
 && make install \
 && apk del .build-deps \
 # Runtime dependencies setup
 && apk add --no-cache \
      rng-tools \
      $(scanelf --needed --nobanner /usr/bin/ss-* \
      | awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
      | sort -u) \
 && rm -rf /usr/src/*

USER nobody

CMD exec ss-server \
      -s $SERVER_ADDR \
      -s $SERVER_ADDR_IPV6 \
      -p $SERVER_PORT \
      -k ${PASSWORD:-$(hostname)} \
      -m $METHOD \
      -t $TIMEOUT \
      --fast-open \
      -d $DNS_ADDRS \
      -u \
      $ARGS
```

## docker run
```
docker run -d --name=ssr -p 24783:8388 -e PASSWORD='sunday' -e METHOD='chacha20-ietf-poly1305' sundayle/shadowsocksr
```

## docker-compose
docker-compose.yml
```
ss:
  image: sundayle/shadowsocks-libev
  ports:
    - "24771:8388/tcp"
    - "24772:8388/tcp"
    - "24775:8388/tcp"
    - "24776:8388/tcp"
#    - "8388:8388/udp"
  environment:
    - METHOD=chacha20-ietf-poly1305
    - PASSWORD=sunday
  restart: always  
```
```
docker-compose up -d
```