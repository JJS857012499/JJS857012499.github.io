---
title: 创建普通maven项目
date: 2017-03-15 20:27:39
categories: maven
tags: [maven]
---
Maven项目中有一些约定：在项目的根目录中放置pom.xml，在src/main/Java目录中放置项目的主代码，在src/test/java目录中放置项目的测试代码。我们称这些基本的目录结构和pom.xml文件内容为项目的骨架。我们可以手工创建项目骨架，也可以使用Maven提供的Archetype快速勾勒出项目骨架。
<!-- more -->
下面使用Archetype创建项目骨架。
在D盘建一个maven工作空间 D:\demo\spring\maven>
打开命令行窗口，进入maven工作空间，执行命令 >mvn archetype:generate -DgroupId=com.jjs.app -Dartifact=my-app -DarchetypeCatalog=internal
``` shell
mvn archetype:generate -DgroupId=com.jjs.app -Dartifact=my-app -DarchetypeCatalog=internal
```
如果是第一次创建项目，会从远程仓库下载依赖包到本地仓库。
备注：
``` shell
mvn archetype:generate　 必须的格式（3.0以上用generate，用create会出错Failed to execute goal org.apache.maven.plugins:maven-archetype-plugin:2……）
　　-DgroupId　　　　　　　　 包名
　　-DartifactId　　　　　　　　项目名
　　-DarchetypeArtifactId　　 类型maven-archetype-quickstart，创建一个Java Project；maven-archetype-webapp，创建一个Web Project
　　-DinteractiveMode　　　　　　是否使用交互模式，如果为false，输入上面命令后直接创建好，否则会有控制台提示输入操作；
-DarchetypeCatalog=internal 让它不要从远程服务器上取目录。
```
创建的项目结构如下
![logo](516becd9-201b-4bbb-a49a-45dacec4195f.png)
在项目根目录下生成了一个pom.xml文件，这是maven的核心文件：
![logo](aafa98fb-2088-4af2-bcfb-804d391c81ba.png)
文件内容如下：
``` xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.jjs.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>
  <name>my-app</name>
  <url>http://maven.apache.org</url>
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```
有三个非常重要的元素：groupId、artifactId和version，这三个元素定义了一个项目的基本坐标，其他项目通过这三个元素可以定位到这个项目。
另一个重要的元素就是dependencies，该元素下可以包含多个dependency元素以声明项目的依赖。
打开源文件App.java：
``` java
package com.jjs.app;
/**
 * Hello world!
 *
 */
public class App
{
    public static void main( String[] args )
    {
        System.out.println( "Hello World!" );
    }
}
```
进入my-app目录，编译：
``` shell
mvn compile
```
在项目根目录下生成了target文件夹，里面存放了class文件。
运行：
``` shell
mvn exec:java -Dexec.mainClass="com.jjs.app.App"
```
结果：
![logo](a1d9cddc-205b-4b3f-8bfc-5ff884c30df9.png)
运行成功！
普通的默认的生命周期阶段：
validate：验证工程是否正确，所有需要的资源是否可用。
compile：编译项目的源代码。
test：使用合适的单元测试框架来测试已编译的源代码。这些测试不需要已打包和布署。
Package：把已编译的代码打包成可发布的格式，比如jar。
integration-test：如有需要，将包处理和发布到一个能够进行集成测试的环境。
verify：运行所有检查，验证包是否有效且达到质量标准。
install：把包安装在本地的repository中，可以被其他工程作为依赖来使用。
Deploy：在集成或者发布环境下执行，将最终版本的包拷贝到远程的repository，使得其他的开发者或者工程可以共享。
clean：清除先前构建的artifacts（在maven中，把由项目生成的包都叫作artifact），该命令会删除target文件夹。
site：为项目生成文档站点。

来源： [http://blog.csdn.net/zhangzeyuaaa/article/details/39698479](http://blog.csdn.net/zhangzeyuaaa/article/details/39698479)
      [http://blog.csdn.net/edward0830ly/article/details/8748986](http://blog.csdn.net/edward0830ly/article/details/8748986)

