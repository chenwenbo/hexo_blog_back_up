title: Java类加载和初始化
date: 2016-06-30 14:29:15
tags:
---

今天在看到class对象和实例对象的相关文章的时候，注意到文中提到的一点，class的加载是懒加载的，需要的时候才进行加载。
然后发现我对java的类加载和初始化并不是特别的清楚，所以这里总结一下。

> [本文参考](http://www.importnew.com/6579.html)

### 类什么时候被加载
类加载是通过类加载器（Classloader）完成的，它既可以是饿汉式[eagerly load]（只要有其它类引用了它就加载），也可以是懒加载[lazy load]（等到类初始化发生的时候才加载）.
这可能跟不同的jvm实现相关，然而它又是受JLS（Java Language Specification：java语言规范）保证的（当有静态初始化需求的时候才被加载）。

### 类什么时候初始化
加载完类之后，类的初始化就发生，意味着它会初始化所有类静态成员。一下情况类将初始化：

* 实例通过使用new()关键字创建或者使用class.forName()反射，但它有可能导致ClassNotFoundException。
* 类的静态方法被调用
* 类的静态域被赋值
* 静态域被访问，而且它不是常量

```
public class StaticConstant {
	
	public static final int a = 1; //常量，不会被初始化
	public static int b = 1; //非常量，会被初始化
	static {
		System.err.println("初始化");
	}
	
}
```
* 在顶层类中执行assert语句
* 反射

JLS严格声明：一个类不会被任何除以上方法之外的原因被初始化。

### java类初始化顺序
* 类初始化（类加载）
* 对象初始化
* 先父后子
* 从上而下

### 