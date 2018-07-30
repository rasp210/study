# Excel

[TOC]

## 文件操作

### 1. 加密

文件-->信息-->保护工作簿-->用密码进行加密-->输入密码-->确认密码-->确定，完成！

![img](https://github.com/rasp210/study/blob/master/images/excel-01.png)

## 公式相关

### 1. 在某列插入时间，每行相差半小时

=TEXT("5:00"+(ROW(A1)-1)/48, "h:mm")

```
=TEXT(ROW(A1),"yyyy/MM/dd h:mm")
ROW(A1)的值是1，以此类推ROW(An)的值是n，以日期时间的格式输出就是Excel的起始时间：1900/01/0n 00:00:00

=TEXT(ROW(A2)/48,"yyyy/MM/dd h:mm")
即 (1900/01/0n 00:00:00 - 1900/01/01 00:00:00) / 48，如果 n=1，值即为 30 分钟；n=5，值即为 2小时30分钟
```



