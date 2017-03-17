---
title: maven创建普通web项目
date: 2017-03-17 11:21:40
category: maven
tags: [maven]
---
## 进入命令行，执行：
``` shell
mvn archetype:generate -DgroupId=com.jjs.app -DartifactId=my-web-app -DarchetypeArtifactId=maven-archetype-webapp -DinteractivMode=false -DarchetypeCatalog=internal
```
<!--more-->
出现一些版本号确认等直接回车就行，构建成功出现下面的提示。
![logo](e4dd0c75-7d51-4b91-a4c0-7a401477c461.png)
当然，这只是一个简单的web项目
## 运行
用jetty运行
在pom文件集成jetty插件
```shell
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.jjs.app</groupId>
  <artifactId>my-web-app</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>my-web-app Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  <build>
    <finalName>my-web-app</finalName>
    <pluginManagement>
        <!--配置Jetty-->
          <plugins>
            <plugin>
             <groupId>org.mortbay.jetty</groupId>
             <artifactId>maven-jetty-plugin</artifactId>
            </plugin>
          </plugins>
    </pluginManagement>
  </build>
</project>
```
然后执行：mvn jetty:run 就可以在8080端口上访问应用了。
http://127.0.0.1:8080/my-web-app/index.jsp

