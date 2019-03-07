# Nginx获取自定义头部header的值
1、nginx是支持读取非nginx标准的用户自定义header的，但是需要在http或者server下开启header的下划线支持: underscores_in_headers on;

2、比如我们自定义header为X-Real-IP,通过第二个nginx获取该header时需要这样:

$http_x_real_ip; (一律采用小写，而且前面多了个http_)

3、如果需要把自定义header传递到下一个nginx：

如果是在nginx中自定义采用proxy_set_header X_CUSTOM_HEADER $http_host;

如果是在用户请求时自定义的header，例如curl –head -H “X_CUSTOM_HEADER: foo” http://domain.com/api/test，则需要通过proxy_pass_header X_CUSTOM_HEADER来传递
```
http{
   upstream myServer {   
       server 127.0.0.1;
   }
   
   underscores_in_headers on;
   
   server {

       listen       80;
       server_name  localhost;
       location  / {
 	    proxy_set_header Some-Thing $http_x_custom_header;
 	    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
 	    proxy_pass http://myServer;
		add_header Some-Thing $http_x_custom_header;
       }
   }
```
https://yq.aliyun.com/articles/514726


