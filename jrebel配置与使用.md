title: Jrebel配置与使用
date: 2016-07-11 17:14:35
tags:
---

jrebel用于实现web项目的热部署，我之前的的工作场景是这样的，修改了某些java代码之后，重启tomcat服务器验证页面效果，一直重复这个过程，而tomcat启动又是一个很浪费时间的过程，再启动的过程中我们没有办法做任何事情，导致时间白白浪费了，现在可以使用jrebel实现热部署，可以极大的提升开发效率。

我现在使用开发模式是先将所有的功能对应的接口写好，并且通过单元测试验证service接口，在所有的接口顺利通过单元测试之后，才开始设计页面，因为后台管理系统的页面其实是固定的一些代码，核心部分还是在service之上，采用这种方法可以提高效率，先解决主要的问题。

另外最近一直在尝试结对编程的方式进行开发项目，大概实验了五天，感觉还是不错的，上午的时间我们讨论需求，确定技术方案，完成一些伪代码，具体的实现我们留到下午时间去完成，每天上午的时间都会对前一天下午的代码进行code review. 这样下来，我感觉效率比之前提升了很多，且不容易出错。

下面进入正文

### 下载jrebel插件
在Eclipse的插件市场中下载jrebel的插件，也可采用离线下载的方式进行下载。

### 下载jrebel.jar的相关文件
我是在网络上找的破解版，下载好之后讲jrebel.jar 和 jrebel.lic文件覆盖eclipse中对应的插件目录 plugins\org.zeroturnaround.eclipse.embedder_6.4.6.RELEASE\jrebel

### tomcat运行参数配置
-Drebel.spring_plugin=true
-Drebel.hibernate_plugin=true 
-Drebel.struts2-plugin=true
-noverify  关闭字节码校验
-Drebel.log=true 日志配置
-Drebel.log.trace=true 日志配置
-XX:MaxPermSize=128m

### 内存溢出解决方案
我在jeesite中使用jrebel时出现了内存溢出的情况，找到的解决方案如下：
I’m getting java.lang.OutOfMemoryError: PermGen space!??

If you have a large number of classes in your application, then the above exception may occur – running your application with JRebel requires a little bit more memory than running without. Although the exact amount depends on the application itself, we usually find that it falls somewhere between 10-15% extra. Our recommendation is to start from there and then bump it up until the exception stops occurring. Also, in general, we recommend at least 128 MB as the PermGen size for your application/server. You can use the flag -XX:MaxPermSize=128m to configure the amount of available PermGen memory.

### 下载
链接: http://pan.baidu.com/s/1pLvkPKn 密码: ztkq

### 官网常见问题答疑
http://zeroturnaround.com/software/jrebel/learn/faq/