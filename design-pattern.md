- [DesignPattern](#DesignPattern)
    - [creational](#creational)
        - [单例模式Singleton](#单例模式Singleton)

# DesignPattern
## creational
### 抽象工厂模式AbstractFactory

### 简单工厂模式SimpleFactory

### 工厂方法模式FactoryMethod

### 静态工厂模式StaticFactory

### 对象池模式Pool

### 原型模式Prototype

### 建造者模式Builder

### 单例模式Singleton
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

### 多例模式Multition

## structural

### 2.1 adapter / wrapper 适配器模式

### 2.2 bridge 桥梁模式 

### 2.3 composite 组合模式

### 2.4 data mapper 数据映射模式

### 2.5 decoration 装饰模式

### 2.6 dependency injection 依赖注入模式

### 2.7 facade 门面模式

### 2.8 fluent interface 流接口模式

### 2.9 flyweight 享元模式

### 2.10 proxy 代理模式

### 2.11 registry注册模式

## behavioral

### 3.1 chain of responsibilities 责任链模式

### 3.2 command 命令行模式

### 3.3 iterator 迭代器模式

### 3.4 mediator 中介者模式

### 3.5 memento 备忘录模式

### 3.6 null object 空对象模式

### 3.7 observer 观察者模式

### 3.8 specification 规格模式

### 3.9 state 状态模式

### 3.10 strategy 策略模式

### 3.11 template method 模板方法模式

### 3.12 visitor 访问者模式

## more

### 4.1 delegation 委托者模式

### 4.2 service locator 服务定位器模式

### 4.3 repository 资源库模式

### 4.4 entity attribute value（EVA） 实体属性值模式

## appendix

### 5.1 anti-pattern 反面模式

### 5.2 PHP 设计模式阅读清单



