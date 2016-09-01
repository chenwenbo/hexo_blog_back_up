title: MySQL中EXPLAIN命令详解
date: 2016-05-12 09:06:19
tags:
---
EXPLAIN显示了MySQL如何使用索引来处理SELECT语句以及连接表。可以帮助选择更好的索引和写出更优化的查询语句。

使用方法，在select语句前加上EXPLAIN就可以了：

如：
```
EXPLAIN SELECT `surname`,`first_name` FORM `a`,`b` WHERE `a`.`id`=`b`.`id` 
```
EXPLAIN列的解释：

![](http://ww4.sinaimg.cn/large/8a0ce11egw1f3tj9zcycwj20pp0edtcn.jpg)

![](http://ww2.sinaimg.cn/large/8a0ce11egw1f3tjagtwqnj20ps0oidnd.jpg)