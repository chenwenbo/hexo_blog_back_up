title: union/union all
date: 2016-01-18 09:42:30
tags:
---

> 在数据库中，**union**和**union all**关键字都是将两个结果集合并为一个，但这两者从使用和效率上来说都有所不同。

## union

union在进行表链接后会筛选掉重复的记录，所以在表链接后会对所产生的结果集进行排序运算，删除重复的记录再返回结果。如： 
```sql
 select * from test_union1 
   union 
 select * from test_union2 
```
这个SQL在运行时先取出两个表的结果，再用排序空间进行排序删除重复的记录，最后返回结果集，如果表数据量大的话可能会导致用磁盘进行排序。 

## union all

而union all只是简单的将两个结果合并后就返回。这样，如果返回的两个结果集中有重复的数据，那么返回的结果集就会包含重复的数据了。 
     从效率上说，union all要比union快很多，所以，如果可以确认合并的两个结果集中不包含重复的数据的话，那么就使用union all，如下： 

```sql
select * from test_union1 
union all 
select * from test_union2 
```

使用 union 组合查询的结果集有两个最基本的规则：

 1. 所有查询中的列数和列的顺序必须相同
 2. 据类型必须兼容

---
![](http://ww4.sinaimg.cn/large/8a0ce11egw1f03erkkcfhj20i009z75t.jpg)
![](http://ww1.sinaimg.cn/large/8a0ce11egw1f03es7p346j20ig0a6q4j.jpg)
![](http://ww3.sinaimg.cn/large/8a0ce11egw1f03eshgw3lj20gm07igmd.jpg)
![](http://ww3.sinaimg.cn/large/8a0ce11egw1f03esmq0m4j20hx07j3zi.jpg)
