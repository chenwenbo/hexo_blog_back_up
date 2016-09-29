title: zsh_feature
date: 2016-09-05 11:15:22
tags:
---

原文链接：http://code.joejag.com/2014/why-zsh.html

zsh是我目前工作中用到最多的一个shell,通过oh-my-zsh的配置包可以让我们的命令行变得异常强大，现在介绍一下zsh的特性。

### TAB命令
* 补全命令，可将可能出现的命令选项列在下方
* 可支持目录补全 g/r/p+tab -> github/resource/post
* 更好用的历史记录  命令+上下左右 -> 可以将该命令的历史记录列出来。
* 在命令中使用通配符，可以讲该规则的文件全部列出 cp *.txt +tab -> cp aa.txt bb.txt
* cd -+tab ：选择历史目录
