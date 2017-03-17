---
title: Freemarker中对List进行排序
date: 2017-03-17 14:23:07
category: freeMarker
tags: [freeMarker]
---
# Freemarker中对List进行排序
通常我们的排序操作都是通过DAO层来实现的，如果我们想随时更改我们的排序，那么就必须修改我们的DAO层代码，
确实不方便。但Freemarker为我们提供了这样的排序方法，解决了这个问题。
<!--more-->
## sort升序排序函数
sort对序列(sequence)进行排序，要求序列中的变量必须是：字符串（按首字母排序）,数字，日期值。
```shell
<#list list?sort as l>…</#list>
```
## sort_by函数
sort_by有一个参数,该参数用于指定想要排序的子变量，排序是按照变量对应的值进行排序,如：
```shell
<#list userList?sort_by(“age”) as user>…</#list>
```
age是User对象的属性，排序是按age的值进行的。
## reverse降序排序函数
``` shell
<#list list? reverse as l>…</#list>
```
reverse使用同sort相同。reverse还可以同sort_by一起使用
如：想让用户按年龄降序排序，那么可以这个样写
```shell
<#list userList?sort_by(“age”)?reverse as user>…</#list>
```
"_index"是FreeMarker中对于循环索引变量的命名约定。
通过“迭代变量名_index”即可访问当前循环索引。
类似的循环状态访问约定还有“迭代变量名_has_next”，可通过这个循环状态属性判断是否还有后继循环。
因很多时候要用到“序号”、“索引”之类的东西，而FreeMarker刚好自己带有，但很多人还并不知道它的妙用。
因此就从Spring开发指南摘录下来。这样可以避免另申请一个变量，然后每次循环体又对它+1了。不知道是否提到过这个。
```shell
<#list subDir as d>
    <input type="checkbox" name="ids" value="file-${d_index}"/>
</#list>
```