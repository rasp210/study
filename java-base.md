- [Java基础](#Java基础)
	- [&与&&的区别？](#&与&&的区别？)
	- [|与||的区别？](#|与||的区别？)
	- [System.out.println(3 | 9); 输出什么?](#System.out.println(3 | 9); 输出什么?)
# Java基础

## &与&&的区别？
```
相同点：
	都可作为逻辑运算符，运算符两边同为true时结果为true
区别：
	&：作为逻辑运算符，左右两边都会执行；它也是位运算符，即左右两边可以为非boolean型
	&&：具有短路效应，左边为false时，右边不会执行
例如：
	int a = 0;
    int b = 0;
    System.out.println(a > 0 & ++a > 0);
    System.out.println(a);
    System.out.println(b > 0 && ++b > 0);
    System.out.println(b);
    /*
     * false
     * 1
     * false
     * 0
     */
```

## |与||的区别？
```
相同点：
	都可以作为逻辑运算符，运算符两边同为false时结果为false
区别：
	|：作为逻辑运算符，左右两边都会执行；它也是位运算符
	||：具有短路效应，左边为true时，右边不会执行
```

## System.out.println(3 | 9); 输出什么?
```
11
```


