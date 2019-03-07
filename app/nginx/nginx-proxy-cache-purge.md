    http ｛
        proxy_connect_timeout   5s;
        proxy_read_timeout      5s;
        proxy_send_timeout      5s;
        proxy_buffer_size       16k;
        proxy_buffers           4 64k;
        proxy_busy_buffers_size 128k;
        proxy_temp_file_write_size  128k;
    
        proxy_temp_path  /dev/shm/proxy_temp 1 2;
        proxy_cache_path /dev/shm/proxy_cache levels=1:2 keys_zone=amusic_cache:100m max_size=5g inactive=60m;
    
        proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_404;
        proxy_hide_header Vary;
        proxy_ignore_headers Set-Cookie Cache-Control Expires Vary;
        proxy_hide_header Set-Cookie; #按需添加
        proxy_cache cache_one;
        proxy_cache_key $scheme$request_method$host$uri$is_args$args;
        proxy_cache_min_uses 1;
        proxy_cache_methods GET HEAD;
        proxy_cache_lock on;
        proxy_cache_lock_timeout 2s;
        proxy_cache_use_stale error timeout invalid_header updating; 
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    
        open_file_cache max=102400 inactive=30s;
        open_file_cache_valid 30s;
        open_file_cache_min_uses 1;
        open_file_cache_errors on;
        underscores_in_headers on;
        
    #多IP出口负载，解决65535端口耗尽问题
        split_clients "$remote_addr$remote_port" $split_ip {
            30%  192.168.1.201;
            30%  192.168.1.202;
            30%  192.168.1.203;
            * 192.168.1.200;
        }
        
    upstream web.cluster {
        server web1.sundayle.com:80 weight=2;
        server web1.sundayle.com:80 weight=3;
        check interval=1000 rise=1 fall=2 timeout=1000;
        #ip_hash;
        keepalive 20;
    }
    
        location ~ /purge(/.*) {
            allow 127.0.0.1;
            allow 192.168.1.0/24;
            deny all;
            proxy_cache_purge cache_one $scheme$request_method$host$1$is_args$args;
        }
    
        location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|ico|js|css)?$ {
            proxy_bind $split_ip;
            proxy_pass http://sundayle.web.cluster;
            proxy_set_header Host www.sundayle.com;
            proxy_http_version 1.1;
            proxy_set_header Connection "";
            proxy_cache_valid  200 304  10d;
    		expires 10d;
    		add_header X-Cache '$upstream_cache_status from $host 10d';
    } 


清除缓存

```
curl -XGET https://www.sundayle.com/static/scripts/vendor.ec9d5a2b99344dc4af5c.js
```

注意：

proxy_ignore_headers Set-Cookie Cache-Control Expires Vary;
如不添加proxy_ignore_headers vary;则会出现不同客户端或浏览器Accept-Encoding不同，尤其是经过lua处理过缓存url。

Google Chrome f12 查看 Request Headers  
有时或有点客户端Accept-Encoding如下多种不同性。  
Accept-Encoding: gzip, deflate, br  
Accept-Encoding: gzip, deflate  

添加proxy_ignore_headers Vary; #proxy代理缓存时忽略vary,解决不同客户端缓存不同key问题，同时解决ngx_purge_cache无法清除某些缓存问题（如lua处理过），或不同客户端清除缓存需加Accept-Encoding。
```
curl -XGET https://www.sundayle.com/purge/static/scripts/app.14c2e51bf4537456e501.js -H 'Accept-Encoding: gzip,deflate,br'

curl -XGET 'https://www.sundayle.com/purge/user/info?type=4&uid=532777' -H ' 'Accept-Encoding: gzip,deflate' #lua处理过
```


http://www.ttlsa.com/nginx/dynamic-php-nginx-cache/