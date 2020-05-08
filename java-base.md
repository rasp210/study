- [Java基础](#Java基础)
    - [基础语法](#基础语法)
        - [Java的基本数据类型及其范围](#Java的基本数据类型及其范围)
        - [Java基本类型的自动类型转换关系](#Java基本类型的自动类型转换关系)
        - [判断浮点型运算结果是否等于某值可以用等号吗](#判断浮点型运算结果是否等于某值可以用等号吗)
        - [java是解释执行还是编译执行](#java是解释执行还是编译执行)
        - [final、finally和finalize有什么不同](#final、finally和finalize有什么不同)
    	- [&与&&的区别？](#&与&&的区别？)
    	- [|与||的区别？](#|与||的区别？)
    	- [按位与&和取模%的等价公式](#按位与&和取模%的等价公式)
    	- [System\.out\.println\(3\|9\)\;输出什么?](#System.out.println\(3\|9\)\;输出什么\?)
    - [Exception和Error有什么区别？](#Exception和Error有什么区别？)
    - [JavaIO](#JavaIO)
        - [IO和NIO的区别](#IO和NIO的区别)
        - [NIO的三大核心组件](#NIO的三大核心组件)
    - [设计模式](#设计模式)
        - [单例模式](#单例模式)

# Java基础
## 基础语法
### Java的基本数据类型及其范围

类型|字节|范围|备注
---|---|---|---
byte|1B|-2^7 ~ 2^7 - 1|
short|2B|-2^15 ~ 2^15 - 1|
int|4B|-2^31 ~ 2^31 - 1|
long|8B|-2^63 ~ 2^63 - 1|
float|4B|3.402823e+38 ~ 1.401298e-45 (1位符号位，有效位6～7位)|浮点数是不精确的，不能对浮点数进行精确比较
double|8B|1.797693e+308 ~ 4.9000000e-324 (1位符号位，有效位15位)|浮点数是不精确的，不能对浮点数进行精确比较
char|2B|0 ~ 65535，\u0000 ~ \uFFFF(前128位与ASCII)|
boolean|1B|true、false|

### Java基本类型的自动类型转换关系
无精度损失转换：
- byte -> short -> int -> long
- char -> char
- int -> double
- float -> double

有精度损失转换：
- int --> float
- long --> double
- long --> float

### 判断浮点型运算结果是否等于某值可以用等号吗
不可以，因为浮点数值采用二进制系统表示，而二进制系统中无法精确表示分数1/10

### java是解释执行还是编译执行
javac是将源码编译成字节码的.class文件
在运行时，JVM会通过类加载器加载字节码，解释或者编译执行
JDK8是解释和编译混合的一种模式(-Xmixed)
通常运行在server模式下的JVM，会进行上万次的调用，以收集足够的信息进行高效编译
client模式这个门限是1500次，适用于对启动速度敏感的应用
默认采用分层编译
AOT编译（Ahead Of Time Compililation）：直接将字节码编译成机器码，这样就避免了JIT预热等各方面的开销（JDK9）

### final、finally和finalize有什么不同
final可以用来修饰类、方法、变量，final类不可以被继承，final方法不可以被重写，final变量不可以被修改。
finally是java保证重要代码一定要被执行的一种机制，通常使用try-finally或try-catch-finally来进行类似资源关闭等操作。但是更推荐使用JDK7中try-with-resources语句来关闭资源。
finalize是java.lang.Object的一个方法，是保证对象在被垃圾回收前完成特定资源的回收，在JDK9中已经标记为deprecated。java目前逐步使用java.lang.ref.Cleaner来替换原有的finalize，Cleaner的实现利用了幻象引用，这是一种常见的post-mortem机制。

### &与&&的区别？
相同点：
> 都可作为逻辑运算符，运算符两边同为true时结果为true

区别：
> &：作为逻辑运算符，左右两边都会执行；它也是位运算符，即左右两边可以为非boolean型
> &&：具有短路效应，左边为false时，右边不会执行

例如：
```
    int a = 0;
    int b = 0;
    System.out.println(a > 0 & ++a > 0); // false
    System.out.println(a); // 1
    System.out.println(b > 0 && ++b > 0); // false
    System.out.println(b); // 0
```
	

### |与||的区别？
相同点：
> 都可以作为逻辑运算符，运算符两边同为false时结果为false

区别：
> |：作为逻辑运算符，左右两边都会执行；它也是位运算符
> ||：具有短路效应，左边为true时，右边不会执行

### System.out.println(3 | 9); 输出什么?
11

## Exception和Error有什么区别？
Exception和Error都是继承自Throwable类，Java中只有Throwable类型的实例才可以被抛出或者捕获，它是异常处理机制的基本组成类型，体现了Java平台设计者对不同异常情况的分类。

Exception是程序正常运行过程中，可以预料的意外情况，可以并且应该被捕获，进行相应处理。

Error是指在正常情况下，不大可能出现的情况，绝大多数Error都会导致程序处于非正常、不可恢复状态，不便于也不需要捕获。

异常又可以分为可检查和不检查异常，可检查异常必须显示的进行捕获处理，这是编译期检查的一部分；不检查异常包括运行时异常和Error。

## JavaIO
### IO和NIO的区别
```
1. NIO是面向缓冲区的，IO是面向流的
2. NIO具有选择器特性Selector
3. NIO具有管道特性Channel
4. NIO是非阻塞的，IO是阻塞的
```

### NIO的三大核心组件
```
1. Buffer
2. Selector
3. Channel
```
