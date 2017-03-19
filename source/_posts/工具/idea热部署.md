---
title: ida热部署
date: 2017-03-19 16:49:49
category: 工具
tags: [idea]
---
intellij idea默认文件是自动保存的，但是手头有个项目jsp文件改动后，在tomcat中不能立即响应变化。
<!--more-->
要jsp文件改动后立刻看到变化，有个配置。
在idea tomcat 中server的配置里，有个on frame deactivation，选择update classes and resources。
另外有个配置on update action，就是手动操作的时候采取什么动作，可以重启服务器，也可以像上面一样更新类和资源文件，我选的是Redeploy。
可是当前项目没有update classes and resources这个选项，有个Hot Swap classes。
这是由于服务器添加的Artifact类型问题，一般一个module对应两种类型的Artifact，一种是war，一种是war explored。
war就是已war包形式发布，当前项目是这种形式，在这种形式下on frame deactivation配置没有update classes and resources选项。
war explored是发布文件目录，选择这种形式，on frame deactivation中就出现update classes and resources选项了


