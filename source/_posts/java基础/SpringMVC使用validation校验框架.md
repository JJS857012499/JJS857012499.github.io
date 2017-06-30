---
title: SpringMVC使用validation校验框架
date: 2017-06-30 16:44:19
category: java基础
tags: [validation, valid]
---
今天遇到一个问题，用了valid框架，发现不能级联去校验。
<!--more-->
最后发现@Valid这个注解解决了我的问题，在这里要感谢一下我的同学==》荣老板的指点，如果需要联系方式，可以私聊我哦！！！
![logo](@DW1ANHCV5RXGF56$G$O.png)

Valid源码：
```java
/**
 * Marks a property, method parameter or method return type for validation cascading.
 * <p/>
 * Constraints defined on the object and its properties are be validated when the
 * property, method parameter or method return type is validated.
 * <p/>
 * This behavior is applied recursively.
 *
 * @author Emmanuel Bernard
 * @author Hardy Ferentschik
 */
@Target({ METHOD, FIELD, CONSTRUCTOR, PARAMETER })
@Retention(RUNTIME)
public @interface Valid {
}
```
上面的注释大意是：让属性、方法参数或者方法返回一个级联的校验类型。定义属性、方法参数或者方法返回值是一个校验类型的约束
我理解为：验证值是否需要递归验证

Validated源码
```java
/**
 * Variant of JSR-303's {@link javax.validation.Valid}, supporting the
 * specification of validation groups. Designed for convenient use with
 * Spring's JSR-303 support but not JSR-303 specific.
 *
 * <p>Can be used e.g. with Spring MVC handler methods arguments.
 * Supported through {@link org.springframework.validation.SmartValidator}'s
 * validation hint concept, with validation group classes acting as hint objects.
 *
 * <p>Can also be used with method level validation, indicating that a specific
 * class is supposed to be validated at the method level (acting as a pointcut
 * for the corresponding validation interceptor), but also optionally specifying
 * the validation groups for method-level validation in the annotated class.
 * Applying this annotation at the method level allows for overriding the
 * validation groups for a specific method but does not serve as a pointcut;
 * a class-level annotation is nevertheless necessary to trigger method validation
 * for a specific bean to begin with. Can also be used as a meta-annotation on a
 * custom stereotype annotation or a custom group-specific validated annotation.
 *
 * @author Juergen Hoeller
 * @since 3.1
 * @see javax.validation.Validator#validate(Object, Class[])
 * @see org.springframework.validation.SmartValidator#validate(Object, org.springframework.validation.Errors, Object...)
 * @see org.springframework.validation.beanvalidation.SpringValidatorAdapter
 * @see org.springframework.validation.beanvalidation.MethodValidationPostProcessor
 */
@Target({ElementType.TYPE, ElementType.METHOD, ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Validated {

	/**
	 * Specify one or more validation groups to apply to the validation step
	 * kicked off by this annotation.
	 * <p>JSR-303 defines validation groups as custom annotations which an application declares
	 * for the sole purpose of using them as type-safe group arguments, as implemented in
	 * {@link org.springframework.validation.beanvalidation.SpringValidatorAdapter}.
	 * <p>Other {@link org.springframework.validation.SmartValidator} implementations may
	 * support class arguments in other ways as well.
	 */
	Class<?>[] value() default {};

}
```

@Valid是JSR-303的javax.validation里的，支持组校验
@Validated是@Valid的一次封装，不是规范。
因此推荐在Springmvc的controller用@Validated

[http://blog.csdn.net/ljhabc1982/article/details/18728239](http://blog.csdn.net/ljhabc1982/article/details/18728239)

