---
title: MySql中Null字段的排序问题
date: 2017-12-25 14:31:02
category: mysql
tags: [mysql,Null,排序]
---
    MySQL中order by 排序遇到NULL值的问题 MySQL数据库，在order by排序的时候，如果存在NULL值，那么NULL是最小的，ASC正序排序的话，NULL值是在最前面的。 如果我们想让NULL排在后面，让非NULL的行排在前面该怎么做呢？
    这里有个需要注意的事项，就是NULL值本身是无法排序的，也就是说一个NULL是无法和另外一个NULL比较的。 
### 解决方法一
* 重新生成一列，比如agenull，利用is null操作符，把NULL值的行变成1，非NULL值的行变成0，先对该字段排序，再对age排序
### 解决方法二
* 直接利用isnull函数对age列求值，跟第一种方法的道理是一样的    
### 解决方法三
* 可以利用MySQL中的一个小技巧，在字段前面加上一个负号，也就是减号，ASC改成DESC ，DESC改成ASC

来源：[http://www.th7.cn/db/mysql/201606/195206.shtml](http://www.th7.cn/db/mysql/201606/195206.shtml)