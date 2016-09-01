title: JSTL标签
date: 2016-07-14 09:49:30
tags:
---

### 核心标签 c标签

* c:out	用于在JSP中显示数据，就像<%= ... >
* c:set	用于保存数据
* c:remove	用于删除数据
* c:catch	用来处理产生错误的异常状况，并且将错误信息储存起来
* c:if	与我们在一般程序中用的if一样
* c:choose	本身只当做 <c:when>和 <c:otherwise>的父标签
* c:when	 <c:choose>的子标签，用来判断条件是否成立
* c:otherwise	 <c:choose>的子标签，接在<c:when>标签后，当<c:when>标签判断为false时被执行
* c:import	检索一个绝对或相对 URL，然后将其内容暴露给页面
* c:forEach	基础迭代标签，接受多种集合类型
* c:forTokens	根据指定的分隔符来分隔内容并迭代输出
* c:param	用来给包含或重定向的页面传递参数
* c:redirect	重定向至一个新的URL.
* c:url	使用可选的查询参数来创造一个URL

### 格式化标签 fmt标签

* fmt:formatNumber	使用指定的格式或精度格式化数字
* fmt:parseNumber	解析一个代表着数字，货币或百分比的字符串
* fmt:formatDate	使用指定的风格或模式格式化日期和时间
* fmt:parseDate	解析一个代表着日期或时间的字符串
* fmt:bundle	绑定资源
* fmt:setLocale	指定地区
* fmt:setBundle	绑定资源
* fmt:timeZone	指定时区
* fmt:setTimeZone	指定时区
* fmt:message	显示资源配置文件信息
* fmt:requestEncoding	设置request的字符编码

### 函数标签 fn标签

* fn:contains()	测试输入的字符串是否包含指定的子串
* fn:containsIgnoreCase()	测试输入的字符串是否包含指定的子串，大小写不敏感
* fn:endsWith()	测试输入的字符串是否以指定的后缀结尾
* fn:escapeXml()	跳过可以作为XML标记的字符
* fn:indexOf()	返回指定字符串在输入字符串中出现的位置
* fn:join()	将数组中的元素合成一个字符串然后输出
* fn:length()	返回字符串长度
* fn:replace()	将输入字符串中指定的位置替换为指定的字符串然后返回
* fn:split()	将字符串用指定的分隔符分隔然后组成一个子字符串数组并返回
* fn:startsWith()	测试输入字符串是否以指定的前缀开始
* fn:substring()	返回字符串的子集
* fn:substringAfter()	返回字符串在指定子串之后的子集
* fn:substringBefore()	返回字符串在指定子串之前的子集
* fn:toLowerCase()	将字符串中的字符转为小写
* fn:toUpperCase()	将字符串中的字符转为大写
* fn:trim()	移除首位的空白符
