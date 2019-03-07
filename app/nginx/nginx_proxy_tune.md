## upstram
```
upstream backend{
	hash $uri #哈希，添加、删除服务器，key 负载变动
	hash $consistent_key consistent; #一致哈希，增加、删除服务器 key少数负载
	
    check interval=3000 rise=1 fall=3 timeout=2000 type=http;
    check_http_send "HEAD /status HTTP/1.0\r\n\r\n";
    check_http_expect_alived httpd_2xx http_3xx;
    #keealive 100; #不限制与上游总连接
}
```
## 长连接
```  
    #开启长连接
    #proxy_htt_version 1.1;
    #proxy_set_header Connetion "";
```
## 全局配置
```
proxy_cache
proxy_buffering on;
proxy_buffer_size 4k;
proxy_busy_buffers_size 64k;
proxy_temp_file_write_size 256k;
proxy_cache_lock on;
proxy_cache_lock_timeout 200ms;
proxy_temp_path /tmpfs/proxy_temp;
proxy_cache_path /tmpfs/proxy_cache levels=1:2 keys_zone=cache:512m inactive=5m max_size=8g;

proxy_connect_timeout 3s;
proxy_read_timeout 3s;
proxy_send_timeout 5;
```
## location
```
location ~ ^/backend/(.*)$ {
	#设置一致性哈希负载均衡key
    set_by_lua_file $consistent_key "/export/App/c.3.cn/lua/lua_balancing_backend.properties";
    #失败重试
    proxy_next_upstream error timeout http_500 http_502 http_504;;
    proxy_next_upstream_timout 2s;
    proxy_next_upstream_tries 2;
    
    #请求上游服务器Get方法
    proxy_method GET;
    #不给上游服务器传递请求体
    proxy_pass_request_body off;
    #不给上游服务器传递请求头
    proxy_pass_request_headers off;
    #设置上游服务器的哪些响应头不发送给客户端
    proxy_hide_heaer Vary;
    #支持keep-alive
    proxy_http_version 1.1;
    proxy_set_header Connection "";
    #给上游服务器传递Referer、Cookies和Host（按需传递）
    proxy_set_header Referer $http_referer;
    proxy_set_header Cookies $http_cookies;
    proxy_set_header Host web.c.3.local;
    proxy_pass http://backend/$1$is_args$args;
  }
```
 proxy_pass_request_body、proxy_pass_request_headers禁止向上游传递请求头和内容体，从而使上游服务器不爱请求头攻击，也不需要解析，如果需要传递，则使用proxy_set_header按需传递即可

## gzip
  ```
  gzip on;
  gzip_min_leagth 1k;
  gzip_buffers 16 16k;
  gzip_http_version 1.0;
  gzip_proxied any;
  gzip_comp_devel 4;
  gzip_types text/plain text/xml text/css text/javascript application/javascript application/x-javascript application/json; 
  gzip_vary on;
  ```
日志格式
```
    log_format main '{"@timestamp":"$time_iso8601",'
    '"host": "$server_addr",'
    '"clientip": "$remote_addr",'
    '"size": $body_bytes_sent,'
    '"responsetime": $request_time,'
    '"upstreamtime": "$upstream_response_time",'
    '"upstreamhost": "$upstream_addr",'
    '"http_host": "$host",'
    '"url": "$scheme://$server_name$request_uri",'
    '"xff": "$http_x_forwarded_for",'
    '"referer": "$http_referer",'
    '"agent": "$http_user_agent",'
    '"status": "$status"}';
```
```
    if ($request_uri ~ ^/(login|register|password\/reset)){
        set $cookie_nocache 1;
    }
        location / {
        proxy_pass http://backend;
        
        proxy_cache sam_cache;                  #使用前面定义的proxy_cache_path
        proxy_cache_valid 200 304 12h;          #设置200和304过期时间为12h
        proxy_cache_valid any 10m;              #其他过期时间为10分钟
        proxy_cache_key $host$uri$is_args$args; #修改缓存纬度,缓存的key
        add_header Nginx-Cache "$upstream_cache_status";
        
        #配置不缓存的，如果上面条件判断中有新增变量$cookie_nocache，那么对应请求不进行缓存
        proxy_no_cache $cookie_nocache;
        
        #当命中的服务器出现错误、超时、请求头不完整、500、502、503时，会跳过这一台服务器去访问下一台服务器。
        proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
        
        ###### 以下是代理常规配置 ######
        proxy_redirect default; #一般配置默认即可
        
        #添加头信息
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        
        #配置超时
        proxy_connect_timeout 30;
        proxy_send_timeout 60;
        proxy_read_timeout 60;
        
        #配置缓冲区，
        proxy_buffer_size 32k;
        proxy_buffering on;
        proxy_buffers 4 128k;
        proxy_busy_buffers_size 256k;
        proxy_max_temp_file_size 256k;
    }
ss
```

```

```

curl https://mall.open.amusic.shop/purge/static/scripts/app.14c2e51bf4537456e501.js -H 'Accept-Encoding: gzip,deflate,br'

curl -XGET 'https://api.open.amusic.shop/purge/user/info?type=4&uid=532777' -H ' 'Accept-Encoding: gzip,deflate'
