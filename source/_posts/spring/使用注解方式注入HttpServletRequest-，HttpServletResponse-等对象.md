---
title: 使用注解方式注入HttpServletRequest ，HttpServletResponse 等对象
date: 2017-03-19 22:35:27
category: spring
tags: [spring,注解]
---
## 在web.xml添加spring 的 ContextLoaderListener  监听器
```xml
<listener>
  		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```
## 在Controller可以注入如下对象
```java
    @Autowired
	protected HttpServletRequest request;
	@Autowired
	protected HttpServletResponse response;
	@Autowired
	protected HttpSession session;
	@Autowired
	protected ServletContext application;
```

