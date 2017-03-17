---
title: FreeMarker对null值得处理
date: 2017-03-17 14:37:24
category: freeMarker
tags: [freeMarker]
---
## 判断是否存在,通过exists关键字或者"??"运算符。都将返回一个布尔值
user.name?exists
user.name??
<!--more-->
```shell
<#if user.name?exists>
 //TO DO
</#if>

<#if user.age??>
 //TO DO
</#if>
```

## 忽略null值
假设前提：user.name为null
${user.name}，异常
${user.name!},显示空白
${user.name!'vakin'}，若user.name不为空则显示本身的值，否则显示vakin
${user.name?default('vakin')}，同上
${user.name???string(user.name,'vakin')},同上

来源： [http://www.cnblogs.com/modou/articles/2577450.html](http://www.cnblogs.com/modou/articles/2577450.html)