---
title: mysql远程连接
date: 2017-03-13 11:03:22
categories: mysql
tags: [mysql]
---
## 在云主机上连接进入mysql
<!-- more -->
##　执行以下mysql命令：
``` shell
use mysql                #打开mysql数据库
#将host设置为%表示任何ip都能连接mysql，当然您也可以将host指定为某个ip
update user set host='%' where user='root' and host='localhost';
flush privileges;        #刷新权限表，使配置生效
```
然后我们就能远程连接我们的mysql

## 如果您想关闭远程连接，恢复mysql的默认设置（只能本地连接），您可以通过以下步骤操作：
``` shell
use mysql                #打开mysql数据库
#将host设置为localhost表示只能本地连接mysql
update user set host='localhost' where user='root';
flush privileges;        #刷新权限表，使配置生效
```
备注：您也可以添加一个用户名为yuancheng，密码为123456，权限为%（表示任意ip都能连接）的远程连接用户。命令参考如下：
``` shell
grant all on *.* to 'yuancheng'@'%' identified by '123456';
flush privileges;
```
