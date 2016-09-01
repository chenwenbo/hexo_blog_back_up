title: windows平台PowerLine字体配置
date: 2016-08-24 14:09:15
tags:
---

### 背景
公司的电脑一直都是windows的环境，所以配置起来一直很麻烦，这段时间一直在玩命令行，发现windows上关于powerline font的配置和linux or mac os上有很大的差别，所以记录一下以免以后再遇到此坑。

### 配置
linux中powerline font的配置非常简单，从github中download font的字体库之后，执行一下shell脚本就可以完成字体的注册。
windows环境中我用的terminal是babun,之前一直想以同样的方式进行处理，但是发现不起作用，最后在babun的官方问答中找到了答案。（能看懂英文是多么的重要）

```
This is not an error. Btw you have to install the fonts on your host system. Just download the powerline fonts and install them into your Windows fonts directory. Then you have to change to mintty font (Options -> Text) f.e. to "Dejavu Sans Mono for Powerline".
```

简单说来就是：需要讲字体文件copy到系统的字体目录（c:/windows/fonts/）中，然后在terminal的设置中选中相关的字体就可以了。

