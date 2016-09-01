title: Java_class对象_实例对象
date: 2016-06-30 13:50:46
tags:
---

今天看到一片文章[Java到底是不是一种纯面向对象语言？](http://mp.weixin.qq.com/s?__biz=MzIxNjA5MTM2MA==&mid=2652432852&idx=1&sn=d26a99274231e0272fd51b6db491adc4&scene=4#wechat_redirect)中提到实例化对象和class对象的区别，我的理解是因为这两者存在内存区域不同，一者存放在数据共享区域，另一者存放在线程私有区域，所有才会有静态变量共享的情况。

> [本文引用](https://segmentfault.com/a/1190000004706888)

### java中的两种对象 
java作为一种面相对象的语言，在jvm中一切皆为对象。基本数据类型有其包装类，静态变量有其对应的class对象

** 实例对象 **
```
public class Person {
    public static void main(String[] args){
        Person p = new Person();
    }
}
```
通过new这种方式得到的我们称之为实例对象。

** class对象 **

Class对象是没有办法用new关键字得到的，它是jvm用来保存对应的类信息的，有点类似与数据库中的元数据，描述数据的数据。
当我们讲java源文件编译为字节码class文件的时候，同时会创建一个Class对象保存在.class文件中。

同时，jvm的类加载机制会在需要的时候将.class文件和对应的Class对象加载到内存中。
所以，在源文件编译的同时也会产生一个Class对象

### 如何获取Class对象

Class对象是jvm用来保存实例对象的相关信息，除此之前我们可以将其看作一般的实例对象。
事实上，所有的Class对象都是类Class的实例。

* 通过实例变量的getClass()方法获得
```
        Dog dog = new Dog();
        Class d = dog.getClass();  
```

* 通过类Class的静态方法forName()
```
 try {
            Class dog1 = Class.forName("Dog");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
```

加载数据库驱动类
```
 try{   
    //加载MySql的驱动类   
    Class.forName("com.mysql.jdbc.Driver") ;   
    }catch(ClassNotFoundException e){   
    System.out.println("找不到驱动程序类 ，加载驱动失败！");   
    e.printStackTrace() ;   
    }  
```

* 直接通过对象类文件的.class
```
Class dog2 = Dog.class;
```

### Class对象的使用和反射
JAVA反射机制是在运行状态中,对于任意一个类，都能够知道这个类的所有属性和方法；
对于任意一个对象，都能调用它的任意一个方法和属性；
这种动态获取信息以及动态调用对象的方法称之为java的语言机制。

简而言之，我们可以通过.class逆向生成java源文件（jad反编译工具），我们可以通过反射机制访问一个类的对象属性，方法，甚至轻易改变一个私有成员。

```
class Cat{
    public static int count;
    public int age;
    private String name;
    
    static {
        count = 0;
    }
    
    public Cat(){
        age = count++;
        System.out.println("this is class Cat!");

    }
    
    public void run(){
        
    }
    
    private void ruff(){}
}
```

注意到我们的类中包含静态成员，私有变量，静态初始化以及私有方法。这里在提一下所谓的懒加载：当Cat.java编译成Cat.class文件后并不会立即被加载到内存，而是在它的的静态成员第一次被访问时才被加载(这么看来，Cat的默认构造方法也是静态的！)

```
	Class c = Cat.class;
	Field[] fields = c.getDeclaredFields();
	for (Field field : fields){
	    System.out.println(field);
	}

	运行结果：
	public static int Cat.count
	public int Cat.age
	private java.lang.String Cat.name
```
### 通过Class对象拿到对应的实例对象
```
	try {
	    Cat cat = (Cat) c.newInstance();
	} catch (InstantiationException e) {
	    e.printStackTrace();
	} catch (IllegalAccessException e) {
	    e.printStackTrace();
	}
```
这时候静态代码块执行了：

this is class Cat!
接下来我们做一件神奇的事情：
```
	try {
        Class catClass = Class.forName("Cat");
        Field name = catClass.getDeclaredField("name");
        name.setAccessible(true); //关闭安全检查开关，提高反射速度
        Cat cat2 = (Cat) catClass.newInstance();
        name.set(cat2,"Aristark");
        System.out.println(cat2.getName());
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    } catch (IllegalAccessException e) {
        e.printStackTrace();
    } catch (InstantiationException e) {
        e.printStackTrace();
    } catch (NoSuchFieldException e) {
        e.printStackTrace();
    }
```
这次我们使用Class.forname()来获取Class对象，它的作用是让jvm查找并加载指定的类，也就是说Cat类的静态代码块会被执行。其次值得注意的是，我们通过Class的几个方法访问了原本不可以被访问的name属性：
```
this is class Cat!
Aristark
```

