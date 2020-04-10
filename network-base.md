- [网络基础](#网络基础)
	- [从输入url地址到页面加载，整个过程都发生了什么？](#从输入url地址到页面加载，整个过程都发生了什么？)
	- [说一下同源策略](#说一下同源策略)
	- [如何跨域](#如何跨域)
	- [HTTP常用的请求方法及其作用](#HTTP常用的请求方法及其作用)
	- [说一下转发（Forword）和重定向（Redirect）的区别？](#说一下转发（Forword）和重定向（Redirect）的区别？)
	- [](#)
	- [](#)
	- [](#)
	- [](#)

# 网络基础

## 从输入url地址到页面加载，整个过程都发生了什么？
[参考地址](https://segmentfault.com/a/1190000006879700)
```
1. 浏览器查找该url是否存在缓存，缓存是否过期
如浏览器查看`Cache-Control`属性的`max-age`，`Expires`属性，`Last-modified`属性，`Etag`属性等。前两个浏览器自己判断是否过期，过期后重新发起请求；后两个先发送判断字段`If_modified_since`或`If-none-match`给服务器，服务器判断缓存是否有效，有效则返回304，无效返回新资源
Cache-Control: private
Cache-Control: max-age = 484200
Expires: Fri, 03 Apr 2020 02:48:01 GMT

Last-modified    if-modified-since
Etag     if-none-match

2. DNS解析
即通过网址找到目标机器的物理IP地址。
首先, 查找系统缓存(/etc/hosts)，路由器缓存等是否存在；
如果没有找到，`本地域名服务器`会向`根域名服务器（.）`发送一个请求，查询IP地址；
如果没有找到，`本地域名服务器`会向`顶级域名服务器（.com.）`发送一个请求，查询IP地址；
如果没有找到，`本地域名服务器`会向`二级域名服务器（google.com.）`发送一个请求，查询IP地址；
直到最后，`本地域名服务器`得到此网址的IP地址，并把它缓存到本地，供下次查询使用。
从上述过程中，可以看出网址的解析是一个从右向左的过程:`.`->`.com.`->`google.com.`->`www.google.com.`

### 3. TCP连接
三次握手
### 4. 浏览器向服务器发送HTTP请求
### 5. 服务器处理请求并返回HTTP报文
### 6. 浏览器解析渲染页面
### 7. 关闭TCP连接
四次挥手
```

## 说一下同源策略
```
SOP（Same Origin Policy）：如果两个url地址的协议、域名、端口都相同，则表示两个地址同源
协议：http、https
域名：www.baidu.com、baidu.com
端口：80、443、其他
```

## 如何跨域
```
前端实现跨域：
1. 通过jsonp跨域
2. document.domain + iframe跨域
3. location.hash + iframe
4. window.name + iframe跨域
5. postMessage跨域
6. 跨域资源共享（CORS）
7. nginx代理跨域
8. nodejs中间件代理跨域
9. WebSocket协议跨域
后端实现跨域：
1. controller类或方法加 @CrossOrigin 注解
2. 拦截器设置header
@Configuration
public class WebConfig extends WebMvcConfigurerAdapter {
    @Autowired
    private EnvConfig envConfig;
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new HandlerInterceptor() {
            @Override
            public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
                    throws Exception {
                boolean isTrue = envConfig.getIsDev();//判断是测试服才需要解决跨域问题
                if (isTrue) {
                    response.addHeader("Access-Control-Allow-Origin", "*");
　　　　　　　　　　   response.addHeader("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS");
                    response.addHeader("Access-Control-Allow-Headers",
                            "Content-Type,X-Requested-With,accept,Origin,Access-Control-Request-Method,Access-Control-Request-Headers,token");
                }
                return true;
            }

            @Override
            public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
                    ModelAndView modelAndView) throws Exception {

            }

            @Override
            public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
                    Exception ex) throws Exception {
            }
        });
    }
}
```

## HTTP常用的请求方法及其作用
```
GET：用于从指定的地址获取数据。请求不安全、数据可缓存、长度受限制
HEAD：同GET，但是没有响应体
POST：用于创建或修改资源。请求安全、不可缓存、长度无限制
PUT：用来创建或更新资源
DELETE：用来删除指定资源
OPTIONS：通信选项
CONNECTION：
TRACE：
```

## 说一下转发（Forword）和重定向（Redirect）的区别？
```
Forword：是服务器行为
Redirect：是客户端行为，它利用服务器返回的状态码来实现的，如果状态码为为301或302，则浏览器会到新地址请求资源
区别：
	1. 地址栏显示：Forword不变，Redirect显示的是新地址
	2. 数据共享：Forword转发页面和转发到的页面共享request数据，Redirect不共享
	3. 使用场景：Forword一般用于用户登录，根据不同角色转发到相应模块；Redirect一般用于注销登录后的页面跳转
	4. 请求效率：Forword效率高于Redirect

```

