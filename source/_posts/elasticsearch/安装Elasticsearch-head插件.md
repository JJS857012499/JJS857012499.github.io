---
title: 安装Elasticsearch-head插件
date: 2017-03-17 14:59:17
category: Elasticsearch
tags: [Elasticsearch,Elasticsearch-head]
---
elasticsearch 5.0 与elasticsearch2.0差距比较大，在这里记录下插件elasticsearch-head的安装
<!--more-->
## 下载elasticsearch-head插件
```shell
git clone https://github.com/mobz/elasticsearch-head.git
```
## 由于需要node环境，如果没有则需要安装。
## 在node环境基础上，如果没有安装grunt， 则需要安装grunt。
```shell
npm install -g grunt
```
## 进入elasticsearch-head目录下进行npm install 使用node.js安装。
```shell
npm install
```
## 启动elasticsearch-head
```shell
grunt server
```
## 修改elasticsearch配置参数，并启动
```shell
# 修改一下ES的监听地址，这样别的机器也可以访问
network.host: 0.0.0.0

# 增加新的参数，这样head插件可以访问es
http.cors.enabled: true
http.cors.allow-origin: "*"
```

