---
title: FreeMarker 基础知识 表达式(引用)
date: 2017-03-17 11:48:10
category: freeMarker
tags: [freeMarker]
---
FreeMarker 基础知识 表达式(引用)
表达式是FreeMarker的核心功能，FreeMarker中的插值支持多种表达式。
<!--more-->
## 一、直接指定值
直接指定值可以是字符串、数值、布尔值、集合及Map对象。
1. 字符串
直接指定字符串值使用单引号或双引号限定。字符串中可以使用转义字符”\"。如果字符串内有大量的特殊字符，则可以在引号的前面加上一个字母r，则字符串内的所有字符都将直接输出。
2. 数值
数值可以直接输入，不需要引号。FreeMarker不支持科学计数法。
3. 布尔值
直接使用true或false，不使用引号。
4. 集合
集合用中括号包括，集合元素之间用逗号分隔。
使用数字范围也可以表示一个数字集合，如1..5等同于集合[1, 2, 3, 4, 5]；同样也可以用5..1来表示[5, 4, 3, 2, 1]。
5. Map对象
Map对象使用花括号包括，Map中的key-value对之间用冒号分隔，多组key-value对之间用逗号分隔。
注意：Map对象的key和value都是表达式，但key必须是字符串。
## 二、输出变量值
FreeMarker的表达式输出变量时，这些变量可以是顶层变量，也可以是Map对象的变量，还可以是集合中的变量，并可以使用点（.）语法来访问Java对象的属性。
1. 顶层变量
所谓顶层变量就是直接放在数据模型中的值。输出时直接用${variableName}即可。
2. 输出集合元素
可以根据集合元素的索引来输出集合元素，索引用中括号包括。如： 输出[“1”， “2”， “3”]这个名为number的集合，可以用${number[0]}来输出第一个数字。
FreeMarker还支持用number[1..2]来表示原集合的子集合[“2”， “3”]。
3. 输出Map元素
对于JavaBean实例，FreeMarker一样把它看作属性为key，属性值为value的Map对象。
输出Map对象时，可以使用点语法或中括号语法，如下面的几种写法的效果是一样的：
             book.author.name
             book.author["name"]
             book["author"].name
             book["author"]["name"]
使用点语法时，变量名字有和顶层变量一样的限制，但中括号语法没有任何限制。
## 三、字符串操作
1. 字符串连接
 字符串连接有两种语法：
（1） 使用${..}或#{..}在字符串常量内插入表达式的值；
(2)  直接使用连接运算符“+”连接字符串。
如，下面两种写法等效：
              ${"Hello, ${user}"}
              ${"Hello, " + user + "!"}
有一点需要注意： ${..}只能用于文本部分作为插值输出，而不能用于比较等其他用途，如：
              <#if ${isBig}>Wow!</#if>
              <#if "${isBig}">Wow!</#if>
应该写成：
              <#if isBig>Wow!</#if>
2. 截取子串
截取子串可以根据字符串的索引来进行，如果指定一个索引值，则取得字符串该索引处的字符；如果指定两个索引值，则截取两个索引中间的字符串子串。如：
              <#assign number="01234">
              ${number[0]} <#-- 输出字符0 -->
              ${number[0..3]} <#-- 输出子串“0123” -->四、集合连接操作
      连接集合的运算符为“+”
## 五、Map连接操作
Map连接操作的运算符为“+”
## 六、算术运算符
FreeMarker表达式中支持“+”、“－”、“*”、“/”、“%”运算符。
## 七、比较运算符
表达式中支持的比较运算符有如下几种：
1. =（或者==）： 判断两个值是否相等；
2. !=： 判断两个值是否不相等；
注： =和!=可以用作字符串、数值和日期的比较，但两边的数据类型必须相同。而且FreeMarker的比较是精确比较，不会忽略大小写及空格。
3. >（或者gt）： 大于
4. >=（或者gte）： 大于等于
5. <（或者lt）： 小于
6. <=（或者lte）： 小于等于
注： 上面这些比较运算符可以用于数字和日期，但不能用于字符串。大部分时候，使用gt比>有更好的效果，因为FreeMarker会把>解释成标签的结束字符。可以使用括号来避免这种情况，如：<#if (x>y)>。
## 八、逻辑运算符
1. &&： 逻辑与；
2. ||： 逻辑或；
3. !： 逻辑非
逻辑运算符只能用于布尔值。
## 九、内建函数
FreeMarker提供了一些内建函数来转换输出，可以在任何变量后紧跟?，?后紧跟内建函数，就可以通过内建函数来转换输出变量。字符串相关常用的内建函数：
1. html： 对字符串进行HTML编码；
2. cap_first： 使字符串第一个字母大写；
3. lower_case： 将字符串转成小写；
4. upper_case： 将字符串转成大写；集合相关常用的内建函数：
1. size： 获得集合中元素的个数；数字值相关常用的内建函数：
1. int： 取得数字的整数部分。十、空值处理运算符
FreeMarker的变量必须赋值，否则就会抛出异常。而对于FreeMarker来说，null值和不存在的变量是完全一样的，因为FreeMarker无法理解null值。
FreeMarker提供两个运算符来避免空值：
1. !： 指定缺失变量的默认值；
2. ??：判断变量是否存在。
!运算符有两种用法：variable!或variable!defaultValue。第一种用法不给变量指定默认值，表明默认值是空字符串、长度为0的集合、或长度为0的Map对象。
使用!运算符指定默认值并不要求默认值的类型和变量类型相同。
??运算符返回布尔值，如：variable??，如果变量存在，返回true，否则返回false。

freemarker 的内建函数 contains 的使用
freemarker 的内建函数 contains 的使用 关键字: freemarker contains
freemarker 的内建函数 contains 的使用:
<#if employee.departments?contains(department)>checked="checked"</#if> 其中departments是一个集合，而department是departments集合里的一个元素。contains函数可以判断出，元素 department是否存在于集合departments里，最终返回一个Booleancontains是freemarker的内建函数，即自带的 。
FreeMarker 的内建函数有：
chunk,  is_date,  last,  root,  j_string,  round,  contains,  is_hash,  long,  float,  ends_with,  namespace,  matches,  time,  values,  seq_last_index_of,  uncap_first,  byte,  substring,  is_transform,  web_safe,  groups,  seq_contains,  is_macro,  index_of,  word_list,  int,  is_method,  eval,  parent,  xml,  number,  capitalize,  if_exists,  rtf,  node_type,  double,  is_directive,  url,  size,  default,  floor,  ceiling, is_boolean,  split,  node_name,  is_enumerable,  seq_index_of,  is_sequence,  sort,  is_node,
sort_by,  left_pad,  cap_first,  interpret,  children,  node_namespace,  chop_linebreak, date,  short,  last_index_of,  is_collection,  ancestors,  length,  trim,  datetime, is_string,  reverse,  c,  keys,  upper_case,  js_string,  has_content,  right_pad,  replace,  is_hash_ex,  new,  is_number,  lower_case,  is_indexable,  string,  exists,  html,  first


