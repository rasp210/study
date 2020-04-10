- [Java基础](#Java基础)
    - [基础语法](#基础语法)
    	- [&与&&的区别？](#&与&&的区别？)
    	- [|与||的区别？](#|与||的区别？)
    	- [System\.out\.println\(3\|9\)\;输出什么?](#System.out.println\(3\|9\)\;输出什么\?)
    - [JavaIO](#JavaIO)
        - [IO和NIO的区别](#IO和NIO的区别)
        - [NIO的三大核心组件](#NIO的三大核心组件)
    - [Java容器](#Java容器)
        - [ArrayList和LinkedList的区别](#ArrayList和LinkedList的区别)
        - [ArrayList和Vector的区别](#ArrayList和Vector的区别)
        - [ArrayList的扩容机制](#ArrayList的扩容机制)
        - [ArrayList在指定索引位置添加元素](#ArrayList在指定索引位置添加元素)
        - [ArrayList如果添加元素特别多应设置初始容量](#ArrayList如果添加元素特别多应设置初始容量)
        - [HashMap的扩容机制](#HashMap的扩容机制)
        - [HashMap的put和remove的时间复杂度](#HashMap的put和remove的时间复杂度)
    - [设计模式](#设计模式)
        - [单例模式](#单例模式)

# Java基础

## 基础语法
### &与&&的区别？
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

### |与||的区别？
```
相同点：
	都可以作为逻辑运算符，运算符两边同为false时结果为false
区别：
	|：作为逻辑运算符，左右两边都会执行；它也是位运算符
	||：具有短路效应，左边为true时，右边不会执行
```

### System.out.println(3 | 9); 输出什么?
```
11
```

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

## Java容器
### ArrayList和LinkedList的区别
```
相同点：
都是不同步的，不能保证线程安全
不同点：
1. ArrayList底层实现使用的是Object数组；LinkedList使用的是双向链表
2. ArrayList支持随机访问，但是插入和删除元素时间复杂度为O(N)；LinkedList支持O(1)时间插入和删除，但是不支持随机访问
3. ArrayList空间占用体现在列表末尾的预留空间；LinkedList空间占用体现每个元素需要保存在前驱后继信息

实现了RandomAccess接口的list，优先选择普通for循环，其次foreach；
未实现RandomAccess接口的list，优先选择iterator遍历（foreach遍历底层也是通过iterator实现的），大size的数据千万不要使用普通for循环
```

### ArrayList和Vector的区别
```
Vector中的所有方法都是同步的，保证线程安全，但是在处理线程同步上会耗费大量的时间；
ArrayList是不同步的，不能保证线程安全
```

### ArrayList的扩容机制
```
    // 默认容量大小
    private static final int DEFAULT_CAPACITY = 10;

    // 空元素
    private static final Object[] EMPTY_ELEMENTDATA = {};

    // 初始化空元素
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

    // 如果等于DEFAULTCAPACITY_EMPTY_ELEMENTDATA，当第一个元素添加之后，会扩展DEFAULT_CAPACITY容量
    transient Object[] elementData; // non-private to simplify nested class access

    // 元素个数
    private int size;

   // 方式一：默认构造函数，实际上初始化赋值的是一个空数组。当真正对数组进行添加元素操作时，才真正分配容量。即向数组中添加第一个元素时，数组容量扩为10
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }

    // 方式二：用户自己指定容量
    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+initialCapacity);
        }
    }

    // 方式三：用指定集合填充
    public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {
            // c.toArray might (incorrectly) not return Object[] (see 6260652)
            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }

    // 针对方式一，添加元素
    public boolean add(E e) {
        // 确保添加新元素的最小容量可满足
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
    private void ensureCapacityInternal(int minCapacity) {
        ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
    }
    // 计算容量
    private static int calculateCapacity(Object[] elementData, int minCapacity) {
        // 如果列表还未添加任何元素，返回默认值10和1中的最大值
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        }
        return minCapacity;
    }
    private void ensureExplicitCapacity(int minCapacity) {
        modCount++;
        // 最小容量超出分配空间，需要扩容
        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ? Integer.MAX_VALUE : MAX_ARRAY_SIZE;
    }
```

### ArrayList在指定索引位置添加元素
```java
// 在index位置添加新元素，将index及之后的元素往后移动，不建议如此使用
public void add(int index, E element) {
    rangeCheckForAdd(index);

    ensureCapacityInternal(size + 1);  // Increments modCount!!
    System.arraycopy(elementData, index, elementData, index + 1, size - index);
    elementData[index] = element;
    size++;
}
private void rangeCheckForAdd(int index) {
    if (index > size || index < 0)
        throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
}
```

### ArrayList如果添加元素特别多应设置初始容量
调用此方法，就不会持续的一次次扩容，插入一千万的数据，两者时间消耗是 2～20倍左右
ArrayList<Object> list = new ArrayList<>();
list.ensureCapacity(10000000);
```java
public void ensureCapacity(int minCapacity) {
    int minExpand = (elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
        // any size if not default element table
        ? 0
        // larger than default for default empty table. It's already
        // supposed to be at default size.
        : DEFAULT_CAPACITY;

    if (minCapacity > minExpand) {
        ensureExplicitCapacity(minCapacity);
    }
}
```

### System.arraycopy方法和Arrays.copyOf方法区别
```
// Arrays.copyOf()主要用来扩容，底层也是System.arraycopy
int[] oldArr = {0, 1, 2, 3};
oldArr = Arrays.copyOf(oldArr, 10);
System.out.println(Arrays.toString(oldArr));
// [0, 1, 2, 3, 0, 0, 0, 0, 0, 0]

// System.arraycopy主要是插入删除数组元素
System.arraycopy(oldArr, 2, oldArr, 3, 2);
oldArr[2] = 99;
System.out.println(Arrays.toString(oldArr));
// [0, 1, 99, 2, 3, 0, 0, 0, 0, 0]
```

## 设计模式
### 单例模式
饿汉式：线程安全
```java
public class Singleton {
    private static Singleton instance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return instance;
    }
} 
```
由于使用static关键字进行了修饰，只能获取到一个对象，从而达到了单例；
在Singleton类初始化的时候就创建了对象，加载到了内存，造成资源浪费

懒汉式：线程不安全
```java
public class Singleton {
    private static Singleton instance = null;

    private Singleton() {}

    public static Singleton getInstance() {
        if (null == instance) {
            instance = new Singleton();
        }
        return instance;
    }
} 
```
这种方法在调用Singleton.getInstance()时才会创建对象，起到了延迟加载的作用；
这样的写法在多个线程同时运行时，很有可能会产生多个实例对象，导致线程安全问题

懒汉式：线程安全
```java
public class Singleton {
    private static Singleton instance = null;

    private Singleton() {}

    public synchronized static Singleton getInstance() {
        if (null == instance) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

[参考地址](#https://www.cnblogs.com/ckgame/p/8328347.html)
