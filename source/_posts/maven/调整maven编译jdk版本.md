---
title: 调整maven编译jdk版本
date: 2017-03-19 17:34:58
category: maven
tags: [maven,jdk]
---
调整maven编译jdk版本
<!--more-->
在maven 的settings.xml文件添加如下节点
```xml
<profile>
    <id>jdk-1.7</id>
    <activation>
            <activeByDefault>true</activeByDefault>
            <jdk>1.7</jdk>
    </activation>
    <properties >
            <maven.compiler.source>1.7</maven.compiler.source>
            <maven.compiler.target>1.7</maven.compiler.target>
            <maven.compiler.compilerVersion>1.7</maven.compiler.compilerVersion>
    </properties>
</profile>
```