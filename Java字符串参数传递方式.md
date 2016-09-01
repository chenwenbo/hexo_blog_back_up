title: Java字符串参数传递方式
date: 2016-05-10 18:35:19
tags:
---
这是java的一个典型的问题，很多相似的问题曾经在 stackoverflow被提出，并且有很多不正确不完整的答案，如果你不想太多的话， 这个问题会很简单，现在让我来看一下问题吧。

### 1. 一个有趣的 令人迷惑的代码片段
```
public static void main(String[] args) {
        String x = new String("ab");
        change(x);
        System.out.println(x);}
 
public static void change(String x) {
        x = "cd";}
```
这段程序将输出 “ab”

在c++中，看如下代码：
```
void change(string &x) {
    x = "cd";}
 
int main(){
    string x = "ab";
    change(x);
    cout << x << endl;}
```
这段程序将输出 “cd”

### 2.一般容易迷惑的问题
x存储着指向“ab”的heap区的引用，所以当x作为参数传递到change方法时候，他在heap区还是指向"ab "
![](http://ww4.sinaimg.cn/large/8a0ce11egw1f3qh1yeeakj20i206vaaa.jpg)

因为java是值传递，x的值是 “ab”的引用。当change方法调用的时候，它创建了一个新的“cd”对象，并且x现在指向“cd”,向下图一样，
![](http://ww3.sinaimg.cn/large/8a0ce11egw1f3qh4rl01nj20i206v0t2.jpg)

这个看起来好像是一个很合理的解释，这儿确定的是使用的值传递，但是问题在哪里呢？

### 3.这段代码真正做了什么？
上面一段解释有几个错误，对于简单的理解真个过程来说，这或许是一个好主意。
当“ab”被创建的时候，java持有一定数量的内存储存这个对象，这个对象被分配给x,这个变量实际上是分配了一个引用给这个对象，这个引用是对象被创建的内存地址。

这个变量包含了String对象的引用，x不是引用本身，他是储存引用（内存地址）的变量，

java是值传递，所以当x被传递到change方法的时候，x的值被传递了，change方法创建了另一个“cd”对象，并且他们有着不同的引用，并不是之前的自身引用。

下面的图表展示了实际情况，     
![](http://ww1.sinaimg.cn/large/8a0ce11egw1f3qh5k8osgj20i206smxh.jpg)

### 4.错误的解释
第一段代码的问题与java String的不可变性没有关系，甚至java可以替换为 StringBuilder.结果也是一样的，关键点是变量存储的是引用，并不是引用自身，

### 5.解决方案
如果我们真的需要改变对象的值，首先，这个对象应该是可以改变的，例如，Stringbuilder,第二步，我们需要确定没有新的对象产生，没有新的对象赋值给参数变量，因为java是值传递，
public static void main(String[] args) {
        StringBuilder x = new StringBuilder("ab");
        change(x);
        System.out.println(x);}
 
public static void change(StringBuilder x) {
        x.delete(0, 2).append("cd");}

