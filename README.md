- [Algorithm](#Algorithm)
- [Database](#Database)
    - [MongoDB](#MongoDB)
    - [MySQL](#MySQL)
    - [Redis](#Redis)
- [ElasticSearch](#ElasticSearch)
- [Exception](#Exception)
- [Network](#Network)
    - [网络基础](#网络基础)
    - [HTTP](#HTTP)
- [Java](#Java)
    - [Java基础](#Java基础)
    - [JVM](#JVM)
    - [Spring](#Spring)
    - [Spring Boot](#Spring Boot)
- [Profile](#Profile)

# Study

## Algorithm

## Database
### [MongoDB](https://github.com/rasp210/study/blob/master/database-mongodb.md)
### [MySQL](https://github.com/rasp210/study/blob/master/database-mysql.md)
### [Redis](https://github.com/rasp210/study/blob/master/database-redis.md)
-----

## [Design Pattern](https://github.com/rasp210/study/blob/master/design-pattern)
### [设计模式](https://github.com/rasp210/study/blob/master/design-pattern/design-pattern.md)
-----

## [Docker](https://github.com/rasp210/study/tree/master/git)
-----

## [ElasticSearch](https://github.com/rasp210/study/blob/master/elasticsearch.md)

## [Exception](https://github.com/rasp210/study/blob/master/exceptions/exceptions.md)
遇到的各种异常/错误/问题等。
1. [Mac terminal ls 别名设置带颜色失败](https://github.com/rasp210/study/blob/master/exceptions/mac-alias-ls-color.md)

-----

## [Git](https://github.com/rasp210/study/tree/master/git)
Git 是分布式版本控制系统的一种。客户端把仓库完整地镜像下来，这样，任何一处协同工作用的服务器发生故障，都可以通过任何一个镜像出来的本地仓库恢复。因为每一次的克隆操作，实际上都是一次代码仓库的完整备份。
1. [简介及配置](https://github.com/rasp210/study/tree/master/git/intro-config.md)
2. [命令](https://github.com/rasp210/study/tree/master/git/command.md)
3. [冲突解决](https://github.com/rasp210/study/tree/master/git/conflict.md)
-----

## Java
### [Java基础](https://github.com/rasp210/study/blob/master/java-base.md)
### [JVM](https://github.com/rasp210/study/blob/master/java-jvm.md)
### [Spring](https://github.com/rasp210/study/blob/master/java-spring.md)
### [Spring Boot](https://github.com/rasp210/study/blob/master/java-spring-boot.md)
##### 1. Java类加载机制？

##### 2. Exception和Error有什么区别？
```
Exception和Error都是继承自Throwable类，Java中只有Throwable类型的实例才可以被抛出或者捕获，它是异常处理机制的基本组成类型，体现了Java平台设计者对不同异常情况的分类。
Exception是程序正常运行过程中，可以预料的意外情况，可以并且应该被捕获，进行相应处理。
Error是指在正常情况下，不大可能出现的情况，绝大多数Error都会导致程序处于非正常、不可恢复状态，不便于也不需要捕获。
异常又可以分为可检查和不检查异常，可检查异常必须显示的进行捕获处理，这是编译期检查的一部分；不检查异常包括运行时异常和Error。
```

##### 3. final、finally和finalize有什么不同？
```
final可以用来修饰类、方法、变量，final类不可以被继承，final方法不可以被重写，final变量不可以被修改。
finally是java保证重要代码一定要被执行的一种机制，通常使用try-finally或try-catch-finally来进行类似资源关闭等操作。但是更推荐使用JDK7中try-with-resources语句来关闭资源。
finalize是java.lang.Object的一个方法，是保证对象在被垃圾回收前完成特定资源的回收，在JDK9中已经标记为deprecated。java目前逐步使用java.lang.ref.Cleaner来替换原有的finalize，Cleaner的实现利用了幻象引用，这是一种常见的post-mortem机制。
```

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

## Network
### [网络基础](https://github.com/rasp210/study/blob/master/network-base.md)
### [HTTP](https://github.com/rasp210/study/blob/master/network-http.md)

-----

## O
### [Operating System](https://github.com/rasp210/study/blob/master/os)

-----

## P
### [PHP](https://github.com/rasp210/study/blob/master/php)   
1. [PHP源码](https://github.com/rasp210/study/blob/master/php/source-code.md)   
2. [PHP框架](https://github.com/rasp210/study/blob/master/php/framework.md)   
3. [LNMP介绍](https://github.com/rasp210/study/blob/master/php/lnmp.md)  

### [Profile](https://github.com/rasp210/study/blob/master/profile.md)   
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
