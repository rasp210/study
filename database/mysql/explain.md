# MySQL Explain

[TOC]

mysql 的 explain 命令，用来对 select 语句进行分析并输出 select 执行的详细信息。可以查看有没有命中索引，是否进行了全表扫描等。

## 语法

用法一：`explain <表名>` ，返回结果和命令 `desc <表名>` 、`describe <表名>` 相同。

```mysql
mysql> explain student;
+--------------+------------------+------+-----+---------+----------------+
| Field        | Type             | Null | Key | Default | Extra          |
+--------------+------------------+------+-----+---------+----------------+
| id           | int(5) unsigned  | NO   | PRI | NULL    | auto_increment |
| name         | varchar(15)      | NO   |     |         |                |
| is_del       | tinyint(1)       | NO   |     | 0       |                |
+--------------+------------------+------+-----+---------+----------------+
```

用法二：`explain <sql 语句>`

```mysql
mysql> explain select * from student limit 2;
+----+-------------+-------------+------+---------------+------+---------+------+------+-------+
| id | select_type | table       | type | possible_keys | key  | key_len | ref  | rows | Extra |
+----+-------------+-------------+------+---------------+------+---------+------+------+-------+
|  1 | SIMPLE      | student     | ALL  | NULL          | NULL | NULL    | NULL |   24 |       |
+----+-------------+-------------+------+---------------+------+---------+------+------+-------+
```

下面对用法二进行详细介绍。创建student和course表，并插入数据。

```mysql
CREATE TABLE `student` (
  `id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(50) NOT NULL,
  `age` TINYINT(4) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8;
INSERT  INTO `student`(`id`,`name`,`age`) VALUES (1,'a',21),(2,'b',23),(3,'c',50),(4,'d',15),(5,'e',20),(6,'f',21),(7,'g',23),(8,'h',50),(9,'i',15);
CREATE TABLE `course` (
  `id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
  `sid` INT(10) UNSIGNED NOT NULL,
  `course_name` VARCHAR(100) NOT NULL,
  `score` VARCHAR(50) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `sid_cn_score_index` (`sid`,`course_name`,`score`)
) ENGINE=INNODB AUTO_INCREMENT=19 DEFAULT CHARSET=utf8;
INSERT  INTO `course`(`id`,`sid`,`course_name`,`score`) VALUES (1,1,'课程1','80'),(10,1,'课程2','89'),(2,2,'课程1','80'),(11,2,'课程2','90'),(3,3,'课程1','80'),(12,3,'课程2','90'),(4,4,'课程1','80'),(13,4,'课程2','90'),(5,5,'课程1','80'),(14,5,'课程2','90'),(6,6,'课程1','80'),(15,6,'课程2','90'),(7,7,'课程1','80'),(16,7,'课程2','90'),(8,8,'课程1','80'),(17,8,'课程2','90'),(9,9,'课程1','80'),(18,9,'课程2','90');

```

## 字段解释

### 一、id

select 语句的执行顺序号。

1. id值从大到小依次执行查询
2. id值相同时，从上往下执行
3. id值可为NULL，此时table值为 `<unionM,N>` ，表示联合查询结果的id值为M和N的查询结果的union

情况一：id为1。简单sql语句。

```mysql
eg1.1:
mysql> EXPLAIN SELECT * FROM `course` WHERE id = 3 \G;
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: course
   partitions: NULL
         type: const
possible_keys: PRIMARY
          key: PRIMARY
      key_len: 4
          ref: const
     filtered: 100.00
         rows: 1
        Extra: 
```

情况二：多个id值相同。包括left join语句、

```mysql
eg1.2:
mysql> EXPLAIN SELECT course.*, student.`name` FROM `course` LEFT JOIN `student` ON course.`sid` = student.`id` \G;
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: course
         type: index
possible_keys: NULL
          key: sid_cn_score_index
      key_len: 458
          ref: NULL
         rows: 18
        Extra: Using index
*************************** 2. row ***************************
           id: 1
  select_type: SIMPLE
        table: student
         type: eq_ref
possible_keys: PRIMARY
          key: PRIMARY
      key_len: 4
          ref: mix.course.sid
         rows: 1
        Extra: 
2 rows in set (0.00 sec)
```

情况三：不同优先级的sql语句。

```mysql
eg1.3:
mysql> EXPLAIN SELECT * FROM `course` WHERE sid IN (SELECT student.`id` FROM `student`) \G;
*************************** 1. row ***************************
           id: 1
  select_type: PRIMARY
        table: course
         type: index
possible_keys: NULL
          key: sid_cn_score_index
      key_len: 458
          ref: NULL
         rows: 18
        Extra: Using where; Using index
*************************** 2. row ***************************
           id: 2
  select_type: DEPENDENT SUBQUERY
        table: student
         type: unique_subquery
possible_keys: PRIMARY
          key: PRIMARY
      key_len: 4
          ref: func
         rows: 1
        Extra: Using index
2 rows in set (0.00 sec)
```

### 二、select_type

每个select子句的类型。

#### 1）SIMPLE

简单select（不使用union或子查询），详见eg1.1。

#### 2）PRIMARY

查询中若包含任何复杂的子查询，最外层的select被标记为PRIMARY，详见eg1.3。

#### 3）UNION

UNION中的第二个或后面的select语句。

```mysql
eg2.1：
mysql> EXPLAIN SELECT sid FROM `course` UNION SELECT id FROM `student` LIMIT 5 \G;
*************************** 1. row ***************************
           id: 1
  select_type: PRIMARY
        table: course
         type: index
possible_keys: NULL
          key: sid_cn_score_index
      key_len: 458
          ref: NULL
         rows: 18
        Extra: Using index
*************************** 2. row ***************************
           id: 2
  select_type: UNION
        table: student
         type: index
possible_keys: NULL
          key: PRIMARY
      key_len: 4
          ref: NULL
         rows: 9
        Extra: Using index
*************************** 3. row ***************************
           id: NULL
  select_type: UNION RESULT
        table: <union1,2>
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: NULL
        Extra: 
3 rows in set (0.00 sec)
```

#### 4）DEPENDENT UNION

UNION中的第二个或后面的select语句，取决于外面的查询。

#### 5）UNION RESULT

UNION的结果。

#### 6）SUBQUERY

子查询中的第一个select。

#### 7）DEPENDENT SUBQUERY

子查询中的第一个select，取决于外面的查询。

#### 8）DERIVED

派生表的select, FROM子句的子查询 

#### 9）MATERIALIZED 

Materialized subquery 

#### 10）UNCACHEABLE SUBQUERY

一个子查询的结果不能被缓存，必须重新评估外链接的第一行。

A subquery for which the result cannot be cached and must be re-evaluated for each row of the outer query 

#### 11）UNCACHEABLE UNION 

 The second or later select in a [`UNION`](https://dev.mysql.com/doc/refman/5.7/en/union.html) that belongs to an uncacheable subquery (see `UNCACHEABLE SUBQUERY`) 

### 三、table

输出结果的来源名，可取值如下：

1. 表名，如下图中的 `t1`
2. `<unionM,N>` ：其中M,N表示id值，The row refers to the union of the rows with `id` values of *M* and *N*. 
3. `<derivedN>`：其中N表示id值，The row refers to the derived table result for the row with an `id` value of *N*. A derived table may result, for example, from a subquery in the `FROM` clause.
4. `<subqueryN>`：其中N表示id值，The row refers to the result of a materialized subquery for the row with an `id` value of *N*. 

```mysql
mysql> explain select * from (select * from ( select * from t1 where id=2602) a) b;
+--+-----------+----------+------+-----------------+-------+-------+----+----+-----+
|id|select_type|table     |type  |possible_keys    |key    |key_len|ref |rows|Extra|
+--+-----------+----------+------+-----------------+-------+-------+----+----+-----+
| 1|PRIMARY    |<derived2>|system|NULL             |NULL   |NULL   |NULL|  1 |     |
| 2|DERIVED    |<derived3>|system|NULL             |NULL   |NULL   |NULL|  1 |     |
| 3|DERIVED    |t1        |const |PRIMARY,idx_t1_id|PRIMARY|4      |    |  1 |     |
+--+-----------+----------+------+-----------------+-------+-------+----+----+-----+
```

### 四、type（==重要==）

表示MySQL在表中找到所需行的方式，又称访问类型。可选值由好到坏依次是：

#### 1）NULL

#### 2）system

表中只有一条数据。

#### 3）const

针对主键或唯一索引的等值查询扫描，最多只返回一行数据。const 查询速度非常快，因为它仅仅读取一次即可。 例如下面的这个查询, 它使用了主键索引, 因此 `type` 就是 `const` 类型的。==（高效）==

```mysql
mysql> explain select * from user_info where id = 2\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: user_info
   partitions: NULL
         type: const
possible_keys: PRIMARY
          key: PRIMARY
      key_len: 8
          ref: const
         rows: 1
     filtered: 100.00
        Extra: NULL
1 row in set, 1 warning (0.00 sec)
```

#### 4）eq_ref  

最多只返回一条符合条件的记录。使用唯一性索引或主键查找时会发生 ==（高效）== 

#### 5）ref

一种索引访问，它返回所有匹配某个单个值的行。此类索引访问只有当使用非唯一性索引或唯一性索引非唯一性前缀时才会发生。这个类型跟eq_ref不同的是，它用在关联操作只使用了索引的最左前缀，或者索引不是UNIQUE和PRIMARY KEY。ref可以用于使用=或<=>操作符的带索引的列。

#### 6）fulltext

#### 7）ref_or_null

#### 8）index_merge  

#### 9）unique_subquery  



#### 10）index_subquery  



#### 11）range

范围扫描，一个有限制的索引扫描。key 列显示使用了哪个索引。当使用=、 <>、>、>=、<、<=、IS NULL、<=>、BETWEEN 或者 IN 操作符,用常量比较关键字列时,可以使用 range 

#### 12）index

和全表扫描一样。只是扫描表的时候按照索引次序进行而不是行。

主要优点就是避免了排序, 但是开销仍然非常大。如在Extra列看到Using index，说明正在使用覆盖索引，只扫描索引的数据，它比按索引次序全表扫描的开销要小很多 

#### 13）ALL（==需要优化==）

最坏的情况，全表扫描。

A full table scan is done for each combination of rows from the previous tables. This is normally not good if the table is the first table not marked [`const`](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#jointype_const), and usually *very* bad in all other cases. Normally, you can avoid [`ALL`](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#jointype_all) by adding indexes that enable row retrieval from the table based on constant values or column values from earlier tables. 

### 五、possible_keys（==重要==）

显示查询使用了哪些索引，表示该索引可以进行高效地查找，

如果值为NULL，表示可以通过添加索引来提高查询性能。

通过命令 `show index from <表名>` 来查看表有哪些索引。

### 六、key

key列显示MySQL实际使用的索引。如果没有选择索引，键是NULL。

要想强制MySQL使用或忽视possible_keys列中的索引，在查询中使用FORCE INDEX、USE INDEX或者IGNORE INDEX。 

### 七、key_len

key_len列显示MySQL决定使用的键长度。如果键是NULL，则长度为NULL。使用的索引的长度。在不损失精确性的情况下，长度越短越好 。

- 字符串
  - char(n): n 字节长度
  - varchar(n): 如果是 utf8 编码, 则是 3 *n + 2字节; 如果是 utf8mb4 编码, 则是 4* n + 2 字节.
- 数值类型:
  - TINYINT: 1字节
  - SMALLINT: 2字节
  - MEDIUMINT: 3字节
  - INT: 4字节
  - BIGINT: 8字节
- 时间类型
  - DATE: 3字节
  - TIMESTAMP: 4字节
  - DATETIME: 8字节
- 字段属性: NULL 属性 占用一个字节，如果一个字段是 NOT NULL 的，则没有此属性，即key_len不再在原基础上加1 

### 八、ref

### 九、rows

rows列显示MySQL认为它执行查询时必须检查的行数。对于InnoDB来说，这是一个预估值。原则上rows越少越好。 

### 十、Extra（==重要==）

额外的信息 

#### 1）Using filesort（==需要优化==）

MySQL有两种方式可以生成有序的结果，通过排序操作或者使用索引 

#### 2）Using temporary （==需要优化==）

To resolve the query, MySQL needs to create a temporary table to hold the result. This typically happens if the query contains `GROUP BY` and `ORDER BY` clauses that list columns differently. 

#### 3）Not exists 

MYSQL优化了LEFT JOIN，一旦它找到了匹配LEFT JOIN标准的行， 就不再搜索了。 

#### 4）Using index 

说明查询是覆盖了索引的，不需要读取数据文件，从索引树（索引文件）中即可获得信息。

如果同时出现using where，表明索引被用来执行索引键值的查找，没有using where，表明索引用来读取数据而非执行查找动作。这是MySQL服务层完成的，但无需再回表查询记录。 

#### 5）Using index condition 

这是MySQL 5.6出来的新特性，叫做“索引条件推送”。简单说一点就是MySQL原来在索引上是不能执行如like这样的操作的，但是现在可以了，这样减少了不必要的IO操作，但是只能用在二级索引上。 

#### 6）Using where 

使用了WHERE从句来限制哪些行将与下一张表匹配或者是返回给用户。**注意**：Extra列出现Using where表示MySQL服务器将存储引擎返回服务层以后再应用WHERE条件过滤。 

#### 7）Using join buffer 

使用了连接缓存：**Block Nested Loop**，连接算法是块嵌套循环连接;**Batched Key Access**，连接算法是批量索引连接 

#### 8）impossible where 

where子句的值总是false，不能用来获取任何元组 

#### 9）select tables optimized away 

在没有GROUP BY子句的情况下，基于索引优化MIN/MAX操作，或者对于MyISAM存储引擎优化COUNT(*)操作，不必等到执行阶段再进行计算，查询执行计划生成的阶段即完成优化。 

#### 10）distinct 

优化distinct操作，在找到第一匹配的元组后即停止找同样值的动作 

### 十一、partition

分区。若表未分区则值为NULL。

### 十二、filtered

filtered: 表示此查询条件所过滤的数据的百分比 





