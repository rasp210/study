- [JVM](#JVM)
	- [Java内存模型](#Java内存模型)
	- [性能指标](#性能指标)
		- [TPS](#TPS)
	- [Java类加载机制](#Java类加载机制)
	- [java应用启动参数](#java应用启动参数)

# JVM
## Java内存模型
```
Java内存模型（Java Memory Model，JMM）：在特定的操作协议下，对特定的内存或高速缓存的读写访问的过程抽象。
目的是：屏蔽掉各种硬件和操作系统的内存访问差异，以实现让Java程序在各个平台下都能达到一致性内存访问效果。

```

## 性能指标
### TPS
```
每秒事务处理数（Transaction Per Second，TPS）是衡量一个服务性能高低好坏的一个重要指标，它代表一秒内服务端平均能响应的请求总数
```

## Java类加载机制

## java应用启动参数
-Xint：JVM仅进行解释执行
-Xcomp：JVM仅进行编译执行，最大优化级别
JDK9
jaotc --output libHelloWorld.so HelloWorld.class
jaotc --output libjava.base.so --module java.base
java -XX:AOTLibrary=./libHelloWorld.so,./libjava.base.so HelloWorld