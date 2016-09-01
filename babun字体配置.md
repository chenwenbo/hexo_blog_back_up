title: babun的powerline字体配置
date: 2016-08-25 10:13:14
tags:
---
### 问题
下载babun到我的windows环境中之后，我准备开始切换主题，但是这个时候我开始发现各种乱码？

### 解决思路
基于我在mac上配置过程我知道我应该先安装powerline status.然后下载字体。

最开始我按照这种思路进行配置，发现并没有任何效果，最后在babun的论坛中找到了类似的出题思路，需要讲字体下载到windows的字体库中：windows/fonts/~.

下载完成之后我可以在terminal的opation中设置字体，但是现在还有一个问题就是，terminal中的字体设置会出现仅仅列出了很少的字体配置，查资料得知，在babun的~/.minttyrc文件中保存的是babun的字体配置文件，然后将其修改为合适的字体即可。

### babun 提示信息配置
zsh的很多主题的左边都会显示user and hostname 的信息，会影响美观，这个时候可以在zshconfig中加入这样一段配置
```
prompt_context(){
    prompt_segment balck default "chen"
}
```

### babun link setting 
每次当我利用wox打开babun的时候，总会默认使用bash. 然后我就要手动zsh切换一下，觉得很不爽。
于是我把babun.link的目标修改为C:\Users\Administrator\.babun\cygwin\bin\mintty.exe /bin/zsh.exe就可以解决我的问题了。


### 感悟
学好英语真的很重要，因为我在查找资料过程中看到的有价值的信息都是英文的，如果读不懂英文真的会错过好多有用的东西。

