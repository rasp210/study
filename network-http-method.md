# HTTP Method   
> HTTP 是应用层的无状态协议。   

**http method版本历史**   
> HTTP/0.9：支持GET方法   
> HTTP/1.0：支持GET、HEAD、POST方法   
> HTTP/1.1：支持GET、HEAD、POST、PUT、DELETE、CONNECT、OPTIONS、TRACE、PATCH方法   

- Content-Type: text/html;charset=utf-8 *Note：大小写不敏感*   
> Content-Type = media-type     
> media-type = type . "/" . subtype . ";" . parameter
> type = token   
> subtype = token   
> parameter = token . "=" . token *Note: =号左右不能有空格；第二个token可以加双引号；*     
```
	text/html;charset=utf-8
    text/html;charset=UTF-8
    Text/HTML;Charset="utf-8"
    text/html; charset="utf-8"
```

- Content-Encoding: gzip      
> Content-Encoding: value may be anyone of compress | x-compress | deflate | gzip | x-gzip   

**http method简介**   
服务器至少支持其中的GET和HEAD，其余的方法都是可选的：POST/PUT/DELETE/CONNECT/OPTIONS/TRACE/PATCH      
- GET   
> 获取数据      

- HEAD   
> 同get，但是没有响应体   

- POST   
> 创建或修改资源   
 
- PUT   
> 没有则创建，有则更新。   

- DELETE   
> 请求服务器删除Request-URI所标识的资源

- CONNECT   
> 方法名是区分大小写的。   
> 405(Method Not Allowed)   
> 501(Not Implemented)   

- OPTIONS   
> 

- TRACE   
> 回显服务器接收得到的请求内容，主要用来测试和诊断   

- PATCH   
> 局部修改应用到资源   

**安全方法**   
只读方法被认为是安全方法。如GET、HEAD、OPTIONS、TRACE    

**幂等方法**   
多次请求的影响和单次请求的结果相同，则表示该方法是幂等方法。如PUT、DELETE、GET、HEAD、OPTIONS、TRACE   

**可缓存方法**   
方法的响应数据可以缓存下来以备不时之需。如GET、HEAD、POST

*参考文献*   
[1. RFC 7231](https://tools.ietf.org/html/rfc7231#section-4)   
