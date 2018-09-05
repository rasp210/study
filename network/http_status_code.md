# HTTP Status Code

HTTP 状态码是用以表示网页服务器 HTTP 响应状态的 3 位数字代码。

## 1.消息

## 2.成功

## 3.重定向
### 301 Moved Permanently
- 会提示更新收藏地址   
- 

### 302 Move temporarily 临时重定向   
- 先请求原地址，然后重定向到新的地址，不会提示更新收藏地址   
- 【使用】   
- header("Location: http://xxx.com");   

## 4.请求错误
### 400 Bad Request   
- 【原因】   
- 1. 语义有误，当前请求无法被服务器理解   
- 2. 请求参数有误

### 401 Unauthorized 未授权   
- 当前请求需要用户验证

### 402 Payment Required 预留

### 403 Forbidden 禁止   
- 服务器已经理解请求，但是拒绝执行它。   

### 404 Not Found 未找到    
- 未找到

### 405 Method Not Allowed   
- 请求行中指定的请求方法不被允许
- 鉴于 PUT，DELETE 方法会对服务器上的资源进行写操作，因而绝大部分的网页服务器都不支持或者在默认配置下不允许上述请求方法，对于此类请求均会返回405错误   

### 406 Not Acceptable 无法接受   
- 客户端向web服务器表明他所能接受的返回数据的类型，包括：      
- 1. MIME（多功能互联网邮件扩充服务）：Content-Type   
- 2. 字符集：Content-Type   
- 3. 语言：Accept-Language   
- 4. 编码：Accept-Encoding   
- 5. 范围：客户端是否接受来自网络资源的字节范围，即该资源的一部分   
- 
- 【原因】   
- 如果web服务器检测到它将返回给客户端的数据不能被客户端接受，web服务器则返回一个含有406错误代码的报头。
- 
- 【解决】
- 一般不会出现406错误。如果出现了，可以通过分析客户端的接受投和web服务器的返回数据来定位问题。   

### 495 https certificate error --NginxError--   
### 496 https no certificate --NginxError--   
### 497 http to https --NginxError--
### 498 cancled --NginxError--   
### 499 client has closed connection --NginxError--
- 客户端主动断开了连接   
- 
- 【原因】   
- 1. 服务器处理时间太长，客户端等不了了   
- 2. 服务器受到攻击导致服务器资源大量消耗   
- 
- 【解决】
- 
- 【错误示例】  
- 61.135.249.220 – - [02/Oct/2009:10:28:21 +0000] “GET /subject/93390/ HTTP/1.1″ 499 0 “-” “Mozilla/5.0 (compatible; YoudaoBot/1.0; http://www.youdao.com/help/webmaster/spider/; )”
- 
- 61.135.249.216 – - [02/Oct/2009:10:30:08 +0000] “GET /subject/476083/ HTTP/1.1″ 499 0 “-” “Mozilla/5.0 (compatible; YoudaoBot/1.0; http://www.youdao.com/help/webmaster/spider/; )”
-

## 5.服务器错误   
### 502 Bad Gateway   
- 
- 【原因】
- 1. 存在特别消耗资源的代码，如导出excel，死循环等，可以通过查看php-fpm中配置的slow-log路径指向的文件来定位耗时较长的脚本   
- 2. php.ini中的max_execution_time设置太小，一般是30s。同时需要修改php-fpm中的request_terminate_timeout也为30
- 3. php-fpm配置中listen的sock路径错误   
- 4. php-fpm配置中max_children或max_requests的值设置太小
- 5. nginx配置中fastcgi_connect_timeout、fastcgi_read_timeout、fastcgi_send_timeout的值设置太小
- 6. php-cgi或php-fpm进程没有执行

- 【解决】
- 
- 【错误示例】   
- 

### 504 Gateway Timeout 网关超时
- 
- 【原因】
- 1. 服务器出问题
- 【解决】   
- 1. 重新加载页面   
- 2. 重启所有的网络设备   

