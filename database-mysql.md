- [MySQL](#MySQL)
    - [MySQL存储引擎](#MySQL存储引擎)
    - [MySQL的隔离级别及InnoDB默认隔离级别](#MySQL的隔离级别及InnoDB默认隔离级别)
    - [事务的四个特性](#事务的四个特性)
    - [索引](#索引)
        - [InnoDB索引实现原理](#InnoDB索引实现原理)
        - [索引命中原则](#索引命中原则)
        - [B-Tree和B+Tree的区别](#B-Tree和B+Tree的区别)
        - [B+Tree层数计算](#B+Tree层数计算)
        - [索引优化](#索引优化)


# MySQL
## MySQL存储引擎
InnoDB
MySQL v5.1之后版本的默认存储引擎；
将表分为.frm（表结构）和.ibd（表数据和索引）两个文件进行存储；
表基于聚簇索引建立的；
InnoDB实现了四个隔离级别，默认级别是REPETABLE READ，并通过间隙锁策略防止幻读的出现；
它的锁粒度是行级锁，采用MVCC（多版本并发控制）来支持高并发，它是典型的事务型存储引擎

MyISAM：
MySQL v5.1及之前版本的默认存储引擎；
将表分为.frm（表结构）、.MYD（表数据）、.MYI（表索引）三个文件进行存储；
表基于非聚簇索引建立的；
支持表级别锁、全文索引（这是一种基于分词创建的索引）、压缩、空间函数（GIS）；
不支持事务，崩溃后无法安全恢复
适用于只读数据，或者表比较小且允许数据丢失的情况

## MySQL的隔离级别及各自存在的问题
MySQL包括四种隔离级别：
- 未提交
脏读
- 提交读
不可重复读
- 可重复度
幻读
- 串行化
间隙锁
MVCC

InnoDB的默认隔离级别是REPEATABLE READ（可重复度），并通过间隙锁（next-key locking）策略防止幻读的出现。
间隙锁使得InnoDB不仅仅锁定查询的行，还会对索引中的间隙进行锁定，以防止幻影行的插入。

## 事务的四个特性
事务主要用来处理操作量大，复杂度高的数据。事务的处理可以维护数据的完整性，保证成批的SQL操作要么全部执行，要么全部不执行。
事务的四个特性ACID：
1. 原子性 Atomicity：一个事务中的所有操作，要么全部执行，要么全部不执行。
2. 一致性 Consistency：在事务开始之前和事务结束之后，数据的完整性没有被破坏。
3. 隔离性 Isolation：数据库允许多个并发事务同时对数据进行读写和修改，隔离性可以防止多个事务并发执行时由于事务交叉执行导致的数据不一致。
4. 永久性 Durability：事务执行之后，对数据的修改是永久的

## 索引
### InnoDB索引实现原理

### 索引的作用
1. 过滤 filter
2. 排序或分组 sort/group
3. 覆盖 cover

### 索引命中原则

### B-Tree和B+Tree的区别
1. 在B+Tree中，所有数据记录节点都是按照键值大小顺序存放在同一层的叶子节点上，而非叶子节点上只存储key值信息，这样可以大大加大每个节点存储的key值数量，降低B+Tree的高度；而在B-Tree中，每个节点都存储key和data
2. 在B+Tree的叶子结点中，每个叶子结点都维护着与该节点相邻的前驱和后继；而B-Tree中叶子结点都是独立的
3. 在B+Tree中，所有查找都会到达叶子结点；而B-Tree中找到结点即返回，不一定到达叶子结点

B-Tree
![ab7bacb0bd1bfffeefd6c51cd17f34b5.png](evernotecid://AF10A51D-E4EF-4051-986B-D4AF5BD2B8D8/appyinxiangcom/7388633/ENResource/p2328)
B+Tree
![254fe90fcd1729611103510926a67a05.png](evernotecid://AF10A51D-E4EF-4051-986B-D4AF5BD2B8D8/appyinxiangcom/7388633/ENResource/p2329)
(图片来源于网络，侵删)

### B+Tree层数计算
B+Tree的层数计算，10GB的数据，每行有10个long型的字段；或者有5000行，来计算层数

### 索引优化
分页优化
```
500万数据

数据分页：
limit 20
limit 10000, 20
limit 4900000, 20  04.100
慢查询：
limit语句偏移量越大，耗时越长
如果有覆盖索引，时间可以缩短，== 用子查询的时间
如果where id > 4900000 limit 20 0.0010，效率高

select *
from table
where id > (
		select id
		from (
			select id
			from table
			where id < 4900000
			order by id desc
			limit 20
		) as dev_info_res
		order by id
		limit 1
	)
limit 20

count
sum
group by

使用统计表
非数据库端的优化

```
乐观锁和悲观锁、行锁与表锁、共享锁与排他锁（inndob如何手动加共享锁与排他锁）

MVCC（增加两个版本号）及delete、update、select时的具体控制

死锁判定原理和具体场景

查询缓慢和解决方式（explain、慢查询日志、show profile等）

drop、truncate、delete区别

查询语句不同元素（where、jion、limit、group by、having等等）执行先后顺序

mysql优化，读写分离、主从复制

数据库崩溃时事务的恢复机制（REDO日志和UNDO日志）