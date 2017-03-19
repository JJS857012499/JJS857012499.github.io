---
title: Spring Data JPA 初步了解
date: 2017-03-19 22:40:56
category: spring
tags: [spring,spring data jpa]
---
Spring Data JPA
Spring Data是什么
Spring Data是一个用于简化数据库访问，并支持云服务的开源框架。其主要目标是使得对数据的访问变得方便快捷，并支持map-reduce框架和云计算数据服务。
<!--more-->
Spring Data JPA是什么
由Spring提供的一个用于简化JPA开发的框架
nSpring Data JPA能干什么
可以极大的简化JPA的写法，可以在几乎不用写实现的情况下，实现对数据的访问和操作。除了CRUD外，还包括如分页、排序等一些常用的功能。

Spring Data JPA有什么
主要来看看Spring Data JPA提供的接口，也是Spring Data JPA的核心概念：
1：Repository：最顶层的接口，是一个空的接口，目的是为了统一所有Repository的类型，且能让组件扫描的时候自动识别。
2：CrudRepository ：是Repository的子接口，提供CRUD的功能
3：PagingAndSortingRepository：是CrudRepository的子接口，添加分页和排序的功能
4：JpaRepository：是PagingAndSortingRepository的子接口，增加了一些实用的功能，比如：批量操作等。
5：JpaSpecificationExecutor：用来做负责查询的接口
6：Specification：是Spring Data JPA提供的一个查询规范，要做复杂的查询，只需围绕这个规范来设置查询条件即可


来源：[http://sishuok.com/forum/posts/list/7000.html](http://sishuok.com/forum/posts/list/7000.html)
