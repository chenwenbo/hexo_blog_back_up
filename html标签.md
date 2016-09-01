title: html标签
date: 2016-07-19 13:30:11
tags:
---

### HTML 标题
HTML 标题（Heading）是通过 &lt;h1&gt; - &lt;h6&gt; 等标签进行定义的。
```
<h1>This is a heading</h1>
<h2>This is a heading</h2>
<h3>This is a heading</h3>

```
### HTML 段落
HTML 段落是通过 &lt;p&gt; 标签进行定义的。
```
<p>This is a paragraph.</p>
<p>This is another paragraph.</p>

```
### HTML 链接
HTML 链接是通过 a 标签进行定义的。
```
<a href="http://www.w3school.com.cn">This is a link</a>

```
### HTML 图像
HTML 图像是通过 &lt;img&gt; 标签进行定义的。
```
<img src="w3school.jpg" width="104" height="142" />
```

### 表格
表格由 table 标签来定义。每个表格均有若干行（由 tr 标签定义），每行被分割为若干单元格（由 td 标签定义）。字母 td 指表格数据（table data），即数据单元格的内容。数据单元格可以包含文本、图片、列表、段落、表单、水平线、表格等等。
```
<table border="1">
<tr>
<td>row 1, cell 1</td>
<td>row 1, cell 2</td>
</tr>
<tr>
<td>row 2, cell 1</td>
<td>row 2, cell 2</td>
</tr>
</table>
```

### 无序列表
无序列表是一个项目的列表，此列项目使用粗体圆点（典型的小黑圆圈）进行标记。
无序列表始于 ul 标签。每个列表项始于 li。
```
<ul>
<li>Coffee</li>
<li>Milk</li>
</ul>
```

### 有序列表
同样，有序列表也是一列项目，列表项目使用数字进行标记。
有序列表始于 ol 标签。每个列表项始于 li 标签。
```
<ol>
<li>Coffee</li>
<li>Milk</li>
</ol>
```