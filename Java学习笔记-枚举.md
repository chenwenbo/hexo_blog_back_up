layout: effective
title: Java学习笔记---枚举
date: 2016-01-19 10:26:40
tags: JAVA
categories: 编程
---

在我们的代码中的范围常量应尽量使用枚举来进行处理，而不是private static final int SIZE = 1 这种。
范围常量：例如 月份、类型等
在我们的枚举类中可以重写toString()方法使我们的拿到我们想要的值。
eg:
```
public enum Operatoin{
      PLUS("+"),MINUS("-");
      private String symbol;
      piblic void Operation(String symbol){
           this.symbol = symbol;
      }
      @override publicString toString(){
           return symbol;
      }
}
```