title: Json格式校验
date: 2016-01-18 09:51:50
tags:
---

用到的第三方库：     **stringtree-json-2.0.5.jar**

使用方法：
使用JSONValidator类，其只有一个构造方法如下：
 
```python
public JSONValidator(JSONErrorListener listener)
  {
    this.listener = listener;
  }
```
JSONErrorListener是一个json校验的监听器接口，会在json校验过程中将错误处记录下来。
包含三个方法：


```python
public abstract interface JSONErrorListener
{
  public abstract void start(String paramString);

  public abstract void error(String paramString, int paramInt);

  public abstract void end();
}
```
其有三个实现类：BufferErrorListener/StdoutStreamErrorListener/ExceptionErrorListener
其结构如下：

![](http://ww1.sinaimg.cn/large/8a0ce11egw1f03ewlz3akj20cx02mwex.jpg)

BufferErrorListener：  不会打印出校验的错误信息记录，只会返回true/false.
StdoutStreamErrorListener: 会在控制台打印出校验的错误信息记录，也会返回true/false.
ExceptionErrorListener： 会以异常的形式抛出错误。不会返回true/false.

我在项目中的需求是要把错误信息返回给调用方。所以我这里用到了BufferErrorListener的另一个构造方法。

![](http://ww4.sinaimg.cn/large/8a0ce11egw1f03ewzcncvj20ao03ugm0.jpg)

自己定义一个变量去接受错误消息。  然后将此变量传给接口的调用方。
Like this:
![](http://ww3.sinaimg.cn/large/8a0ce11egw1f03excdi20j20hw03m0u7.jpg)

至此，完美解决我的需求。
