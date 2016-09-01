title: 对final关键字的理解
date: 2016-01-19 10:26:28
tags: JAVA
categories: 编程
---
根据程序上下文环境，Java关键字final有“这是无法改变的”或者“终态的”含义，它可以修饰非抽象类、非抽象类成员方法和变量。你可能出于两种理解而需要阻止改变：设计或效率。
* final类不能被继承，没有子类，final类中的方法默认是final的。
* final方法不能被子类的方法覆盖，但可以被继承。
* final成员变量表示常量，只能被赋值一次，赋值后值不再改变。
* final不能用于修饰构造方法。
注意：父类的private成员方法是不能被子类方法覆盖的，因此private类型的方法默认是final类型的。
这个时候我看到了这样一段代码：
```
private static final Map<String,Operation> stringEnum = new HasmMap<String,Operation>();

static{
	
	for(Operation op : values()){
		stringEnum.put(op.toString(),op);
	}
}

public static Operation fromString(String symbol){
	return stringEnum.get(symbol);
}


```
解释如下：
Because final marks the reference, not the object. You can't make that reference point to a different hash table. But you can do anything to that object, including adding and removing things.

Your example of an int is a primitive type, not a reference. Final means you cannot change the value of the variable. So, with an int you cannot change the value of the variable, e.g. make the value of the int different. With an object reference, you cannot change the value of the reference, i.e. which object it points to.

大概意思就是：针对对象而言，final修饰的只是对象的引用。  而针对原始数据类型，就是直接起作用了。


> [相关文章](http://www.cnblogs.com/EvanLiu/archive/2013/06/13/3134776.html)

