# LNMP

## LNMP 工作原理
LNMP，即Linux/Nginx/MySQL/PHP，其中，M也可以是MongoDB，P也可以是Python。
Nginx 是一个高性能的 web 服务器，本身是不能处理 PHP 脚本语言的。
- 浏览器发送发送请求到 web 服务器（Nginx）
- 服务器响应并处理请求，如果是静态文本则直接返回，否则将动态脚本（PHP 脚本）通过接口传输协议传输给 PHP-FPM
