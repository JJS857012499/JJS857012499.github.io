---
title: git冲突解决
date: 2017-03-11 10:54:21
categories: 云主机
tags:
 - git
 - Linux
---
背景：用git pull 时，发现报如下错误
```` shell
Please, commit your changes or stash them before you can merge.
````
<!-- more -->
## stash
通常遇到这个问题，你可以直接commit你的修改；但我这次不想这样。
看看git stash是如何做的。
``` shell
git stash
git pull
git stash pop
```
接下来diff一下此文件看看自动合并的情况，并作出相应修改。
git stash: 备份当前的工作区的内容，从最近的一次提交中读取相关内容，让工作区保证和上次提交的内容一致。同时，将当前的工作区内容保存到Git栈中。
git stash pop: 从Git栈中读取最近一次保存的内容，恢复工作区的相关内容。由于可能存在多个Stash的内容，所以用栈来管理，pop会从最近的一个stash中读取内容并恢复。
git stash list: 显示Git栈内的所有备份，可以利用这个列表来决定从那个地方恢复。
git stash clear: 清空Git栈。此时使用gitg等图形化工具会发现，原来stash的哪些节点都消失了。

## 放弃本地修改，直接覆盖之
``` shell
git reset --hardgit pull
```