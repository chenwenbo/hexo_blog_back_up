title: sql优化总结
date: 2016-01-19 10:27:09
tags: SQL
categories: 数据库
---

### 总结： 
在数据量不大的情况下，是否对sql进行优化其实对我们的查询并没有太大的影响， 但是我们应当养成良好的习惯，以便应对大数据量的情况。
查询优化无非就是对索引的处理，怎么样的情况下才能够利用索引优化的查询。下面总结了很多有用的方法。深入学习sql优化还是得对数据结构这一块的只是有一定的了解，所以数据结构还是必须得学啊。。。。

### where优化指南
1.对查询进行优化，尽量避免全表扫描，首先应考虑在where及order by 涉及的列上进行索引。

2.尽量避免在where子句对字段进行null判断， where a.num is null.此种情况会对全表进行扫描，效率非常低，我们通常的做法可以讲字段的默认值设置为0或者”“,(""情况待考究)

3.应尽量避免对where子句中使用！=或<>操作符，否则将引擎放弃使用索引而进行全表扫描。

4.应尽量避免在where子句中使用or来连接条件，否则将导致引擎放弃使用索引而进行全表扫表。

5.in and not in 要谨慎使用，否则in只包含数据量过大时会导致全表扫描，
使用in的情况下，可以修改为使用exist来进行查询，会走索引，
 
```
select num from a where num in(select num from b);
```
用下面的语句替换：
```
select num from a where exists(select 1 from b where num=a.num);
```
6.使用 like%xxx%这种情况会进行全表扫描，索引不会起作用， 但是使用一个%则会利用索引进行优化查询。

7.在进行区间查询的时候尽量使用between on这种表达式，这样会用到索引进行查询。

```
select id from t where num in(1,2,3);
```
对于连续的数值，能用 between 就不要用 in 了：
 
```
select id from t where num between 1 and 3;
```
8.应尽量避免在where子句对查询的左边字段进行表达式或者函数操作，这样DB会放弃索引而进行全表扫描
 
```
select id from t where num/2=100;
```
可以这样查询：
```
select id from t where num=100*2;
select id from t where substring(name,1,3)=&39;abc&39;;name 以 abc 开头的 id
```
应改为：
```
select id from t where name like &39;abc%&39;;
```
9.不要在 where 子句中的“=”左边进行函数、算术运算或其他表达式运算，否则系统将可能无法正确使用 索引。

10.在使用索引字段作为条件时，如果该索引是复合索引，那么必须使用到该索引中的第一个字段作为条件才能保证系统使用该索引， 否则该索引将不会被使用， 并且应尽可能的让字段顺序与索引顺序相一致。
```
cerate index(A,B);
select * from t where A="xx"; select * from t where A="xx" and B="";  会用到索引
select * from t where B="xx"; select * from t where B="xx" and A="";  不会用到索引
```
11.并不是所有索引对查询都有效，SQL 是根据表中数据来进行查询优化的，当索引列有大量数据重复时， SQL查询可能不会去利用索引，如一表中有字段“性别” ,male、female几乎各一半，那么即使在“性别”上建了索引也对查询效率起不了作用。  

12.索引并不是越多越好，索引固然可以提高相应的 select 的效率，但同时也降低了 insert 及 update 的效率，因为 insert 或 update 时有可能会重建索引，所以怎样建索引需要慎重考虑，视具体情况而定。一个表的索引数最好不要超过6个，若太多则应考虑一些不常使用到的列上建的索引是否有必要。

13.任何地方都不要使用 select * from t ,用具体的字段列表代替“*”,不要返回用不到的任何字段
14.尽量把所有的列设置为 NOT NULL,如果你要保存 NULL,手动去设置它，而不是把它设为默认值。

---

补充：
1. 在海量查询时尽量少用格式转换。
2. ORDER BY 和 GROPU BY:使用 ORDER BY 和 GROUP BY 短语，任何一种索引都有助于 SELECT 的性能提高。
3. 任何对列的操作都将导致表扫描，它包括数据库教程函数、计算表达式等等，查询时要尽可能将操作移至等号右边。
4. IN、OR 子句常会使用工作表，使索引失效。如果不产生大量重复值，可以考虑把子句拆开。拆开的子 句中应该包含索引。
5. 只要能满足你的需求，应尽可能使用更小的数据类型：例如使用 MEDIUMINT 代替 INT
6. 尽量把所有的列设置为 NOT NULL,如果你要保存 NULL,手动去设置它，而不是把它设为默认值。
7. 尽量少用 VARCHAR、TEXT、BLOB 类型（可能的时候用数值型）
8. 如果你的数据只有你所知的少量的几个。最好使用 ENUM 类型
9. 建立索引。
10. 合理用运分表与分区表提高数据存放和提取速度。
