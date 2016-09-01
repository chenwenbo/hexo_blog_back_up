title: 用图表展示java String 对象的不变性
date: 2016-05-10 18:28:09
tags:
---

这里有一组图表阐述了java String 对象的 不变性。

1. String 的声明
```
String s = "abcd";
```
s 储存着String对象的引用，下面的箭头应该解释为"内存的引用"
![](http://ww1.sinaimg.cn/large/8a0ce11egw1f3qgwd1fjaj20ic0b4aag.jpg)     

2. 用一个String变量给另一个String变量赋值
```
String s2 = s;
```
因为是同样的对象，所以s2存储的是同样的内存引用
![](http://ww4.sinaimg.cn/large/8a0ce11egw1f3qgyb3f01j20ic0b4mxq.jpg)

3.  字符串的连接
```
s = s.contact("abc");
```
s现在存储的是重新被创造出来的对象引用。
![](http://ww4.sinaimg.cn/large/8a0ce11egw1f3qgyioqfej20i207rgm4.jpg)

### 总结：
一旦String对象在内存（heap区） 中被创建了。它就不能改变，
所以我们应该注意，一旦有String的方法想改变String.
我们还不如重新创造一个String对象。

如果我们需要创建可变的String对象。 我们可以使用 StringBuffer 或者 StringBuilder. 否则，很多时间可能被浪费在垃圾回收上。因为每一次都会有一个新的对象产生。

### 延伸阅读：
[StringBuilder](http://www.programcreek.com/2011/11/java-convert-a-file-into-a-string/)  
[jvm内存](http://www.programcreek.com/2013/04/jvm-run-time-data-areas/)  
