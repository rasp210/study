# LNMP

## LNMP 工作原理
LNMP，即Linux/Nginx/MySQL/PHP，其中，M也可以是MongoDB，P也可以是Python。
Nginx 是一个高性能的 web 服务器，本身是不能处理 PHP 脚本语言的。
- 浏览器发送发送请求到 web 服务器（Nginx）
- 服务器响应并处理请求，如果是静态文本则直接返回，否则将动态脚本（PHP 脚本）通过接口传输协议传输给 PHP-FPM


## Nginx和php-fpm进程间通信   
**两种方式：**   
1. TCP。即 `IP:端口号` 
2. Unix Domain Socket。不经网络，只能处理Nginx和php-fpm都在一个服务器上的情况   

**方式一：**   
- php-fpm.conf
- listen = 127.0.0.1:9000   
- 
- nginx.conf   
- server {
- 	location ~ \.php {
- 		fastcgi_pass 127.0.0.1:9000;
- 	}
- }   

**方式二：**   
- php-fpm.conf   
- listen = /tmp/php-fpm.sock   
- 
- nginx.conf   
- server {
-	location ~ \.php {
-		fastcgi_pass unix:/tmp/php-fpm.sock;
-	}
- }

类似于使用mysql命令行连接mysqld服务：   
Unix Socket连接：   
mysql -uroot -p --protocol=socket --socket=/tmp/msyql.sock   
TCP连接：   
mysql -uroot -p --protocol=tcp --host=127.0.0.1 --port=3306   



