# Study

[TOC]

## A
### Algorithm
#### LeetCode
##### 10.正则表达式匹配
```
给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '*' 的正则表达式匹配。

'.' 匹配任意单个字符
'*' 匹配零个或多个前面的那一个元素
所谓匹配，是要涵盖 整个 字符串 s的，而不是部分字符串。

说明:

s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 . 和 *。
示例 1:

输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/regular-expression-matching
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

```java
public static void main(String[] args) {
    String[][] arr = {
            {"", ""},
            {"", "a*"},
            {"aa", "a*"},
            {"aa", ".*"},
            {"ab", "c*a*b"},
            {"a", ""},
            {"", "a"},
            {"", "."},
            {"aa", "a"},
    };
    for (String[] item: arr) {
        System.out.println(isMatch(item[0], item[1]));
    }
}
public static boolean isMatch(String s, String p) {
	if (p.isEmpty()) {
		return s.isEmpty();
	}

	boolean firstMatch = (!s.isEmpty() && (s.charAt(0) == p.charAt(0) || p.charAt(0) == '.'));

	if (p.length() >= 2 && p.charAt(1) == '*') {
		return (firstMatch && isMatch(s.substring(1), p)) || isMatch(s, p.substring(2));
	} else {
		return firstMatch && isMatch(s.substring(1), p.substring(1));
	}
}
```
#### 几种常见的排序算法

#### 数组循环右移k位
-----

## D
### [Database](https://github.com/rasp210/study/blob/master/database)
##### [MySQL](https://github.com/rasp210/study/blob/master/database/mysql)
1. [Engine 引擎](https://github.com/rasp210/study/blob/master/database/mysql/engine.md)
2. [Index 索引](https://github.com/rasp210/study/blob/master/database/mysql/index.md)
3. [Transaction 事务](https://github.com/rasp210/study/blob/master/database/mysql/transaction.md)
4. [Explain](https://github.com/rasp210/study/blob/master/database/mysql/explain.md)

##### [Redis](https://github.com/rasp210/study/blob/master/database/redis)
##### [MongoDB](https://github.com/rasp210/study/blob/master/database/mongodb)
-----

### [Design Pattern](https://github.com/rasp210/study/blob/master/design-pattern)
##### [设计模式](https://github.com/rasp210/study/blob/master/design-pattern/design-pattern.md)
-----

### [Docker](https://github.com/rasp210/study/tree/master/git)
-----

## E
### [Exception](https://github.com/rasp210/study/blob/master/exceptions/exceptions.md)
遇到的各种异常/错误/问题等。
1. [Mac terminal ls 别名设置带颜色失败](https://github.com/rasp210/study/blob/master/exceptions/mac-alias-ls-color.md)

-----

## G
### [Git](https://github.com/rasp210/study/tree/master/git)
Git 是分布式版本控制系统的一种。客户端把仓库完整地镜像下来，这样，任何一处协同工作用的服务器发生故障，都可以通过任何一个镜像出来的本地仓库恢复。因为每一次的克隆操作，实际上都是一次代码仓库的完整备份。
1. [简介及配置](https://github.com/rasp210/study/tree/master/git/intro-config.md)
2. [命令](https://github.com/rasp210/study/tree/master/git/command.md)
3. [冲突解决](https://github.com/rasp210/study/tree/master/git/conflict.md)
-----

## J
### Java
#### 基础
##### 1. Java类加载机制？

#### 虚拟机
##### 1. 常见的垃圾回收器
SerialGC
ParallelGC
CMS
G1
基本原理？
适用于什么样的工作负载？

##### 2. 诊断工具？
jmap
jstack
jconsole
jhsdb
jcmd

##### 3. java是解释执行还是编译执行
javac是将源码编译成字节码的.class文件
在运行时，JVM会通过类加载器加载字节码，解释或者编译执行
JDK8是解释和编译混合的一种模式(-Xmixed)
通常运行在server模式下的JVM，会进行上万次的调用，以收集足够的信息进行高效编译
client模式这个门限是1500次，适用于对启动速度敏感的应用
默认采用分层编译
AOT编译（Ahead Of Time Compililation）：直接将字节码编译成机器码，这样就避免了JIT预热等各方面的开销（JDK9）

##### 4. java应用启动参数
-Xint：JVM仅进行解释执行
-Xcomp：JVM仅进行编译执行，最大优化级别
JDK9
jaotc --output libHelloWorld.so HelloWorld.class
jaotc --output libjava.base.so --module java.base
java -XX:AOTLibrary=./libHelloWorld.so,./libjava.base.so HelloWorld

#### IO/NIO
#### 并发
#### 分布式
#### 安全
#### 性能
#### 范型
#### Lambda
#### 反射
-----
## L
### [Linux](https://github.com/rasp210/study/blob/master/linux)   
- [awk](https://github.com/rasp210/study/blob/master/linux/awk.md)     
- [ps](https://github.com/rasp210/study/blob/master/linux/ps.md)   
- [rpm](https://github.com/rasp210/study/blob/master/linux/rpm.md)   
- [sed](https://github.com/rasp210/study/blob/master/linux/sed.md)   
- [uniq](https://github.com/rasp210/study/blob/master/linux/uniq.md)
- [Shell特殊变量](https://github.com/rasp210/study/blob/master/linux/shell-special-variable.md)   

-----

## N
### [Network](https://github.com/rasp210/study/blob/master/network)

-----

## O
### [Operating System](https://github.com/rasp210/study/blob/master/os)

-----

## P
### [PHP](https://github.com/rasp210/study/blob/master/php)   
1. [PHP源码](https://github.com/rasp210/study/blob/master/php/source-code.md)   
2. [PHP框架](https://github.com/rasp210/study/blob/master/php/framework.md)   
3. [LNMP介绍](https://github.com/rasp210/study/blob/master/php/lnmp.md)     
-----

### [Python](https://github.com/rasp210/study/blob/master/python)

-----

## S
### [Security](https://github.com/rasp210/study/blob/master/security)

常见网络攻击，安全问题处理等。

-----

## T
### [Tool](https://github.com/rasp210/study/blob/master/tool)
Excel，Markdown等工具的使用。
1. [Excel](https://github.com/rasp210/study/blob/master/tool/excel.md)
-----
