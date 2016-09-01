title: Java_泛型笔记
date: 2016-07-01 09:51:10
tags:
---

> [黄博文@无敌北瓜](http://www.cnblogs.com/huang0925)
> [segmentfault](https://segmentfault.com/a/1190000005179142)
> [segmentfault](https://segmentfault.com/a/1190000003831229)


### 使用泛型的好处
* 类型检查，编译阶段可以检查出错误类型信息
* 避免强制类型转换
* 方便实现通用方法

### 约定的类型名称
* E element
* K key
* N number
* T type
* V value 

### 对类使用泛型
```
public class Box<T> {
    public T getObject() {
        return object;
    }

    private T object;

    public void setObject(T object) {
        this.object = object;
    }
}
```
我们可以看到所有的Object都替换为了T,这样我们可以直接在调用的时候显示的传入需要的类型。

我们也可以使用多个参数作为泛型。
```
public class Pair<T, V> {
    private T key;
    private V value;

    public Pair(T key, V value) {
        this.key = key;
        this.value = value;
    }

    public T getKey() {
        return key;
    }

    public V getValue() {
        return value;
    }
```
使用方法如下：
```
Pair<Integer, String> one = new Pair<Integer, String>(1, "one");
```

### 对方法使用泛型
当泛型作用与类时，在整个类的范围内都可以使用该泛型。
另外， 我们在静态方法、非静态方法及构造函数都可以使用泛型。
```
public static <T, U> boolean compare(Pair<T,U> pair1, Pair<T,U> pair2)
    {
        return pair1.getKey().equals(pair2.getKey()) && pair1.getValue().equals(pair2.getValue());
    }
```

### 对泛型进行限定
默认情况下我们可以使用任何类型的泛型，但是我们如果相对传入的参数进行限定的时候，我们可以声明的时候使用extends关键字。
```
public class Box<T extends Number> {
    public T getObject() {
        return object;
    }

    private T object;

    public void setObject(T object) {
        this.object = object;
    }
}

        Box box = new Box();
        box.setObject(10);    //ok
        box.setObject("hello");  //compile-time error
```
加上限定类或者接口之后，我们可以使用泛型参数变量调用该类或者接口的方法。

### 通配符的使用（List泛型传参的问题）
```
	public static void test(List<Number> list){
		
	} 
	
	public static void main(String[] args) {
		 List<Integer> list = new ArrayList<Integer>();
		 test(list); // compile error
	}
```
```
public static void test(List<? extends Number> list){
		
	} 
	
	public static void main(String[] args) {
		 List<Integer> list = new ArrayList<Integer>();
		 test(list); // pass
	}
```
通配符也可以作用与 父类
<? super A>

### 总结
* 泛型的参数只能是对象，而不能是原始类型
* 不能直接使用泛型对象创建实例
```
public static <E> void append(List<E> list) {    E elem = new E();  // compile-time error    list.add(elem); 
}
```
* 可以通过反射创建对象
```
public static <E> void append(List<E> list, Class<E> cls) throws Exception {    E elem = cls.newInstance();   // OK    list.add(elem);}
```
调用方法
```
List<String> ls = new ArrayList<>();
append(ls, String.class);
```
* 类型参数不能作为静态字段
```
public class Box<T> ｛
	pravate static T; //compile error
｝
```
* 不能创建类型参数的数组
```
List<Integer>[] arrayList = null; //compile error
```
* 不能重载一个方法，该方法的形参都来自同一个类型的参数对象
```
public class Example {

  public void print(List<Integer> integers) {}

    public void print(List<Double> doubles) {}
}

```
