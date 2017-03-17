---
title: FreeMarker list的遍历
date: 2017-03-17 14:29:56
category: freeMarker
tags: [freeMarker]
---
# freemarker List的遍历
Freemarker中如何遍历List摘要：在Freemarker应用中经常会遍历List获取需要的数据，
并对需要的数据进行排序加工后呈现给用户。那么在Freemarker中如何遍历List，
并对List中数据进行适当的排序呢？
通过下文的介绍，相信您一定会找到答案。
<!--more-->
## 一、 Freemarker中list指令简单介绍
要想在Freemarker中遍历list,必须通过使用list指令,即<#list sequence as item>…</#list>
sequence是集合(collection)的表达式，item是循环变量的名字，不能是表达式。
当在遍历sequence时，会将遍历变量的值保存到item中。
举个例子说明吧：
```shell
<#list userList as user>
…
</#list>
```
userList中封装了很多个User对象，我们在遍历userList时候，会将遍历的User对象的值，保存到上述的user变量中。
那么在取值时，我们可以通过${user.userName }来获取User对象的userName属性值。
List指令还隐含了两个循环变量：
item_index:当前迭代项在所有迭代项中的位置，是数字值。
item_has_next:用于判断当前迭代项是否是所有迭代项中的最后一项。
注意：在使用上述两个循环变量时，一定要将item换成你自己定义的循环变量名,item其实就是前缀罢了。
例如，如果你使用<# list list as l>..</#list>定义，那么就要使用l_index,l_has_next。
在循环过程中，如果您想跳出循环，那么可以使用结合break指令，即<#break>来完成。
