---
title: linux上多线程下载
date: 2017-03-10 16:55:48
categories: 云主机
tags:
 - 下载
 - Linux
---
常规就用weget下载
<!-- more -->
## axel
### 安装
``` shell
sudo apt install axel
```
### 用法
``` shell
jjs@jjs-thinkpad-w520:/media/jjs/MY/android$ axel
用法: axel [选项] 地址1 [地址2] [地址...]
--max-speed=x		-s x	指定最大速率（字节 / 秒）
--num-connections=x	-n x	指定最大连接数
--output=f		-o f	指定本地输出文件
--search[=x]		-S [x]	搜索镜像并从 X 服务器下载
--no-proxy		-N	不使用任何代理服务器
--quiet			-q	使用输出简单信息模式
--verbose		-v	更多状态信息
--alternate		-a	文本式进度指示器
--help			-h	帮助信息
--version		-V	版本信息
```


