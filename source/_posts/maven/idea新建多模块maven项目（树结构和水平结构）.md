---
title: idea新建多模块maven项目（树结构和水平结构）
date: 2017-03-17 11:34:40
category: maven
tags: [maven]
---
## Maven可以创建两种结构的多模块项目，一个是树形结构，一个是水平结构，下面看图了解它们的区别：
<!--more-->
![logo](82811fb4-084a-471a-a470-12957c376cd4.png)
 水平结构和树形结构，就创建过程来说，没有什么区别，就是module的路径修改一下就可以互相转化了。

## 具体步骤
### 新建一个父工程
      File->new-->project，选择Maven，建立一个最普通的maven项目
### 右键创建好的tree父项目-->new-->module-->maven，这里的创建过程就和上面的类似了
（备注：如果是水平项目则在同个目录下）
树形结构的项目结构:
/tree
/tree/tree-dao
/tree/tree-service

水平结构的项目结构:
/horizon
/horizon-dao
/horizon-service

