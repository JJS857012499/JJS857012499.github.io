---
title: MySql查询不区分大小写解决方案
date: 2017-07-13 17:19:50
category: mysql
tags: [mysql, 区分大小写]
---
    今天发现mysql查询条件中，对大小写不敏感。
```mysql
SELECT * from category_cert_require where CERT_NAME='aa';
SELECT * from category_cert_require where CERT_NAME='AA';
```
如上，两个查询的结果是一样的。
[!logo](LERBY1SAEWYDWPT.png)

## 通过查询资料发现需要设置collate（校对） 。 collate规则：
* _bin: 表示的是binary case sensitive collation，也就是说是区分大小写的
* _cs: case sensitive collation，区分大小写
* _ci: case insensitive collation，不区分大小写

## 解决方法
### 可以将查询条件用binary()括起来。  比如：
select * from TableA where binary columnA ='aaa';
### 可以修改该字段的collation 为 binary
比如：
```mysql
ALTER TABLE TABLENAME MODIFY COLUMN COLUMNNAME VARCHAR(50) BINARY CHARACTER SET utf8 COLLATE utf8_bin DEFAULT NULL;
```
## 原理
对于CHAR、VARCHAR和TEXT类型，BINARY属性可以为列分配该列字符集的 校对规则。BINARY属性是指定列字符集的二元 校对规则的简写。排序和比较基于数值字符值。因此也就自然区分了大小写。

来源：[http://blog.csdn.net/followmyinclinations/article/details/50822963](http://blog.csdn.net/followmyinclinations/article/details/50822963)
[http://blog.csdn.net/yippeelyl/article/details/39695399](http://blog.csdn.net/yippeelyl/article/details/39695399)

