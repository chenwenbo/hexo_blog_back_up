title: Spring系列学习笔记-IOC
date: 2016-07-05 10:14:54
tags:
---

> [码农翻身]（http://mp.weixin.qq.com/s?__biz=MzAxOTc0NzExNg==&mid=2665513179&idx=1&sn=772226a5be436a0d08197c335ddb52b8&scene=4#wechat_redirect）

对象的创建
=====================

在java中我们通常创建对象的方式是通过new的方式来进行创建，这样在大型系统中会造成严重的耦合。

例如我们有一个水果类Fruit,我们需要根据不同的业务来进行不同的实例化。我们代码可能会这样写：
```
Fruit f = null;
if("apple".equals(type)){
	f = new Apple();
} else if ("banana".equals(typr)){
	f = new Banana();
}
```

但是这样的代码如果散落到地方，会很难维护，如果我们增加了一个其他的子类。这个时候绝对是灾难。

使用工厂模式进行改进 （封装变化）

```
public class FruitFactory{
	
	public static Fruit create(String type){
		if("apple".equals(type)){
			f = new Apple();
		} else if ("banana".equals(typr)){
			f = new Banana();
		}
	}
}
```


解除依赖
============
我们的业务类中经常会依赖很多其他的业务类，其他业务类实例化都需要在该类中进行显式的实例化，如果其中包含一些与数据库连接或者http相关的服务，则该类在进行测试时会造成巨大的效率问题。

我们的解决方法通常是Mock掉这些耗费资源的操作。但是这就必须要求我们依赖接口，而不是实现编程。我们可以模拟一个Mock的service.去实现该接口。

然后通过set方法进行注入。

但是这种方法还是需要讲代码散落在各个地方。


声明式注入
============
在java中，描述对象最合适的一种方式就是通过xml了。
通过xml中对bean的声明，我们可以讲所有的sevice之间的关系都通过xml进行定义。
这样就可以讲所有的业务依赖在同一个地方进行维护，不用分散在各地了。
至此我们就解决了依赖注入的问题了。

![](http://ww4.sinaimg.cn/large/8a0ce11egw1f5iuzrc2dgj20hs06cgmj.jpg)

XML处理过程
============
1. 解析xml，获取各种元素
2. 通过java反射，创建bean,维护各个bean的依赖关系。

Spring处理方式
==============
Spring的处理方式和上面的类似，但是有更多的细节处理，例如通过setter方法注入，构造函数注入，inin方法 destory方法等。

![](http://ww4.sinaimg.cn/large/8a0ce11egw1f5iuzd2n9jj20e10dxt9h.jpg)

对象创建都有Spring来进行控制，那么Spring中就可以做很多操作了。
AOP就是由此延伸而来。