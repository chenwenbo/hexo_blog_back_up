title: Log4j日志记录前是否需要进行判断日志级别？
date: 2016-01-19 10:27:00
tags: JAVA
categories: 编程
---

今天团队讨论一个问题，在我们进行日志记录的时候，到底应不应该在前面加上 isDebugEnabled()这种判断？
 
```
if (logger.isDebugEnabled ()) {  
    logger.debug("This is debug message from Dao.");  
} 
```
或者直接使用：
```
logger.debug("This is debug message from Dao.");  
```

为了解决这个问题，我查看了一下debug()的源码：
```
public void debug(Object message) {
    if(repository.isDisabled(Level.DEBUG_INT))
      return;
    if(Level.DEBUG.isGreaterOrEqual(this.getEffectiveLevel())) {
      forcedLog(FQCN, Level.DEBUG, message, null);
    }
  }
```
由此可以看出，我们在执行debug方法的时候已经执行了日志级别的判断。所以是不是isDebugEnabled()这类方法就用不到了呢？

** 并不是! ** 如果你的debug写法是这样的 
```
logger.debug("debug信息测试"+serivce.getScore());
```
这个时候你就需要加上if判断了，如下：
```
if（ logger.isDebugEnabled() ）｛
     logger.debug("debug信息测试"+serivce.getScore());
｝
```
因为执行serivce.getScore()是需要时间花销的，所以从系统运行效率上来讲我们应该加上if判断，但是如果是debug信息中的变量参数执行效率可以忽略不计， 
```
logger.debug("debug信息测试"+param);
```
---

### 结论：
到底在打印日志信息是是够需要加上判断，要根据实际情况进行判断， 如果debug（）中的参数没有变量， 或者变量执行时间短，我们则可以不加if判断，反之则需要加上if判断。
