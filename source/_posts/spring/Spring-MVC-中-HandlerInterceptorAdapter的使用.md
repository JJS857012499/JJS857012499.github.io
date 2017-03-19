---
title: Spring MVC 中 HandlerInterceptorAdapter的使用
date: 2017-03-19 22:38:07
category: spring
tags: [spring,拦截器]
---
一般情况下，对来自浏览器的请求的拦截，是利用Filter实现的，这种方式可以实现Bean预处理、后处理。
 <!--more-->
Spring MVC的拦截器不仅可实现Filter的所有功能，还可以更精确的控制拦截精度。
Spring为我们提供了org.springframework.web.servlet.handler.HandlerInterceptorAdapter这个适配器，继承此类，可以非常方便的实现自己的拦截器。
```java
public abstract class HandlerInterceptorAdapter implements AsyncHandlerInterceptor {
	/**
	 * This implementation always returns {@code true}.
	 */
	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
		throws Exception {
		return true;
	}
	/**
	 * This implementation is empty.
	 */
	@Override
	public void postHandle(
			HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView)
			throws Exception {
	}
	/**
	 * This implementation is empty.
	 */
	@Override
	public void afterCompletion(
			HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
			throws Exception {
	}
	/**
	 * This implementation is empty.
	 */
	@Override
	public void afterConcurrentHandlingStarted(
			HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
	}
}
```
分别实现预处理、后处理（调用了Service并返回ModelAndView，但未进行页面渲染）、返回处理（已经渲染了页面）
在preHandle中，可以进行编码、安全控制等处理；
在postHandle中，有机会修改返回结果；
在afterCompletion中，可以根据ex是否为null判断是否发生了异常，进行日志记录。

下面以一个权限拦截为demo
1、
```java
public class SecurityInterceptor extends HandlerInterceptorAdapter {
	protected static Logger log = LoggerFactory.getLogger(SecurityInterceptor.class.getName());
	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
		if (handler instanceof HttpRequestHandler || handler instanceof ResourceHttpRequestHandler) {
			return true;
		}
		// 从切点上获取目标方法
		HandlerMethod handlerMethod = (HandlerMethod) handler;
		Method method = handlerMethod.getMethod();
		// 若目标方法忽略了安全性检查，则直接调用目标方法
		if (method.isAnnotationPresent(IgnoreSecurity.class)) {
			return true;
		}
		Map<String, String[]> parameterMap = request.getParameterMap();
		TreeMap<String, String> data = new TreeMap<>();
		for (String key : parameterMap.keySet()) {
			data.put(key, parameterMap.get(key)[0]);
		}
		boolean verferSignData = SignUtil.verferSignData(data, ParamConstant.RSA_PUBLIC_KEY);
		if (!verferSignData) {
			log.error("参数验签失败！！！");
		}
		// 调用目标方法
		return verferSignData;
	}
}
```

2、配置拦截器
在spring-mvc.xml添加如下配置
```xml
<!-- 拦截器  对请求数据 进去签名校验-->
	<mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <mvc:exclude-mapping path="/static/**"/>
            <bean id="securityInterceptor" class="com.zfull.pro.wx.api.sercurity.SecurityInterceptor">
            </bean>
        </mvc:interceptor>
    </mvc:interceptors>
```

