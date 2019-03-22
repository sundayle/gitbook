```
FROM registry.xwx.cn:5000/xwx/openresty:1.0
ENV LANG C.UTF-8

RUN apt-get update \
 && apt-get install -y curl \
 && cd /usr/local/openresty/lualib/ \
 && curl -fSL -o lua-resty-jwt.tar.gz https://github.com/SkyLothar/lua-resty-jwt/releases/download/v0.1.11/lua-resty-jwt-0.1.11.tar.gz \
 && mkdir lua-resty-jwt \
 && tar xzf lua-resty-jwt.tar.gz --strip-components 1 -C lua-resty-jwt \
 && rm -rf  lua-resty-jwt.tar.gz \
 && cd /usr/local/openresty/lualib/lua-resty-jwt/lib/resty/ \
 && curl -fSL -o hmac.lua https://raw.githubusercontent.com/jkeys089/lua-resty-hmac/master/lib/resty/hmac.lua \
 && apt-get clean \
 && rm -rf /var/lib/apt/* /
```

