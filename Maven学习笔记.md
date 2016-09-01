title: Maven学习笔记
date: 2016-08-02 09:35:22
tags:
---

### Maven坐标
坐标是Maven的基本概念，有了它我们才能在Maven仓库中定位到我们需要的组件。
例子：没有坐标、使用JUnit的时候，我们没有定位下载JUint的依赖包，只有有了坐标我们才能找到我们需要的依赖包。

用依赖的方式，简单配置使用如junit:junit:4.8.2就可以了。这里第一个junit是groupId，第二个junit是artifactId，4.8.2是version。

Maven的很多其他核心机制都依赖于坐标，其中最显著的就是仓库和依赖管理。对于仓库来说，有了坐标就知道在什么位置存储构件的内容，例如junit:junit:4.8.2就对应仓库中的路径/junit/junit/4.8.2/junit-4.8.2.pom和/junit/junit/4.8.2/junit-4.8.2.jar这样的文件，读者可以直接访问中央仓库地址看到这样的仓库布局，或者浏览本地仓库目录~/.m2/repository/以获得直观的体验。

依赖的配置也是完全基于坐标的，例如：
``` 
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.8.2</version>
  <scope>test</scope>
</dependency>
```
有了正确的坐标,Maven才能够在正确的位置找到依赖文件并使用，这里值为test的scope是用来控制该依赖只在测试时可用，与坐标无关。

* groupId : 项目的名字 com.zoo.cat
* artifactid : 模块的名字 （对于分层的项目我们可以划分service respo controller模块）
* versionid : 版本

### Maven构建周期
 validate - validate the project is correct and all necessary information is available
* compile - compile the source code of the project
* test - test the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed
* package - take the compiled code and package it in its distributable format, such as a JAR.
* verify - run any checks on results of integration tests to ensure quality criteria are met
* install - install the package into the local repository, for use as a dependency in other projects locally
* deploy - done in the build environment, copies the final package to the remote repository for sharing with other developers and projects.

### 利用Maven对Web应用做自动化集成测试  
[Maven集成测试配置](http://www.infoq.com/cn/news/2011/03/xxb-maven-5-integration-test)


### Maven常用命令
* mvn compile 编译源代码
* mvn test-compile 编译测试代码
* mvn test 运行测试
* mvn package 打包（根据pom中配置打为jar或者war）
* mvn -Dtest package 打包但不测试
* mvn install 在本地Repository中安装jar
* mvn clean 清楚产生的项目
* mvn eclipse:eclpse 生成eclipse项目
* mvn idea:idea 生成idea项目 （ipr是命令行运行文件）
* mvn eclipse:clean 清楚eclipse的一些系统设置
* mvn jetty:run
* mvn tomcat7:run


### 执行流程
1、通过svn / git 下载代码到本地
2、mvn idea:idea / mvn eclipse:eclipse 生成项目文件
3、导入对应ide
4、修改代码之后进行编译 mvn:compile
5、运行测试 mvn:test


