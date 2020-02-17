# Study

Writing someting about my work.
[TOC]

## A
### Algorithm
#### LeetCode
##### 1.Two Sum

>Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.   
>You may assume that each input would have **exactly** one solution, and you may not use the same element twice.   

**Example**   
>Given nums = [2, 7, 11, 15], target = 9,   
>Because nums[0] + nums[1] = 2 + 7 = 9,   
>return [0, 1].   

**Solution**   
```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* twoSum(int* nums, int numsSize, int target) {
    int* ret = malloc(2 * sizeof(int)); // 分配内存空间
    if (numsSize < 2) {
        return ret;
    }
    
    int i, j;
    int left;
    for (i = 0; i < numsSize - 1; i++) {
        left = target - nums[i];
        for (j = i + 1; j < numsSize; j++) {
            if (left == nums[j]) {
                ret[0] = i;
                ret[1] = j;   
            }
        }
    }
    return ret;
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
