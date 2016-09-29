title: Java类和对象的初始化
date: 2016-09-29 11:15:10
tags:
---


> 每个Java程序执行之前都必须要经历类的加载、连接、初始化过程

**类的加载**

加载是将编译后的Java类文件（.class文件）中的二进制数据读入内存，并将其放在运行时数据区的方法区内，然后在堆区创建一个java.lang.class对象，用来封装在方法区的数据结构。即加载后最终得到的是Class对象。（该对象是单例，JVM保证线程安全性）

类的加载途径：Class.forName("class full path") 、实例对象.class（属性）、实例对象getclass()

**类的连接**

	-验证：确保被加载类的正确性
	-准备：为类的静态变量分配内存，并将其初始化为默认值
	-解析：把类中符号引用转换为直接引用

**类的初始化**

为类的静态变量赋予正确的初始值

### 对象的初始化

在Java中，一个对象在可以被使用之前必须要被正确地初始化，这一点是Java规范规定的。

**对象创建的方法**

* new 显式创建
* Class.forName(path).newInstance()
* String字面量类的隐式创建


### 类初始化的先决条件

**主动使用**

* 创建某个类的新实例 （new/reflect/clone/serialization）
* 调用类的静态方法
* 访问类的静态变量，或者对该变量进行赋值
* 反射 如：Class.forName("com.java.Shape")
* 初始化某个子类
* 虚拟机中启动某个含有main方法的启动类。

**被动使用**
ClassLoader的loadClass方法会加载一个类，但是不会初始化该类

### 对象实例化的两种方式

* new 
* Class.forName(class full path).newInstance()

**区别**
new : 强引用、高效、能调用有参构造器
newInstance ：弱引用、低耦合、仅能调用构造器

使用newInstance之前必须对类进行**装载**和**连接**的操作。
Class.forName 和 ClassLoad.loadClass的作用就是对类进行装载和连接。

forName 和 loadClass方法的区别
ClassLoader的loadClass方法会加载一个类，但是不会初始化该类
而forName方法则会初始化该类。

### JDBC驱动类的加载
我们在加载JDBC驱动类的时候会用到这样一段代码
```
Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
```
这段代码的作用就是加载、连接、初始化Driver类。

为什么不用ClassLoad.loadClass方法？

因为loadClass方法并不会初始化该类，只会对其进行装载和连接，而JDBC的规范是需要初始化DriverManager.而此过程就在Driver类的静态代码块中，只有类的初始化才做才会加载静态代码块。所以此处只能用Class.forName进行处理。

> [Java对象初始化详解](http://blog.jobbole.com/23939/)
> [Java类的初始化顺序](http://liujiacai.net/blog/2014/07/12/order-of-initialization-in-java/)