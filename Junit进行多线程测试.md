title: Junit进行多线程测试
date: 2016-01-18 09:56:17
tags:
---

> **本文引用自**：http://mushiqianmeng.blog.51cto.com/3970029/897786

写过Junit单元测试的同学应该会有感觉，Junit本身是不支持普通的多线程测试的，这是因为Junit的底层实> > 现上，是用System.exit退出用例执行的。

JVM都终止了，在测试线程启动的其他线程自然也无法执行。

在多线程环境下，程序退出的条件是，所有的非Daemon线程都正常结束或者某个线程条用了system.exit方法，导致进程强行退出。在eclipse下运行Junit的类是org.eclipse.jdt.internal.junit.runner.RemoteTestRunner。通过查看这个类的main方法。如下：  


```
public static void main(String  [] args) {  
    try {  
         RemoteTestRunner testRunServer= new RemoteTestRunner();  
         testRunServer.init(args);  
         testRunServer.run();  
     } catch (Throwable   e) {   
         e.printStackTrace(); // don't allow System.exit(0) to swallow exceptions  
     } finally {  
         // fix for 14434  
         System.exit(0);  
     }  
 }    
```

显然，只要主线程结束，整个程序将会退出，这就是采用junit的时候奇怪退出程序的原因。  


为了避免上面的这种情况，我想出来的两种方法：

 - 不使用JUnit来测试，而是自己写个main方法来进行测试.
 - 还使用JUnit，但在测试代码的最后加上sleep语句，即如下代码的最后的 sleep那样：


