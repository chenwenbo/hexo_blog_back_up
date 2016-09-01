title: clean code notes
date: 2016-08-30 12:12:57
tags:
---

## 有意义的命名

### 名副其实

* 为你的class field method variable取一个有表达力的名字，避免采用注释
* 消除魔法数字

### 避免误导

* 避免取名的时候引起迷惑，accountList -> accounts/accountGroup
* 避免因为同样的拼写方式带来的歧义 l和1

### 做有意义的区分

* 避免使用数字作为变量的命名 反模式 ：a1 a2 a3
* 对函数的命名做有意义的区分 反模式 ：getAccounts getAccountInfo getAccount
* 有意义的命名对于搜索会提供便利
* 不要为你的变量命名加上类型 反模式：phoneString

### 接口和实现

* 对于接口的命名不要使用先导词 IShapeFactory -> ShapeFactory 

### 类名

类名和对象名应该是名次短语，如Customer、Account、AddressParse。避免使用Manager、Process、Data或者Info这样的类名。类名不应当是动词。

### 方法名

方法名应当是动词或者动词短语，如postPayment、deletePage、save。属性访问器、修改器和断言应该根据其值命名，并依Javabean标准加上get/set和is前缀。
构造器进行重构时，使用静态工厂方法要优于new
```
Complex fulcrumPoint = Complex.FromRealNumber(23.0);
```
优于
```
Complex fulcrumPoint = new Complex(23.0);
```
可以通过将构造器private的方法，强制使用此种命名手段。

### 每个概念对应一个词
采用同一类型的命名规则，Controller 和 Manager选择一个使用，不需要混合使用。
避免使用双关语，add insert save append 某种意义上可能都会表达同一件事。

### 添加有意义的语境
为你的方法名加上语境吧，save -> saveAccount


## 如何写好函数

* 短小 （10行内）
* 代码块和缩进，if、else、while等语句要有正确的缩进，不能写在一行
* 只做一件事，方便复用
* 每个函数一个抽象层级，不要讲不同抽象层级的代码放在一块
* 利用多态消除switch语句
* 使用描述性的名称，不要害怕名称过长，函数名称要描述函数的作用

### 函数参数

* 函数参数的个数最好不要操作3个
* 参数个数过多时，将其封装为对象
* 使用异常代替错误返回码，简化代码结构
* 由于try/catch的原因，导致我们的代码丑陋不堪，我们可以讲主体部分逻辑代码抽离成为函数，简化代码结构。
* 使用异常类代替枚举错误码，因为枚举类型会在编译器就运行，所以当添加枚举类型的时候需要重新编译使用该枚举的所有类。
* 消除重复
 

## 数据、对象的反对称性
面向过程代码便于在不改动既有数据结构的前提下添加新函数，面向对象代码便于在不改动既有函数的前提下添加新类。

过程式代码难以添加数据结构，因为必须修改所有函数，面向对象代码难以添加新函数，因为必须修改所有的类

## 类
* 类应该保证短小
* 类应该保证单一职能原则、只有保证了此原则，类才会更加内聚

