title: Integer常量池
date: 2016-01-19 10:26:21
tags: JAVA
categories: 编程
---
 
知识点：基本数据类型拆箱装箱机制，缓冲池。
在本文中，我们将研究一下Integer类和Integer常量池以及Integer常量池的用途？

### Integer类
Integer 类在对象中包装了一个基本类型 int 的值。Integer 类型的对象包含一个 int 类型的字段。
此外，该类提供了多个方法，能在 int 类型和 String 类型之间互相转换，还提供了处理 int 类型时非常有用的其他一些常量和方法。

在深入之前请猜一下下面代码的结果。如果你能完全回答正确那说明你已经知道Java Integer常量池的工作机制了。
```    
[java] view plaincopy

public class IntegerClassExampleOne {  
  public static void main(String[] javalatte) {  
  Integer i = new Integer(555);  
  Integer j = new Integer(555);  
  if(i==j){  
  System.out.println("i==j is equal");  
  }else {  
  System.out.println("i==j is not equal");  
  }  
   }  
 }  
   
 public class IntegerClassExampleTwo {  
   public static void main(String[] javalatte) {  
   Integer i = 500;  
   Integer j = new Integer(500);  
   if(i==j){  
   System.out.println("i==j is equal");  
   }else {  
   System.out.println("i==j is not equal");  
   }  
   }  
 }  
 public class IntegerClassExampleThree {  
   public static void main(String[] javalatte) {  
   Integer i = 128;  
   Integer j = 128;  
   if(i==j){  
   System.out.println("i==j is equal");  
   }else {  
   System.out.println("i==j is not equal");  
   }  
   }  
 }  
   
 public class IntegerClassExampleFour {  
   public static void main(String[] javalatte) {  
   Integer i = 127;  
   Integer j = 127;  
   if(i==j){  
   System.out.println("i==j is equal");  
   }else {  
   System.out.println("i==j is not equal");  
   }  
   }  
 }  
```

你可以在本文的后面找到答案。如果你的答案不正确，请继续阅读本文，找出Integer类具有这种不常见方法的原因。

Integer i = 10 和 Integer j = new Integer(10) 的区别
```
[java] view plaincopy

public class IntegerClassDemo {  
  public static void main(String[] javalatte) {  
  Integer i = 10;  
  Integer j = new Integer(10);  
  Integer k = 145;  
  Integer p = new Integer(145);  
  }  
}  
```

在编译了上面这个类之后，我尝试着反编译这个类的Class文件。我得到了下面这个类
```
[java] view plaincopy

public class IntegerClassDemo  
{  
public static void main(String[] javalatte)  
{  
Integer i = Integer.valueOf(10);  
Integer j = new Integer(10);  
Integer k = Integer.valueOf(145);  
Integer p = new Integer(145);  
}  
}  
```


所以当我们用Integer i = 10的方式创建一个Integer类时，Java调用了方法Integer.valueOf()。

### Integer.valueOf()

让我们来看看这个函数做了什么。JDK 文档是这样说的：
```
public static Integer valueOf(int i)
```
返回一个表示指定的 int 值的 Integer 实例。如果不需要新的 Integer 实例，则通常应优先使用该方法，而不是构造方法 Integer(int)，因为该方法有可能通过缓存经常请求的值而显著提高空间和时间性能。

这个方法总是缓存了-128到127之间的值，还有，它也可以缓存这个范围之外的值。
为了说明这个问题，让我们来看看Integer.valueOf()的源代码。
```
[java] view plaincopy

public static Integer valueOf( int i) {  
    assert IntegerCache. high >= 127;  
    if (i >= IntegerCache. low && i <= IntegerCache. high )  
        return IntegerCache. cache[i + (-IntegerCache. low)];  
    return new Integer(i);  
}  
```

通过上面的代码，我们可以很清楚的看到，当我们在-128到127这个范围内调用valueof()这个函数时，实际上使用的是缓存（IntegerCache）里面的值。
IntegerCache是帮助我们缓存Integer的类。缓存数值的大小是由-XX:AutoBoxCacheMax= <size>来控制，或者可以通过系统属性来获得：-Djava.lang.Integer.IntegerCache.high=<size>

换句话来说，当我们通过以下方式创建Integer类：
```
Integer i = 10;
Integer j = new Integer(10);
```
会获得在缓存池上相同的实例。

所以如果值在-128到127之间，你使用valueof()将获得相同的引用，超过这个范围将返回new Integer(int)。因为是相同的引用，在-128到127范围内你的==操作是有用的。

Java缓存了-128到127范围内的Integer类。所以当你给这个范围的封装类赋值时，装箱操作会调用Integer.valueOf() 函数，反过来就会给封装类赋上一个缓存池实例的引用。
另一方面来说，如果用超出这个范围的值给封装类赋值时，Integer.valueOf()会创建一个新的Integer类。因此，比对超出这个范围的Integer类时，会返回false。

我希望你们已经知道Integer常量池的精髓了。再看看上面你曾经猜测输出的代码，这次你能正确的回答了。

Integer常量池概要：
答案：
```
IntegerClassExampleOne
i==j is not equal

IntegerClassExampleTwo
i==j is not equal

IntegerClassExampleThree
i==j is not equal

IntegerClassExampleFour
i==j is equal
```
> [引用链接](http://blog.csdn.net/womenghuangliang/article/details/17377285)
