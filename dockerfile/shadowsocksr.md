## Dockerfile
```
FROM alpine:3.6

ENV SERVER_ADDR     0.0.0.0
ENV SERVER_PORT     8388
ENV PASSWORD        sunday
ENV METHOD          chacha20-ietf
ENV PROTOCOL        auth_aes128_md5
ENV PROTOCOLPARAM   32
ENV OBFS            tls1.2_ticket_auth
ENV TIMEOUT         300
ENV DNS_ADDR        8.8.8.8
ENV DNS_ADDR_2      8.8.4.4

ARG BRANCH=manyuser
ARG WORK=~


RUN apk --no-cache add python \
    libsodium \
    wget


RUN mkdir -p $WORK && \
    wget -qO- --no-check-certificate https://github.com/sundayle/shadowsocksr/archive/$BRANCH.tar.gz | tar -xzf - -C $WORK


WORKDIR $WORK/shadowsocksr-$BRANCH/shadowsocks


EXPOSE $SERVER_PORT
CMD python server.py -p $SERVER_PORT -k $PASSWORD -m $METHOD -O $PROTOCOL -o $OBFS -G $PROTOCOLPARAM
```

## docker run
```
docker run -d --name=ssr -p 24772:8388 -e PASSWORD='sunday' -e METHOD='chacha20-ietf-poly1305' sundayle/shadowsocksr
```

## docker-compose
docker-compose.yml
```
shadowsocksr:
  image: sundayle/shadowsocks
  ports:
    - "24772:8388/tcp"
#    - "8388:8388/udp"
  environment:
    - METHOD=chacha20-ietf
    - PASSWORD=sunday
  restart: always 
```
```
docker-compose up -d
```