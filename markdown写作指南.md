title: markdown写作指南
date: 2016-05-05 18:07:02
tags:
---

> ** Markdown ** is a way to style text on the web. You control the display of the document;formatting words as bold or italic,adding images,and creating lists are just a few of the things we can do with Markdown. Mostly,Markdown is just regular text with a few non-alphabetic characters thrown in,like # or *.

### headers
```
# this is an <h1> tag
## this is an <h2> tag
###### this is an <h6> tag
```
---

### lists
** Unordered **
```
* Item 1
* Item 2
	* Item 2a 
	* Item 2b
```
** Ordered **
```
1. Item 1
2. Item 2
	* Item 2a 
	* Item 2b
```
---
### 图片（Images）
```
![GitHub Logo](/images/logo.png)
Format: ![描述](url)
```
---
### 重点（emphasis）
```
* 这段文字将变成斜体 *
_ 这段文字也会变成斜体 _

** 这段文字将变成粗体 **
__ 这段文字也会变成粗体 __

```
---
### 链接（links）
```
[GitHub Logo](/images/logo.png)
Format: [描述](url)
```
---
### 引用（blockquotes）
```
![GitHub Logo](/images/logo.png)
Format: ![描述](url)
```
---
### 反斜杠 转义字符（backslash escapes）
```
\*hello world\*
*hello world*
```
---
Markdown procides backslash escapes for the following characters:
```
\ ` * _ {} [] () # + - . !
```

### table
```
| 项目        | 价格   |  数量  |
| --------   | -----:  | :----:  |
| 计算机     | \$1600 |   5     |
| 手机        |   \$12   |   12   |
| 管线        |    \$1    |  234  |
```
| 项目        | 价格   |  数量  |
| --------   | -----:  | :----:  |
| 计算机     | \$1600 |   5     |
| 手机        |   \$12   |   12   |
| 管线        |    \$1    |  234  |

### html tag
```
&lt; &gt;
<>
```
      

