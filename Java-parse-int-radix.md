title: Java_parse_int_radix
date: 2016-06-22 18:07:58
tags:
---

以不同的基数（进制）解析整数。


```
public static int parseInt(String s , int radix)
 
 s   the String containing the integer representation to be parsed
 radix   the radix to be used while parsing （进制）

* <p>Examples:
* <blockquote><pre>
* parseInt("0", 10) returns 0  使用十进制
* parseInt("473", 10) returns 473 使用十进制
* parseInt("+42", 10) returns 42 使用十进制
* parseInt(" -0", 10) returns 0 使用十进制
* parseInt(" -FF", 16) returns - 255 使用十六进制
* parseInt("1100110", 2) returns 102 使用二进制
* parseInt("2147483647", 10) returns 2147483647
* parseInt(" -2147483648", 10) returns - 2147483648
* parseInt("2147483648", 10) throws a NumberFormatException
* parseInt("99", 8) throws a NumberFormatException
* parseInt("Kona", 10) throws a NumberFormatException
* parseInt("Kona",
```
