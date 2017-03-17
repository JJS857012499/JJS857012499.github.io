---
title: union和union all区别
date: 2017-03-13 10:51:00
categories: mysql
tags: [mysql,union,union all]
---
UNION 操作符用于合并两个或多个 SELECT 语句的结果集。
请注意，UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。

注释：默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。

另外，UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。
