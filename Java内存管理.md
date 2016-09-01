title: Java内存管理
date: 2016-05-11 16:11:01
tags:
---

## 1.简介
 
现摘录一段Java5内存管理白皮书中的一段话：
> One strength of the Java™ 2 Platform, Standard Edition (J2SE™) is that it performs automatic memory
management, thereby shielding the developer from the complexity of explicit memory management.
 
由上面我们可以知道，Java平台采用了自动的内存管理方式，那么JVM是如何进行内存管理的呢？
 
Java的内存管理实际上包含两个方面：
1. Java内存分配
2. Java的内存回收
 
搞清楚了这两个问题，Java的内存管理就清楚了。
 
写这篇文章之前，问了自己一个问题：
 
Java采用了自动的内存管理方式，程序员实际上不用关心内存管理的细节，那为什么我们仍然需要了解Java内存管理的内幕？

原因有三：
1. 了解Java内存管理的细节，有助于程序员编写出性能更好的程序。比如，在新的线程创建时，JVM会为每个线程创建一个专属的栈（stack），其栈是先进后出的数据结构，这种方式的特点，让程序员编程时，必须特别注意递归方法要尽量少使用，另外栈的大小也有一定的限制，如果过多的递归，容易导致stack overflow。
2. 了解Java内存管理的细节，一旦内存管理出现问题，有助于找到问题的根本原因所在。
3. 了解Java内存管理的内幕，有助于优化JVM，从而使得自己的应用获得最佳的性能体验。
 
JVM的体系结构如下：
如下图所示，JVM的体系结构包含几个主要的子系统和内存区：
类装载子系统 ，负责把类从文件系统中装入内存
GC子系统 ，垃圾收集器的主要工作室自动回收不再运行的程序引用对象所占用的内存，此外，它还可能负责那些还在使用的对象，以减少的堆碎片。
内存区 ，用于存储字节码，程序运行时创建的对象，传递给方法的参数，返回值，局部变量和中间计算结果。
执行引擎： 
1、最简单的：一次性解释字节码。 
2、快，但消耗内存的：“即时编译器”，第一次被执行的字节码会被编译成机器代码，放入缓存，以后调用可以重用。 
3、自适应优化器，虚拟机开始的时候会解释字节码，但是会监视运行中程序的活动，并记录下使用最频繁的代码段。程序运行的时候，虚拟机只把使用最频繁的代码编译成本地代码，其他的代码由于使用的并不频繁，继续保留为字节码--由虚拟机继续解释他们。一般可以使java虚拟机80%~90%的时间里执行被优化过的本地代码，只需要编译10%~20%对性能优影响的代码。 
4、由硬件芯片组成，他用本地方法执行java字节码，这种执行引擎实际上是内嵌在芯片里的。
 ![](http://ww2.sinaimg.cn/large/8a0ce11egw1f3riks0ppmj20l50gjade.jpg)

 
## 2. Java的内存分配
 
在Java程序运行过程中，JVM定义了各种区域用于存储运行时数据。其中的有些数据区域在JVM启动时创建，并只在JVM退出时销毁。其它的数据区域与每个线程相关。这些数据区域，在线程创建时创建，在线程退出时销毁。
 
### 2.1 程序计数器寄存器（The pc Register）
 
JVM支持多个线程同时运行。每个JVM都有自己的程序计数器。在任何一个点，每个JVM线程执行单个方法的代码，这个方法是线程的当前方法。如果方法不是native的，程序计数器寄存器包含了当前执行的JVM指令的地址，如果方法是 native的，程序计数器寄存器的值不会被定义。 JVM的程序计数器寄存器的宽度足够保证可以持有一个返回地址或者native的指针。
 
### 2.2 栈
 
1）栈与线程
 
JVM是基于栈的虚拟机.JVM为每个新创建的线程都分配一个栈.也就是说,对于一个Java程序来说，它的运行就是通过对栈的操作来完成的。栈以帧为单位保存线程的状态。JVM对栈只进行两种操作:以帧为单位的压栈和出栈操作。

我们知道,某个线程正在执行的方法称为此线程的当前方法.我们可能不知道,当前方法使用的帧称为当前帧。当线程激活一个Java方法,JVM就会在线程的 Java堆栈里新压入一个帧。这个帧自然成为了当前帧.在此方法执行期间,这个帧将用来保存参数,局部变量,中间计算过程和其他数据.这个帧在这里和编译原理中的活动纪录的概念是差不多的.

从Java的这种分配机制来看,堆栈又可以这样理解:栈(Stack)是操作系统在建立某个进程时或者线程(在支持多线程的操作系统中是线程)为这个线程建立的存储区域，该区域具有先进后出的特性。
 
2）栈中的方法调用
 
嵌套方法的出栈和入栈示意图：
 ![](http://ww4.sinaimg.cn/large/8a0ce11egw1f3rilixcx1j209u0cet9c.jpg)
 
上图中描述了嵌套方法时，stack的内存分配图，由上面可以知道，当嵌套方法调用时，嵌套越深，stack的内存就越晚才能释放，因此，在实际开发过程中，不推荐大家使用递归来进行方法的调用，递归很容易导致stack flow。
 
非嵌套方法的出栈入栈过程：
![](http://ww2.sinaimg.cn/large/8a0ce11egw1f3rim17vpaj20jm0cvdgq.jpg)
 
### 2.3 堆

每一个Java应用都唯一对应一个JVM实例，每一个实例唯一对应一个堆。应用程序在运行中所创建的所有类实例或数组都放在这个堆中,并由应用所有的线程共享.跟C/C++不同，Java中分配堆内存是自动初始化的。Java中所有对象的存储空间都是在堆中分配的，但是这个对象的引用却是在堆栈中分配,也就是说在建立一个对象时从两个地方都分配内存，在堆中分配的内存实际建立这个对象，而在堆栈中分配的内存只是一个指向这个堆对象的指针(引用)而已。
 
### 2.4 堆和栈的区别

 **1. 栈(stack)与堆(heap)都是Java用来在Ram中存放数据的地方 。与C++不同，Java自动管理栈和堆，程序员不能直接地设置栈或堆。 **

** 2. 栈的优势是，存取速度比堆要快 ，仅次于直接位于CPU中的寄存器。但缺点是，存在栈中的数据大小与生存期必须是确定的，缺乏灵活性。另外，栈数据可以共享，详见第3点。堆的优势是可以动态地分配内存大小，生存期也不必事先告诉编译器，Java的垃圾收集器会自动收走这些不再使用的数据。但缺点是，由于要在运行时动态分配内存，存取速度较慢。 **

** 3. Java中的数据类型有两种: **

一种是基本类型(primitive types), 共有8种，即int, short, long, byte, float, double, boolean, char(注意，并没有string的基本类型)。这种类型的定义是通过诸如int a = 3; long b = 255L;的形式来定义的，称为自动变量。值得注意的是，自动变量存的是字面值，不是类的实例，即不是类的引用，这里并没有类的存在。如int a = 3; 这里的a是一个指向int类型的引用，指向3这个字面值。这些字面值的数据，由于大小可知，生存期可知(这些字面值固定定义在某个程序块里面，程序块退出后，字段值就消失了)，出于追求速度的原因，就存在于栈中。
另外，栈有一个很重要的特殊性，就是存在栈中的数据可以共享。假设我们同时定义： 
```
int a = 3; 
int b = 3；
```
编译器先处理int a = 3；首先它会在栈中创建一个变量为a的引用，然后查找有没有字面值为3的地址，没找到，就开辟一个存放3这个字面值的地址，然后将a指向3的地址。接着处理int b = 3；在创建完b的引用变量后，由于在栈中已经有3这个字面值，便将b直接指向3的地址。这样，就出现了a与b同时均指向3的情况。 
特别注意的是，这种字面值的引用与类对象的引用不同。假定两个类对象的引用同时指向一个对象，如果一个对象引用变量修改了这个对象的内部状态，那么另一个对象引用变量也即刻反映出这个变化。相反，通过字面值的引用来修改其值，不会导致另一个指向此字面值的引用的值也跟着改变的情况。如上例，我们定义完a与b的值后，再令a=4；那么，b不会等于4，还是等于3。在编译器内部，遇到a=4；时，它就会重新搜索栈中是否有4的字面值，如果没有，重新开辟地址存放4的值；如果已经有了，则直接将a指向这个地址。因此a值的改变不会影响到b的值。 

另一种是包装类数据 ，如Integer, String, Double等将相应的基本数据类型包装起来的类。这些类数据全部存在于堆中，Java用new()语句来显示地告诉编译器，在运行时才根据需要动态创建，因此比较灵活，但缺点是要占用更多的时间。
 
** 4. String是一个特殊的包装类数据 。 **

即可以用String str = new String("abc");的形式来创建，也可以用String str = "abc"；的形式来创建(作为对比，在JDK 5.0之前，你从未见过Integer i = 3;的表达式，因为类与字面值是不能通用的，除了String。而在JDK 5.0中，这种表达式是可以的！因为编译器在后台进行Integer i = new Integer(3)的转换)。前者是规范的类的创建过程，即在Java中，一切都是对象，而对象是类的实例，全部通过new()的形式来创建。Java中的有些类，如DateFormat类，可以通过该类的getInstance()方法来返回一个新创建的类，似乎违反了此原则。其实不然。该类运用了单例模式来返回类的实例，只不过这个实例是在该类内部通过new()来创建的，而getInstance()向外部隐藏了此细节。那为什么在String str = "abc"；中，并没有通过new()来创建实例，是不是违反了上述原则？其实没有。 

** 5. 关于String str = "abc"的内部工作。 **

Java内部将此语句转化为以下几个步骤： 
(1)先定义一个名为str的对String类的对象引用变量：String str； 

(2)在栈中查找有没有存放值为"abc"的地址，如果没有，则开辟一个存放字面值为"abc"的地址，接着创建一个新的String类的对象o，并将o的字符串值指向这个地址，而且在栈中这个地址旁边记下这个引用的对象o。如果已经有了值为"abc"的地址，则查找对象o，并返回o的地址。 

(3)将str指向对象o的地址。 
值得注意的是，一般String类中字符串值都是直接存值的。但像String str = "abc"；这种场合下，其字符串值却是保存了一个指向存在栈中数据的引用！ 
为了更好地说明这个问题，我们可以通过以下的几个代码进行验证。 
```
String str1 = "abc"; 
String str2 = "abc"; 
System.out.println(str1==str2); //true 
```
注意，我们这里并不用str1.equals(str2)；的方式，因为这将比较两个字符串的值是否相等。“==”号，根据JDK的说明，只有在两个引用都指向了同一个对象时才返回真值。而我们在这里要看的是，str1与str2是否都指向了同一个对象。结果说明，JVM创建了两个引用str1和str2，但只创建了一个对象，而且两个引用都指向了这个对象。 

我们再来更进一步，将以上代码改成： 
```
String str1 = "abc"; 
String str2 = "abc"; 
str1 = "bcd"; 
System.out.println(str1 + "," + str2); //bcd, abc 
System.out.println(str1==str2); //false 
```
这就是说，赋值的变化导致了类对象引用的变化，str1指向了另外一个新对象！而str2仍旧指向原来的对象。上例中，当我们将str1的值改为"bcd"时，JVM发现在栈中没有存放该值的地址，便开辟了这个地址，并创建了一个新的对象，其字符串的值指向这个地址。

事实上，String类被设计成为不可改变(immutable)的类。如果你要改变其值，可以，但JVM在运行时根据新值悄悄创建了一个新对象，然后将这个对象的地址返回给原来类的引用。这个创建过程虽说是完全自动进行的，但它毕竟占用了更多的时间。在对时间要求比较敏感的环境中，会带有一定的不良影响。 
再修改原来代码： 
```
String str1 = "abc"; 
String str2 = "abc"; 
str1 = "bcd"; 
String str3 = str1; 
System.out.println(str3); //bcd 
String str4 = "bcd"; 
System.out.println(str1 == str4); //true 
```
str3这个对象的引用直接指向str1所指向的对象(注意，str3并没有创建新对象)。当str1改完其值后，再创建一个String的引用str4，并指向因str1修改值而创建的新的对象。可以发现，这回str4也没有创建新的对象，从而再次实现栈中数据的共享。 
我们再接着看以下的代码。 
```
String str1 = new String("abc"); 
String str2 = "abc"; 
System.out.println(str1==str2); //false 
```	
创建了两个引用。创建了两个对象。两个引用分别指向不同的两个对象。 
```
String str1 = "abc"; 
String str2 = new String("abc"); 
System.out.println(str1==str2); //false 
```
创建了两个引用。创建了两个对象。两个引用分别指向不同的两个对象。 
以上两段代码说明，只要是用new()来新建对象的，都会在堆中创建，而且其字符串是单独存值的，即使与栈中的数据相同，也不会与栈中的数据共享。

** 6. 数据类型包装类的值不可修改。不仅仅是String类的值不可修改，所有的数据类型包装类都不能更改其内部的值。 **

** 7. 结论与建议： **

1. 我们在使用诸如String str = "abc"；的格式定义类时，总是想当然地认为，我们创建了String类的对象str。担心陷阱！对象可能并没有被创建！唯一可以肯定的是，指向String类的引用被创建了。至于这个引用到底是否指向了一个新的对象，必须根据上下文来考虑，除非你通过new()方法来显要地创建一个新的对象。因此，更为准确的说法是，我们创建了一个指向String类的对象的引用变量str，这个对象引用变量指向了某个值为"abc"的String类。清醒地认识到这一点对排除程序中难以发现的bug是很有帮助的。 

2. 使用String str = "abc"；的方式，可以在一定程度上提高程序的运行速度，因为JVM会自动根据栈中数据的实际情况来决定是否有必要创建新对象。而对于String str = new String("abc")；的代码，则一概在堆中创建新对象，而不管其字符串值是否相等，是否有必要创建新对象，从而加重了程序的负担。这个思想应该是享元模式的思想，但JDK的内部在这里实现是否应用了这个模式，不得而知。

3. 当比较包装类里面的数值是否相等时，用equals()方法；当测试两个包装类的引用是否指向同一个对象时，用“==”。 

4. 由于String类的immutable性质，当String变量需要经常变换其值时，应该考虑使用StringBuffer类，以提高程序效率。
 
2.5 方法区
 
JVM有一个被所有的线程共享方法区。方法区类似于传统语言的编译后代码的存储区，或者UNIX进程中的text段。它存储每个类结构例如常量池（constant pool),成员字段域和方法和构造函数，包含类和实例初始化和接口类型类型中用到的特殊方法的代码。
方法区在虚拟机启动时创建。尽管方法区在逻辑上时heap的一部分，简单的实现仍然可以选择对它既不回收也不压缩。
 
 
> The Java virtual machine has a method area that is shared among all Java virtual machine threads. The method area is analogous to the storage area for compiled code of a conventional language or analogous to the "text" segment in a UNIX process. It stores per-class structures such as the runtime constant pool, field and method data, and the code for methods and constructors, including the special methods (§3.9) used in class and instance initialization and interface type initialization.
 
> The method area is created on virtual machine start-up. Although the method area is logically part of the heap, simple implementations may choose not to either garbage collect or compact it. This version of the Java virtual machine specification does not mandate the location of the method area or the policies used to manage compiled code. The method area may be of a fixed size or may be expanded as required by the computation and may be contracted if a larger method area becomes unnecessary. The memory for the method area does not need to be contiguous.
A Java virtual machine implementation may provide the programmer or the user control over the initial size of the method area, as well as, in the case of a varying-size method area, control over the maximum and minimum method area size.