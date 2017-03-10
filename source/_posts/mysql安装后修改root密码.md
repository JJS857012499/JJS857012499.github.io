---
title: mysql安装后修改root密码
date: 2017-03-10 17:38:46
categories: 云主机
tags:
 - mysql
 - Linux
---
提示：ERROR 1044 (42000): Access denied for user ''@'localhost' to database 'MySQL'。安装mysql后就，开始不知道密码是空，自己捣鼓了半天也没弄好。后来根据网上的资料，修改root的密码后，还是不行，大多数的操作方法如下：
关闭mysql
```shell
service mysqld stop
```
屏蔽权限
```shell
mysqld_safe --skip-grant-table &
```
屏幕出现： Starting demo from .....
新开起一个终端输入
```shell
mysql -u root mysql
mysql> UPDATE user SET Password=PASSWORD('newpassword') where USER='root';
mysql> FLUSH PRIVILEGES;
mysql> \q
```
来源：[http://blog.csdn.net/laiwenqiang/article/details/12833937](http://blog.csdn.net/laiwenqiang/article/details/12833937)