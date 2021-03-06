title: HTML属性
date: 2016-07-20 13:59:05
tags:
---

### HTML属性

HTML 标签可以拥有属性。属性提供了有关 HTML 元素的更多的信息。
属性总是以名称\/值对的形式出现，比如：name="value"。
属性总是在 HTML 元素的开始标签中规定。

### 属性实例

HTML 链接由 a 标签定义。链接的地址在 href 属性中指定：
```
<a href="http://www.w3school.com.cn">This is a link</a>
```

### 更多 HTML 属性实例

属性例子 1:
```
<h1> 定义标题的开始。
<h1 align="center"> 拥有关于对齐方式的附加信息。
```

TIY : 居中排列标题
属性例子 2:
```
<body> 定义 HTML 文档的主体。
<body bgcolor="yellow"> 拥有关于背景颜色的附加信息。
```

TIY : 背景颜色
属性例子 3:
```
<table> 定义 HTML 表格。（您将在稍后的章节学习到更多有关 HTML 表格的内容）
<table border="1"> 拥有关于表格边框的附加信息。
```

**HTML 提示：使用小写属性**
属性和属性值对大小写不敏感。
不过，万维网联盟在其 HTML 4 推荐标准中推荐小写的属性/属性值。
而新版本的 (X)HTML 要求使用小写属性。

**始终为属性值加引号**
属性值应该始终被包括在引号内。双引号是最常用的，不过使用单引号也没有问题。
在某些个别的情况下，比如属性值本身就含有双引号，那么您必须使用单引号，例如：
```
name='Bill "HelloWorld" Gates'
```

**HTML 属性参考手册**
我们的完整的 HTML 参考手册提供了每个 HTML 元素可使用的合法属性的完整列表：
下面列出了适用于大多数 HTML 元素的属性：

|属性	|值	                |    描述
| --------   | -----:  | :----:  |
|class	|classname	        |规定元素的类名（classname）
|id	id	|规定元素的唯一     |       id
|style	|style_definition	|规定元素的行内样式（inline style）
|title	|text	            |规定元素的额外信息（可在工具提示中显示）
