---
title: decorator（HTML装饰器）
date: 2017-03-20 17:48:35
category: jee
tags: [decorator]
---
1. 首先这个Decorator解释一下这个单词：“装饰器”，我觉得其实可以这样理解，他就像我们用到的Frame，他把每个页面都有的东东提炼了出来，也可能我们也会用各种各样的include标签，将我们的常用页面给包括进来：比如说页面的top,bottom这些每个页面几乎都有，而且都一样，如果我们在每个页面都include,可以发现我们的程序是多吗的冗余，重复。相比之下装饰器给我们提供了一个较好的选择，他在你要显示的页面根本看不出任何include信息，可以说完全解耦。
<!--more-->
2. decorator的原理：
sitemesh应用Decorator模式，用filter截取request和response,把页面组件head,content,banner、bottom结合为一个完整的视图。通常我们都是用include标签在每个jsp页面中来不断的包含各种header, stylesheet, scripts and footer.
3. decorator的实现
- 下载 jar包:  sitemesh-2.4.jar
- 在我们的web.xml中配置如下信息：
```xml
<filter>
  <filter-name>sitemesh</filter-name>
     <filter-class>com.opensymphony.module.sitemesh.filter.PageFilter</filter-class>
  </filter>
  <filter-mapping>
     <filter-name>sitemesh</filter-name>
     <url-pattern>/*</url-pattern>
 </filter-mapping>
```
- 在WEB-INF目录下新建一个decorators.xml文件（/decorator是你的包装jsp根路径在这里main.jsp和panel.jsp都是包装jsp,a.jsp，b,jsp是被包装jsp）
defaultdir: 包含装饰器页面的目录
page : 页面文件名
name : 别名
role : 角色，用于安全
webapp : 可以另外指定此文件存放目录
Patterns : 匹配的路径，可以用*,那些被访问的页面需要被装饰。
```xml
 <decorators defaultdir="/decorator">
    <decorator name="main" page="main.jsp">
      <pattern>/page/a.jsp</pattern>
      <pattern>/page/b.jsp</pattern>
 </decorator>
 <decorator name="panel" page="panel.jsp"></decorator>
 </decorators>
```

- 建立我们的包装jsp在WEBROOT->decorator下面：这里有两个分别是main.jsp和panel.jsp
    panel.jsp
    ```xml
    <decorator:head />
    ```
    插入原始页面(被包装页面)的head标签中的内容(不包括head标签本身)。
    ```xml
    <decorator:body />
    ```
    插入原始页面(被包装页面)的body标签中的内容。
    ```xml
    <decorator:title [ default="..." ] />
    ```
    插入原始页面(被包装页面)的title标签中的内容，还可以添加一个缺省值。

- 下面介绍一下<page:applyDecorator name="  " page=" ">
其实这里是一样的name指的是我们要用的包装器名字也就是在decorator.xml中定义好的，page指的是被包装页面。
还有就是<decorator:getProperty property="" [default=""][writeEntireProperty=""]/>
插入原始页面的property属性指定的值同名的属性。
property:指定那个属性将要被插入
default:如果没有发现指定的属性，则插入此值
writeEntireProperty:表示是否将（空格 属性名=“属性值”）整个插入，允许时的值是true或yes或1







