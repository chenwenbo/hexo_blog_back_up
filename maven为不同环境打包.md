title: Maven为不同环境打包
date: 2016-08-22 11:20:03
tags:
---

开发中我们经常会需要讲不同的组装不同的war(test/app/pro)到服务器上，但是不同的war有一些差异化的文件，这就需要我们抽象出差异化的部分，统一进行处理。

### 方案一
建立多个不同的pom, package时制定不同的pom，每个pom中指定差异化的文件的package方法。
maven war plugin中我使用webresources 中的 directory 和 targetPath 来制定差异化的文件。
```
mvn clean package -f pom_test.xml
or 
mvn clean package -f pom_pro.xml
or
mvn clean package -f pom_app.xml
```
缺点：这种方式的缺点就是多个pom之间重复配置太多，耦合太严重，不易维护。


### 方案二

利用maven的profile进行个性化配置在Maven中我们可以配置多个profile,这样一来我们就可以配置多个环境的profile.
结合war插件制定差异化的位置，这样我们就可以实现为不同环境package.

```
调用命令：mvn clean package -D maven.test.skip=true package -P pro

配置：

<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-war-plugin</artifactId>
	<version>2.4</version>
	<configuration>
		<packagingExcludes>
			<!-- WEB-INF/classes/com/thinkgem/jeesite/** -->
			WEB-INF/classes/org/apache/ibatis/**,
			WEB-INF/classes/org/mybatis/spring/**,
			<!-- ignore different env setting  -->
			WEB-INF/classes/war/**
		</packagingExcludes>
		<warSourceExcludes>
			static/bootstrap/2.3.1/docs/**
		</warSourceExcludes>
		<webappDirectory>${project.build.directory}/${project.artifactId}</webappDirectory><!-- 
		<webXml>${project.basedir}/target/jspweb.xml</webXml> -->
		<warName>${project.artifactId}</warName>
		<webResources>
			<resource>
				<directory>${runtime.env}</directory>
				<targetPath>WEB-INF/classes</targetPath>
			</resource>
		</webResources>
	</configuration>
</plugin>

<profile>
    <id>app</id>
    <properties>
        <runtime.env>src/main/resources/war/pro</runtime.env>
    </properties>
</profile>
<profile>
    <id>demo</id>
    <properties>
        <runtime.env>src/main/resources/war/demo</runtime.env>
    </properties>
</profile>
<profile>
    <id>pro</id>
    <properties>
        <runtime.env>src/main/resources/war/pro</runtime.env>
    </properties>
</profile>
```

至此就可以解决我们的问题了。
