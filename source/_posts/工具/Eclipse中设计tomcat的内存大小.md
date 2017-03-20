---
title: Eclipse中设计tomcat的内存大小
date: 2017-03-20 17:36:25
category: 工具
tags: [eclipse,tomcat,内存]
---
　　有时候项目需要的内存比较大，需要更大的jvm内存，因此我们需要修改默认的tomcat内存
<!--more-->
1  双击server，在配置页面打开tomcat启动配置文件
![logo](45ca9205-f30f-42fe-97b3-3efee151ba7d.png)
2 在Argument中末尾添加参数中的VM arguments中追加：
```xml
-Xms512M -Xmx1024M -XX:PermSize=512m -XX:MaxPermSize=1024
```
![logo](6295420a-7dd6-4bc7-9001-c964155695b3.png)
备注：
参数的意思
-vmargs：说明后面是VM的参数
-Xms40m：虚拟机占用系统的最小内存
-Xmx256m：虚拟机占用系统的最大内存
-XX:PermSize：最小栈内存大小。一般报内存不足时,都是说这个太小,堆空间剩余小于5%就会警告,建议把这个稍微设大一点,不过要视自己机器内存大小来设置
-XX:MaxPermSize：最大栈内存大小。这个也适当大些
-Xmx512M的5%为25.6M，理论上要求-Xmx的数值与-XX:MaxPermSize必须大于25.6M
来源： [http://blog.csdn.net/w420372197/article/details/7878404](http://blog.csdn.net/w420372197/article/details/7878404)

