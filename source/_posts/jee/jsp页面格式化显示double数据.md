---
title: jsp页面格式化显示double数据
date: 2017-03-20 16:48:10
category: jee
tags: [jee,double]
---
```jsp
<fmt:formatNumber>
    此标签会根据区域定制的方式将数字格式化成数字，货币，百分比。
    此标签的属性：
    value:要格式化的数字
    type：按照什么类型格式化
    pattern：自定义格式化样式
    currencyCode:ISO-4721货币代码，只适用于按照货币格式化的数字
    currencySymbol： 货币符号,如￥,只适用于按照货币格式化的数字
    groupingUsed： 是否包含分隔符
    maxIntegerDigits： 整数部分最多显示多少位
    mixIntegerDigits： 整数部分最少显示多少位
    maxFractionDigits： 小数部分最多显示多位位
    minFractionDigits： 小数部分最少显示多位位
    var:存储格式化后的结果
    scope: 存储的范围
```
pattern 符号意义:

|符号     |             意义|
|:---------:|:---------------:|
|0          |            一个数位|
|#          |           一个数位，前导零和追尾零不显示|
|.          |             小数点分割位置|
|，         |             组分隔符的位置|
|-          |            负数前缀|
|%          |          用100乘，并显示百分号|
|其他任何符号|    在输出字符串中包括指定符号|


