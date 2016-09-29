title: Spring_bean_load
date: 2016-09-07 14:47:20
tags:
---

Spring中定义的bean什么时候被加载？

可能初始化的时候进行加载，也有可能第一次访问的时候进行加载，依赖于对bean的配置，受lazy-init scope属性控制。
