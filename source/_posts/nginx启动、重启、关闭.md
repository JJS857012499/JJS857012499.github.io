---
title: nginx启动、重启、关闭
date: 2017-03-09 18:11:53
categories: 云主机
tags:
 - nginx
 - Linux
---
## 启动
``` shell
cd usr/local/nginx/sbin
./nginx
```
## 更改配置重启nginx　　
kill -HUP 主进程号或进程号文件路径
或者使用
### 判断配置文件是否正确　
``` shell
nginx -t -c /usr/local/nginx/conf/nginx.conf
```
或者
``` shell
cd  /usr/local/nginx/sbin
./nginx -t
```
### 重启
``` shell
cd /usr/local/nginx/sbin
./nginx -s reload
```

## 关闭
### 查询nginx主进程号
``` shell
ps -ef | grep nginx
```
　　从容停止   kill -QUIT 主进程号

　　快速停止   kill -TERM 主进程号

　　强制停止   kill -9 nginx

　　若nginx.conf配置了pid文件路径，如果没有，则在logs目录下

　　kill -信号类型 '/usr/local/nginx/logs/nginx.pid'

## 升级

　　1、先用新程序替换旧程序文件

　　2、kill -USR2 旧版程序的主进程号或者进程文件名

　　　　此时旧的nginx主进程会把自己的进程文件改名为.oldbin，然后执行新版nginx，此时新旧版本同时运行

　　3、kill -WINCH 旧版本主进程号

　　4、不重载配置启动新/旧工作进程

　　　　kill -HUP 旧/新版本主进程号

　　　　从容关闭旧/新进程

　　　　kill -QUIT 旧/新进程号

　　　　快速关闭旧/新进程

　　　　kill -TERM 旧/新进程号
