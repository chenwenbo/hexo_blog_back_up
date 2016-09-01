title: vim_学习笔记2
date: 2016-06-24 15:12:49
tags:
---

## vim语义

### 动词

* d 删除 delete
* r 替换 replace
* c 修改 change
* y 复制 yank
* v 选取 visual select

### 名词
* w 单词 word
* s 句子 sentence
* p 段落 paragraph
* t html标签 tag

### 介词

* i 在...之内 inside 
* a 环绕 around
* t 到...位置前 to
* f 到...位置上 forward

![](http://ww2.sinaimg.cn/large/8a0ce11egw1f56fnh6gwvj20lv09m762.jpg)

### 组词为句

动词 介词 名词

* 删除一个段落 （delete inside paragraph）
	dip
* 选取一个句子 （visual select inside sentence）
	vis
* 修改一个单词 （change inside word）
	ciw
* 修改一个单词 （change around word）
	caw
* 删除文本直到字符“x”(不包含字符“x”): delete to x
	dtx
* 删除文本直到字符“x”(包含字符“x”): delete forward x
	dfx

### 数词
动词 介词/数词 名词

* 修改三个单词：change three word
*	c3w
* 删除两个单词：delete two word
*	d2w

数词 动词 名词

* 两次删除单词（等价于删除两个单词）:twice delete word
*	2dw

* 三次删除字符（等价于删除三个字符）：three times delete charater
*	3dx