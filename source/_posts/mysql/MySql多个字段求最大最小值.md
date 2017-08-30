---
title: MySql多个字段求最大最小值
date: 2017-08-30 16:09:02
category: mysql
tags: [mysql,最大值，最小值]
---
一般查询一列中的最大值或者最小值，我们一般用max或者min函数。如果查询是多列的话，可以用COALESCE/GREATEST或LEAST。
```mysql
 COALESCE(value1, value2, value3, ...) #返回value列表第一个非空的值,value列表必须是相同类型
 GREATEST(value1, value2, value3, ...)  #返回值列表中最大值,value列表必须是相同类型,当value值列表中有一个为NULL，则返回NULL值
 LEAST(value1, value2, value3, ...)  #返回value列表最小的值,value列表必须是相同类型,当value值列表中有一个为NULL，则返回NULL值。
```
来源：[http://blog.csdn.net/yangshijin1988/article/details/51698061](http://blog.csdn.net/yangshijin1988/article/details/51698061)
[http://dlblog.iteye.com/blog/1274005](http://dlblog.iteye.com/blog/1274005)

