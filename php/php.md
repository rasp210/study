# php

[TOC]

## final 关键字

PHP 5 中新加了一个 final 关键字。如果父类中某个方法声明为final，则子类不能覆盖该方法；如果一个类声明为final，则不能别继承。

## trait 关键字

自 PHP 5.4.0 起，PHP 实现了一种代码复用的方法称为 trait。

1. 优先级顺序：当前类成员覆盖trait中的方法，trait中的方法覆盖父类的方法。
2. 多个trait冲突解决：方法名一样导致的冲突，使用 insteadof 或 as
3. 修改方法的访问控制：使用 as

```php
<?php
// 1.优先级顺序
class A1 {
    public function sayA() {
        echo 'a1';
    }
}

trait A2 {
    public function sayA() { 
        echo 'a2';
    }    
}

class A3 extends A1
{
    use A2;
    public function sayA() { // a3，注释掉后是a2，trait中注释掉则为a1
        echo 'a3';
    }
}

$a = new A3();
$a->sayA();

/*****************************************************************************************/

// 2. 多个trait冲突解决
trait A {
    public function smallTalk() {
        echo 'a';
    }
    public function bigTalk() {
        echo 'A';
    }
}

trait B {
    public function smallTalk() {
        echo 'b';
    }
    public function bigTalk() {
        echo 'B';
    }
}

class Talker {
    // use A; // OK
    // use A, B; // E_STRICT，可以用 insteadof（实例关系）和 as（别名）来解决，信息：PHP Fatal error:  Trait method smallTalk has not been applied, because there are collisions with other trait methods on Talker in a.php on line 20
    use A, B {
        B::smallTalk insteadof A;
        A::bigTalk insteadof B;
    }
}

class Aliased_Talker {
    use A, B {
        B::smallTalk insteadof A;
        A::bigTalk insteadof B;
        B::bigTalk as talk;
    }
}

$t = new Talker();
$t->smallTalk(); // b
$t->bigTalk(); // A

$at = new Aliased_Talker();
$at->smallTalk(); // b
$at->bigTalk(); // A
$at->talk(); // B

/*****************************************************************************************/

// 3.修改方法的访问控制权限
trait HelloWorld {
    public function sayHello() {
        echo 'Hello World!';
    }
}

// 修改 sayHello 的访问控制
class MyClass1 {
    use HelloWorld { sayHello as protected; }
    
    public function test() {
        $this->sayHello();
    }
}

// 给方法一个改变了访问控制的别名
// 原版 sayHello 的访问控制则没有发生变化
class MyClass2 {
    use HelloWorld { sayHello as private myPrivateHello; }
    
    public function test() {
        $this->myPrivateHello();
    }
}


$mc1 = new MyClass1();
$mc1->test(); // Hello World!
// $mc1->sayHello(); // Fatal error: Uncaught Error: Call to protected method MyClass1::sayHello() from context '' in /usercode/file.php:30

$mc2 = new MyClass2();
$mc2->test(); // Hello World!
$mc2->sayHello(); // Hello World!
// $mc2->myPrivateHello(); // Fatal error: Uncaught Error: Call to private method MyClass2::myPrivateHello() from context '' in /usercode/file.php:34

?>
```



## 单例模式代码示例

```php
// 不被继承
final class Singleton
{
    private static $instance;
    
    // 不允许从外部调用以防止创建多个实例
    private function __construct() {   
    }
     
    // 防止实例被克隆，这会创建实例的副本
    private function __clone() {   
    }
    
    // 防止反序列化，这会创建它的副本
    private function __wakeup() {
    }
    
    // 通过懒加载获得实例（在第一次使用的时候创建）
    public static function getInstance(): Singleton {
        if (NULL === static::$instance) {
            static::$instance = new static();
        }
        return static::$instance;
    }
}
```

```php
<?php 
class Person
{
    private $name, $age, $sex, $info;

    public function __construct($name, $age, $sex)
    {
        $this->name = $name;
        $this->age  = $age;
        $this->sex  = $sex;
        $this->info = sprintf("prepared by construct magic function name: %s age: %d sex: %s",
        $this->name, $this->age, $this->sex);
    }

    public function getInfo()
    {
        echo $this->info . PHP_EOL;
    }
    
    /**
     * serialize前调用 用于筛选需要被序列化存储的成员变量
     * @return array [description]
     */
    public function __sleep()
    {   
        echo __METHOD__ . PHP_EOL;
        //序列化时只会存储 name age sex, info 不会被序列化
        return ['name', 'age', 'sex'];
    }
    
    /**
     * unserialize前调用 用于预先准备对象资源
     */
    public function __wakeup()
    {
        echo __METHOD__ . PHP_EOL;
        $this->info = sprintf("prepared by wakeup magic function name: %s age: %d sex: %s",
        $this->name, $this->age, $this->sex);
    }
}

$boy = new Person( 'sallency', 25, 'male' );
$boy->getInfo(); //构造函数组装的 $info，output：prepared by construct magic function name: sallency age: 25 sex: male

$temp = serialize($boy); //序列化时先调用 __sleep() 并不会存储 $info 属性，output：Person::__sleep
echo $temp . PHP_EOL; //O:6:"Person":3:{s:12:"Personname";s:8:"sallency";s:11:"Personage";i:25;s:11:"Personsex";s:4:"male";}

$boy = unserialize($temp); //反序列化时会调用 __wakeup()，output：Person::__wakeup
$boy->getInfo(); //__wakeup() 组装的 $info，prepared by wakeup magic function name: sallency age: 25 sex: male

/*
输出如下：
prepared by construct magic function name: sallency age: 25 sex: male
Person::__sleep
O:6:"Person":3:{s:12:"Personname";s:8:"sallency";s:11:"Personage";i:25;s:11:"Personsex";s:4:"male";}
Person::__wakeup
prepared by wakeup magic function name: sallency age: 25 sex: male
*/
?>
```



## linux系统杀进程的几种方式

通过 `ps -ef` 或 `ps -aux` 找到进程的pid

1. kill PID：为了防止僵尸进程，杀死进程前先把所有子进程杀掉
2. kill -l PID：以优雅的方式结束进程。会已注销的方式结束进程，并同时杀掉所有的子进程，但也有杀不掉的情况
3. kill -TERM PPID：给父进程发送一个 TERM 信号来终止它及其子进程
4. kill -HUP PID：停止和重启进程，比较和缓
5. kill -s 9 PID：强制、尽快杀死进程

## 正则

### ?:

括号开头的?:表示不捕获该括号内的正则，例如php的preg_match()

```php
preg_match('/^(?:(?:[0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.){3}(?:[0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])$/', '1.1.1.1', $match);

/*
$match 输出如下：
array(1) {
  [0]=>
  string(7) "1.1.1.1"
}
*/

preg_match('/^(([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.){3}([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])$/', '1.1.1.1', $match);
/*
$match 输出如下：
array(4) {
  [0]=>
  string(7) "1.1.1.1"
  [1]=>
  string(2) "1." // 第一个括号
  [2]=>
  string(1) "1" // 第二个括号
  [3]=>
  string(1) "1" // 第三个括号
}
*/
```

## 删除软连接

`rm -rf soft_link_name`  *注意：后面没有斜杠，带上斜杠删除失败*

