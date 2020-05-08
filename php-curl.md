# curl_getinfo 返回值分析

[TOC]

### url

最后一个有效的URL地址

### http_code

最后一个收到的HTTP代码

### content_type

下载内容的*Content-Type:*值，NULL表示服务器没有发送有效的*Content-Type:*header

### http_code

最后一个收到的HTTP代码

### header_size

header部分的大小

### request_size

在HTTP请求中有问题的请求的大小

### filetime

远程获取文档的时间，如果无法获取，则返回值为“-1”

### ssl_verify_result

通过设置**CURLOPT_SSL_VERIFYPEER**返回的SSL证书验证请求的结果

### total_time

最后一次传输所消耗的时间

### namelookup_time

名称解析所消耗的时间

### connect_time

建立连接所消耗的时间

### pretransfer_time

从建立连接到准备传输所使用的时间

### starttransfer_time

从建立连接到传输开始所使用的时间

### size_upload

上传数据量的总值

### size_download

下载数据量的总值

### speed_download

平均下载速度

### speed_upload

平均上传速度

### download_content_length

下载数据量的总值

### upload_content_length

上传数据量的总值

### redirect_time

在事务传输开始前重定向所使用的时间

### redirect_count

重定向数量



### 几种时间分析

![time](C:\Users\yanmin1\Desktop\time.png)

```
namelookup_time: 解析时间， 从开始直到解析完远程请求的时间；
connect_time: 建立连接时间,从开始直到与远程请求服务器建立连接的时间；
pretransfer_time: 从开始直到第一个远程请求接收到第一个字节的时间；
starttranster_time: 从开始直到第一个字节返回给curl的时间；
total_time： 从开始直到结束的所有时间。
```



例子：

```
错误详细：[rerrno]28[rerror]Operation timed out after 600 milliseconds with 118 bytes received[curl_info]url=http://i.utils.sports.sina.com.cn/praise/apipraise/incrsimple|content_type=application/json; charset=UTF-8|http_code=200|header_size=433|request_size=331|filetime=-1|ssl_verify_result=0|redirect_count=0|total_time=0.752522|namelookup_time=1.8E-5|connect_time=0.000524|pretransfer_time=0.000526|size_upload=36|size_download=118|speed_download=156|speed_upload=47|download_content_length=-1|upload_content_length=0|starttransfer_time=0.752483|redirect_time=0|primary_ip=10.71.1.241|redirect_url=[resource]http://i.utils.sports.sina.com.cn/praise/apipraise/incrsimple
count: 206
script: http://mix.sports.sina.cn/2017_cbd/praise/incr?&callback=ijax_cb_1494489401915_6400319
referer: http://sports.sina.cn/activity/sina_running2016.shtml
clientIp: 124.127.74.15
scriptId: 
serverIp: 10.71.48.134
backtrace: 
```

```
错误详细：[rerrno]28[rerror]Operation timed out after 601 milliseconds with 0 bytes received[curl_info]url=http://i.utils.sports.sina.com.cn/praise/apipraise/incrsimple|http_code=0|header_size=0|request_size=331|filetime=-1|ssl_verify_result=0|redirect_count=0|total_time=0.600254|namelookup_time=3.0E-5|connect_time=0.000771|pretransfer_time=0.000815|size_upload=36|size_download=0|speed_download=0|speed_upload=59|download_content_length=-1|upload_content_length=36|starttransfer_time=0|redirect_time=0|primary_ip=10.71.1.241|primary_port=80|local_ip=10.71.48.147|local_port=36517|redirect_url=[resource]http://i.utils.sports.sina.com.cn/praise/apipraise/incrsimple
count: 205
script: http://mix.sports.sina.cn/2017_cbd/praise/incr?&callback=ijax_cb_1494509634801_4602276
referer: http://sports.sina.cn/activity/sina_running2016.shtml?from=timeline&isappinstalled=0
clientIp: 111.174.67.76
scriptId: 
serverIp: 10.71.48.147
backtrace:
```





参考：

[curl_getinfo]:http://php.net/manual/zh/function.curl-getinfo.php
[php curl time 分析]:https://segmentfault.com/a/1190000002467888

