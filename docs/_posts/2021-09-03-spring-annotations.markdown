---
layout: single
toc: true
toc_label: "Table of contents"
title:  "Spring Annotations"
date:   2021-09-03 18:26:00 +0800
categories: Spring
---

暂存一些新遇到的注解，之后再总结。

# Schedule

## `@Scheduled`

用于配制和安排任务。用来注解的方法应该是无返回值（如果有则被忽略）且无参数。
可以实现的配置包括: 1. 在一定延迟后执行，用于强制任务在上个执行完成后才能再次执行的情况，2. 以一定的频率执行，需注意任务不会并发执行，只有在上次任务完成后
才会进行下一次（也可添加 `@Async` 来支持并发），3. 在初始延迟后执行，适用于初始任务需要完成的情况，4. 在特定的时间和频率执行，此种方式使用了cron表达式
（[Cron Expressions [Oracle](https://docs.oracle.com/cd/E12058_01/doc/doc.1014/e12030/cron_expressions.htm)）。

其他的使用方法还包括使用properties文件设置参数而非直接在代码嵌入固定值，不再赘述。

如下为各种用法示例：

```
@Scheduled(fixDelay = 1000)
@Scheduled(fixRate = 1000)
@Scheduled(fixDelay = 1000, initialDelay = 1000)
@Scheduled(cron = "* * * * * *")
```

### 测试方法

·@Scheduled` 的测试方法包括：
1. 集成测试.
2. Awaitility。

首先需要新建 SchduledConfig 类并添加 `@EnableSchduling` 注解。然后就可以采用集成测试的方法或者使用Awaitility的方法，两者区别仅在于注入方法采用 `@Autowire` 还是 `@SpyBean`，以及Awaitiliy提供的方法可读性更好。
（参考 [How to Test the @Scheduled Annotation [Baeldung]](https://www.baeldung.com/spring-testing-scheduled-annotation)

#### Awaitility

>Awaitility is a DSL that allows you to express expectations of an asynchronous system in a concise and easy to read manner.
以上是 Awaitiliy 的官方解释。简而言之 Awaitility 提供了方便的测试异步系统的方法。

（参考 [Awaitility.org](http://www.awaitility.org/)）

## `@SchedulerLock`

为了避免使用 `@Scheduled` 的方法在并行环境下执行导致数据不一致，Spring提供了ShedLock来确保同一时间只执行一个任务。通过给任务加锁，其他实例在检测到任务已
上锁便会跳过，在下一次task schduling时再尝试获取锁。

ShedLock有四种属性：`name`，`lock_until`，`locked_at`，和`locked_by`。分别为一个独有名称，锁持续时间，节点获取锁的时间戳，获取锁的节点标识。

用法示例

```
@Scheduled(fixDelay = 1000)
@SchedulerLock(name = "lockName", lockAtMostFor = "50s", lockAtLeastFor = "30s")
```

# Configuration

## `@ConfigurationProperties`

用于使用配置文件配置Bean。

## `@ConstructorBinding`

绑定构造器和配置文件，使得修饰的类成为不可更改的（immutable）。

## `@Qualifier`

在使用 `@Autowire` 时，有时因为有多个同类型的bean存在，Spring无法判断应该注入哪个bean，这时候就可以使用 `@Qualifier` 来指定使用哪一个bean。

# Entity

## `@Entity`

## `@EntityListeners`

指定对实体的回调监听器（callback listener），常用 `AuditingEntityListener`。设定好监听器后可以在该类中声明监听的属性（如修改日期，修改者），并标明对应的注解，

（参考：[JPA Entity Lifecycle Events [Baeldung](https://www.baeldung.com/jpa-entity-lifecycle-events)，[Auditing [Spring docs]](https://docs.spring.io/spring-data/jpa/docs/1.7.0.DATAJPA-580-SNAPSHOT/reference/html/auditing.html)，[Auditing with JPA, Hibernate, and Spring Data JPA [Baeldung](https://www.baeldung.com/database-auditing-jpa)）

## `@Id`

把数据模型类中的一个属性标记为主键。

# Controller

## `@Controller`

## '@RestController`

RestController的出现是为了简化RESTFul web服务的创建。它相当于组合了 `@Controller` 和 `@ResponseBody`，从而省略了给Controller的每个处理请求的方法单独标注 `@ResponseBody` 的麻烦。

# Request Handling

## `@RequestHeader`

获取HTTP请求的头部，可以指定某个特定的属性，如使用 `@RequestHeader("acccept-language")`，也可以不标注特定属性，用一个Map或者HttpHeaders对象来获取全部信息

# Events

可以通过扩展 `ApplicationEvent` 接口来自定义Event，然后通过 `ApplicationEventPublisher` 的 `publish()` 方法来发布一个活动。活动的监听器可以是一个实现了 `ApplicationListener` 接口的bean，或者通过 `@EventListener` 注册到任意一个bean的公开方法上。

## `@EventListener`

方法签名声明了消费的活动类型，指定的监听器默认是同步的，但也可以通过添加 `@Async` 来设置为异步的。

# Stream

# '@EnableBinding`

首先需要启用绑定才能使用如 `@Input` 之类的注解。

# `@Input` and `@Output`

`@Input` 标明一个MessageChannel为该模块的输入频道（input channel），而 `@Output` 标明输出频道。
更多高级的输入输出设定可以参考 (Advanced binding properties)[https://docs.spring.io/spring-cloud-stream/docs/1.0.0.M3/reference/html/spring-cloud-stream-overview.html#_advanced_binding_properties]

# 参考资料

[The @Scheduled Annotation in Spring [Baeldung]](https://www.baeldung.com/spring-scheduled-tasks)

[Lock @Scheduled Tasks With ShedLock And Spring Boot](https://rieckpil.de/lock-scheduled-tasks-with-shedlock-and-spring-boot/)

[The Spring @Qualifier Annotation [Baeldung]](https://www.baeldung.com/spring-qualifier-annotation)

[The Spring @Controller and @RestController Annotations [Baeldung]](https://www.baeldung.com/spring-controller-vs-restcontroller)

[How to Read HTTP Headers in Spring REST Controllers [Baeldung]](https://www.baeldung.com/spring-rest-http-headers)

[Spring Events [Baeldung]](https://www.baeldung.com/spring-events)

[Spring Cloud Stream Overview [Spring docs]](https://docs.spring.io/spring-cloud-stream/docs/1.0.0.M3/reference/html/spring-cloud-stream-overview.html)
