# SQL 语句   

a->b->c->d根据d中数据name=op找出a中的id，name   
```
SELECT DISTINCT a.*
FROM d
JOIN c ON d.`rule`=c.`rule`
JOIN a ON a.`id`= d.`uid`
JOIN b ON c.`roleid`=b.`id`
WHERE b.`role`='admin'
```

找出a中不包含所有权限b的id，name   
```
SELECT a.*
FROM d
JOIN c ON d.`rule`=c.`rule`
JOIN a ON a.`id`= d.`uid`
JOIN b ON c.`roleid`=b.`id`
GROUP BY a.`id`
HAVING COUNT(a.`id`) < (SELECT COUNT(*) FROM b)
```
