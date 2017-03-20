layout: w
title: 调整maven项目编译jdk版本
date: 2017-03-20 17:08:47
category: [maven]
tags: [maven]
---
　　新建maven项目的时候，手动更改JDK版本号，每次update maven项目的时候，项目的JDK版本都会变成1.5版(这个或许是maven的bug吧)，后来发现可以在设置maven全局的jdk编译版本，或者[设置全局maven默认jdk编译版本](http://jjs857012499.github.io/2017/03/19/maven/调整maven编译jdk版本/)
<!--more-->
解决方案：
在pom文件中添加如下代码：
```xml
<build>
    <finalName>demo</finalName>
       <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.0</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <encoding>UTF-8</encoding>
                    <verbal>true</verbal>
                </configuration>
            </plugin>
        </plugins>
</build>
```
然后update maven项目。OK

