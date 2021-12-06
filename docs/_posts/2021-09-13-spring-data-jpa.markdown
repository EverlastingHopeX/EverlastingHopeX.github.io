---
layout: single
toc: true
toc_label: "Table of contents"
title:  "Spring Data JPA"
date:   2021-09-13 15:26:00 +0800
categories: Spring
---

# JPA

JPA是Java Persistence API，是Java EE的标准的一部分，它定义了对象关系映射和管理可持续对象（persistent object）的API。JPA自身并没有提供任何实现，
仅仅提供了一些接口用于实现持续层（persistence layer）。要使用JPA需要JPA提供者提供对该标准的实现，如Hibernate和EclipseLink，而Spring Data JPA并非
JAP提供者。

# Spring Data JPA

Spring Data JPA并非是JPA提供者，而是一个在JPA提供者之上添加抽象了另一层的框架。Spring Data JPA是JPA数据访问的抽象（JPA Data Access Abstraction）

Spring Data使得DAO的实现可以完全的省略。我们只需要定义一个扩展了 `JpaRepository` 的DAO接口，Spring就会自动找到这个接口并实现它。DAO会提供一些基础的
CRUD方法，Spring JPA也提供了一些定义我们自己的访问方法的途径：

1. 直接在接口中定义新的方法
2. 使用 `@Query` 提供一个实际的JPQL查询
3. 使用Spring Data提供的高级标准和Querydsl支持
4. 使用JPA命名查询来自定义查询

## 自动生成自定义查询

当Spring Data创建一个实例时，它会分析所有接口定义的方法并试图根据方法名自动生成对应的查询。

例如方法 

```Kotlin
fun findByName(String name)
```

 会自动生成依据 `name` 查询数据库的代码。返回值为一个列表，元素为在扩展 `JpaRepository` 时指定的实体。需要注意命名转换的规则。
 
### 命名转换（Naming Convention）
 
#### 默认命名转换

Spring默认使用命名规则的是小写蛇形（lower snake case），即仅使用小写字母和下划线。需注意的是，在命名属性时需要使用驼峰式命名，才能正确地转换为对应的小写蛇形命名。

例如：firstName 会被转换为 first_name

#### 自定义命名转换

如果想要使用自定义的命名转换规则，则需要实现 `PhysicalNamingStrategy` 接口，然后在配置文件中设定命名转换规则为使用我们实现的命名转换规则

[Custom Naming Convention with Spring Data JPA [Baeldung]](https://www.baeldung.com/spring-data-jpa-custom-naming)

## 手动创建自定义查询

手动创建需要使用 `@Query` 。

参考：[Spring Data JPA @Query [Baeldung]](https://www.baeldung.com/spring-data-jpa-query)

## Pagination

当查询的结果所需空间太大时，可以指定返回类型为Page。Page不是实际的资源，而是资源的一个展示的一部分（类比数据库的视图）。当我们想要访问资源的不同部分时，可以调用
Page的方法，Spring Data JPA会自动生成对应的query（修改页面的位置）并获取数据。

[Pagination and Sorting using Spring Data JPA [Baeldung]](https://www.baeldung.com/spring-data-jpa-pagination-sorting)

## Specifications

使用Specification，我们可以简单地通过自定义Specification实现基于条件的查询。
初始化Specification的时候需要注意使用的数据格式和对应数据库需要的数据格式，有时Spring不能做到对应的转化（如把LocalDateTime转化为timestamp）。

[REST Query Language with Spring Data JPA Specifications [Baeldung]](https://www.baeldung.com/rest-api-search-language-spring-data-specifications)

参考： 
[Pagination in Spring Boot Applications [DZone]](https://dzone.com/articles/pagination-in-springboot-applications)
[REST Pagination in Spring [Baeldung]](https://www.baeldung.com/rest-api-pagination-in-spring)
[Pagination and Sorting using Spring Data JPA [Baeldung]](https://www.baeldung.com/spring-data-jpa-pagination-sorting)

# 参考资料

[What’s the difference between JPA, Hibernate and EclipseLink [Thorben Janssen]](https://thorben-janssen.com/difference-jpa-hibernate-eclipselink/#Java_Persistence_API_JPA)


[What Is the Difference Between Hibernate and Spring Data JPA? [DZone]](https://dzone.com/articles/what-is-the-difference-between-hibernate-and-sprin-1)

