---
title: HttpServletRequest常用获取URL的方法
date: 2017-03-19 21:21:51
category: jee
tags: [HttpServletRequest,URL]
---
　　HttpServletRequest常用获取URL的方法
1. 获取的方法
<!--more-->
```java
String requestURL = request.getRequestURL().toString(); //返回的是完整的url，包括Http协议，端口号，servlet名字和映射路径，但它不包含请求参数。
String requestURI = request.getRequestURI();    //得到的是request URL的部分值，并且web容器没有decode过的
String contextPath = request.getContextPath();    //返回 the context of the request.
String servletPath = request.getServletPath();    //返回调用servlet的部分url.
String queryString = request.getQueryString();    //返回url路径后面的查询字符串

log.info("requestURL:" + requestURL);
log.info("requestURI:" + requestURI);
log.info("contextPath:" + contextPath);
log.info("servletPath:" + servletPath);
log.info("queryString:" + queryString);
```
2. 上面的例子打印如下
![7a5e6bc6-ef4c-461b-8a38-a654646c3d44.png](7a5e6bc6-ef4c-461b-8a38-a654646c3d44.png)


