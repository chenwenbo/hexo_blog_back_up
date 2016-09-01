title: mysql不同数据类型比较是否用到索引？
date: 2016-05-25 15:12:48
tags:
---

mysql中int类型和字符串进行比较可以使用索引，但是字符串类型和int类型比较则不能用到索引。

```
CREATE TABLE `b` (
  `c1` int(11) NOT NULL DEFAULT '0',
  `c2` char(2) DEFAULT NULL,
  `c3` varchar(16) DEFAULT NULL,
  PRIMARY KEY (`c1`),
  KEY `c2` (`c2`) USING BTREE,
  KEY `c3` (`c3`)
) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```

```
SELECT * FROM b where c2 = 1;   ----没有用到索引
SELECT * FROM b where c1 = '1';   ----用到索引
```

