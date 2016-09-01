title: RunTimeException
date: 2016-01-15 10:34:56
tags:
---
---
最近在编码的时候，经常需要做一些异常校验，每次都使用**checkExcpetoin**觉得非常繁琐，所以学习了一下jdk自带的**uncheckException**。

常见的几种如下：

 > - **NullPointerException - 空指针引用异常**
 > - **ClassCastException - 类型强制转换异常**
 > - **IllegalArgumentException - 传递非法参数异常**
 > - **ArithmeticException - 算术运算异常**
 > - **ArrayStoreException - 向数组中存放与声明类型不兼容对象异常**
 > - **IndexOutOfBoundsException - 下标越界异常**
 > - **NegativeArraySizeException - 创建一个大小为负数的数组错误异常**
 > - **NumberFormatException - 数字格式异常**
 > - **SecurityException - 安全异常**
 > - **UnsupportedOperationException - 不支持的操作异常**
 

使用实例（参数校验）:
```java 
private static String typeCalculate(String value){
		String res = "";
		switch (value) {
		case "1":
			res = "0.3";
			break;
		case "2":
			res = "0.6";
			break;
		default:
			throw new IllegalArgumentException("input:"+value+"\t,error input parameter!");
		}
		return res;
	}
```
