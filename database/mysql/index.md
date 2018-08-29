# MySQL 索引

## 索引是什么

## 索引的作用

1. 过滤 filter
2. 排序或分组 sort/group
3. 覆盖 cover

## 怎么创建索引

## 最左前缀匹配

1.范围查询中断继续索引匹配：
有一个联合索引:
KEY `user_product_detail_index` (`user_id`, `product_name`, `productor`)
不过此查询语句 WHERE user_id < 3 AND product_name = 'p1' AND productor = 'WHH' 中, 因为先进行 user_id 的范围查询, 而根据 最左前缀匹配 原则, 当遇到范围查询时, 就停止索引的匹配, 因此实际上我们使用到的索引的字段只有 user_id

2.排序查询也可以命中索引：
我们的索引是KEY `user_product_detail_index` (`user_id`, `product_name`, `productor`)
但是上面的查询中根据 product_name 来排序, 因此不能使用索引进行优化, 进而会产生 Using filesort.
如果我们将排序依据改为 ORDER BY user_id, product_name, 那么就不会出现 Using filesort 了

## 索引优化

## 索引数据结构


