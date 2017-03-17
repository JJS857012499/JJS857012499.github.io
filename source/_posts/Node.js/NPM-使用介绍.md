---
title: NPM 使用介绍
date: 2017-03-13 00:04:20
categories: Node.js
tags: [Node.js,npm]
---
NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：
允许用户从NPM服务器下载别人编写的第三方包到本地使用。
允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。
由于新版的nodejs已经集成了npm，所以之前npm也一并安装好了。同样可以通过输入 "npm -v" 来测试是否成功安装。
<!-- more -->
## 全局安装与本地安装 npm install <模块名>
npm 的包安装分为本地安装（local）、全局安装（global）两种，从敲的命令行来看，差别只是有没有-g而已，比如
``` shell
npm install express          # 本地安装
npm install express -g       # 全局安装
```
### 本地安装
1. 将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。
2. 可以通过 require() 来引入本地安装的包。
### 全局安装
1. 将安装包放在 /usr/local 下。
2. 可以直接在命令行里使用。
3. 不能通过 require() 来引入本地安装的包。
## 卸载模块
我们可以使用以下命令来卸载 Node.js 模块。
``` shell
$ npm uninstall express
```
卸载后，你可以到 /node_modules/ 目录下查看包是否还存在，或者使用以下命令查看：
``` shell
$ npm ls
```
## 更新模块
我们可以使用以下命令更新模块：
``` shell
$ npm update express
```
## 搜索模块
使用以下来搜索模块：
``` shell
$ npm search express
```