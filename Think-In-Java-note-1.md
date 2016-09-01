title: Thinking In Java 学习笔记——持有对象
date: 2016-08-31 11:06:34
tags:
---

通常我们只有在程序运行时才会知道我们需要创建多少个对象，在此之前我们并不知道其对象的数量和类型，为了解决这个问题我们引入了容器的概念。

## 容器

### 数组

在Java中我们可以通过数组来保存对象的一组引用，但是数组初始化的时候需要指定容量的大小，但是更多的情况下我们并不知道我们会产生多少个对象，所以使用数组并不能解决我们大多数的问题。 

### 集合
Java的类库中提供了一套相对完整的容器类完成这件事情，其中的基本类型包括List Set Queue Map.这些对象类型被称之为集合类，但是Java类库中使用了Collection这个名字来代替一个特殊子集，所以我们通常用“容器”来称呼他们。

容器类还有一些其他的特性，Set每个值只能保存一个对象，Map允许你将对象和对象关联起来形成关联数组,ArrayList可以自动扩容。

## 泛型和类型安全的容器
Jave SE5之前我们还没有引入泛型的概念，所以会导致类型安全的问题。使用泛型的最大好处就是在编译期间可以进行类型检查，避免了很多的类型转换错误。

### 基本概念
Java容器类的用途是“保存对象”，可以划分为两个不同的概念。

1. Collection. 一个独立元素的序列，这些元素遵循一条或者多条的规则，List必须按照插入顺序进行保存，Set不能有重复的元素，Queue按照队列的规则来确定对象产生的顺序（先进先出）.

2. Map. 一组成对的“键值对”对象，允许你用键来进行查询值。ArrayList允许你用数字下标来进行查找。映射表允许我们使用一个对象来查询另一个对象，它们被称为“关联数组”，因为它们将两个对象关联在一起，所以通常可以称呼其“字典”。

我们通常建议在定义容器类的时候使用接口：List Set Map等，但是在实现的时候制定其实现类。（针对接口编程）
特殊情况：如果我们要使用仅在其实现类中的方法的时候，我们就必须声明的时候就指定其实现类。LinkedList TreeMap中都有其独特的方法。

### List
List承诺可以将元素维护在特定的序列中，List接口在Collection的基础上添加了大量的方法，使得可以在List的中间插入和移除元素。

有两种类型的List:

* ArrayList 长于随机访问元素，但是插入和移除非常慢。因为底层的实现是顺序排列的，插入和移除之后所有所有的元素都要跟着变动
* LinkedList 它通过代价较低的在List中间插入和删除元素，提供了优化顺序的访问。LinkedList在随机访问方面比较慢，但是特性要比ArrayList更大。


### 迭代器

不同的容器类取出元素的方法都会有所区别，在我们修改容器类的实现的时候，这个时候可能就会修改很多取容器中元素的代码，但是如果我们能有统一的标准去拿到容器类中的元素，我们就可以省去修改取元素部分的代码了。

迭代器的概念可以达到此目的。迭代器是一个对象，他的工作是遍历并选择序列中的对象，而客户端程序员并不需要他的底层结构，此外迭代器通常被称为轻量级对象；创建它的代价小。但是经常可以看到迭代器上有一些奇怪的限制。例如，Java的Iterator只能单向移动，这个Iterator只能用来：

* 使用Iterator()方法返回一个Iterator。Iterator准备返回序列的第一个元素
* 使用next()或者序列的下一个元素
* 使用hasNext()检查是否存在下一个元素
* 使用remove将迭代器新近返回的元素删除

### LinkedList

LinkedList像ArrayList一样实现了List接口，但是他执行的某些操作（插入和删除）与ArrayList相比更加高效，但在随机访问操作方面要逊色一些。
LinkedList还添加了可以使其用作栈、队列、或双向队列的方法。（实现了Queue接口）

### Set

Set不保存重复的元素，Set具有和Collection相同的接口，因此没有任何额外的功能，不想之前的ArrayList LinkedList，实际上Set就是Collection，只是行为不同。
* HashSet使用散列函数
* TreeSet讲元素存储在红-黑树数据结构中
* LinkedHashSet因为查询速度的原因也使用了散列，但是它使用了链表来维护插入的顺序

### Map

### Queue

## 总结

使用接口描述的一个理由是它可以使我们能够创建更通用的代码，通过针对接口编程我们的代码可以应用与更多的对象类型，在Java中讲COllection和Iterator()绑定到了一起，如果你实现了Collection意味着你必须提供itarator()方法。