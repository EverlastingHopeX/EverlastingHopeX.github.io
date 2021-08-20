---
layout: single
toc: true
toc_label: "Table of contents"
title:  "Spring IoC"
date:   2021-08-20 18:52:00 +0800
categories: Spring IOC dependency-injection
---

Spring框架为Java开发提供了极大的便利，提供了IoC，MVC，AOP等机制。Spring framework 5.0 后提供了对Kotlin的支持，所以即使转向Kotlin，也需要继续加深学习Spring。希望我在实际工作中，能对Spring有更深层次的了解与认识。

[Spring framework(Core) for complete beginners [Youtube]](https://youtu.be/r2Q0Jzl2qMQ)
>Youtube上Selenium Express制作的Spring framework的视频教程，包含了IoC，注解，Beans等知识点的讲解，清晰易懂。

# IoC （Inversion of Control，控制反转）

## 什么是IoC
>In traditional programming, the custom code that expresses the purpose of the program calls into reusable libraries to take care of generic tasks, but with inversion of control, it is the framework that calls into the custom, or task-specific, code.

以上是维基百科对于IoC的解释。传统编程中，我们的代码调用库来处理一般的任务，而IoC则让框架调用我们自定义的，或者针对特定任务的代码。

简单的来说，IoC，指将选择何种代码实现的控制权交给外部代码，而不是在当前代码中指出。在Spring中，控制权由IoC容器（IoC Container）拥有，所有组件的创建和配置由IoC容器控制。

## IoC带来的好处
1. 将任务的实现与运行解耦
2. 使得切换不同的实现更加简单
3. 提供更好的模块性
4. 使得隔离一个组件或者模拟它的依赖以进行测试更加的容易，允许组件通过合约（contract）进行通信。

## 实现IoC
IoC可以通过不同的机制实现，包括策略设计模式（Strategy Design Pattern)，服务定位模式（Service Locator Pattern），工厂模式（Factory Pattern）以及**依赖注入**（Dependency Injection）。IoC是一种设计原则（Design Principle），而如上机制则是设计模式（Design Pattern）。

Spring通过使用IoC容器对对象注入依赖来实现IoC。这样对象就会使用被注入的实现方式，而不必在本地进行创建和实现。

## IoC容器
![IoC container from Spring framework(Core) for complete beginners](../assets/IoC_Container.png)

**Mechanism of IoC container from [Spring framework tutorial for beginners with examples in eclipse | Why spring inversion of control ? [Youtube]](https://www.youtube.com/watch?v=r2Q0Jzl2qMQ&list=PL3NrzZBjk6m-nYX072dSaGfyCJ59Q5TEi&index=1)**

上图展示了IoC容器如何创建和管理Beans来实现控制反转。IoC容器会读取配置文件并创建对应的Bean，当某处代码需要使用Bean时，只需调用 `getBean("BeanX")` 就可以获取对应的Bean。

Spring中 `BeanFactory` 是IoC容器的根接口（root interface），`ApplicationContext` 则实现了 `BeanFactory` 并添加了诸如AOP等其他功能。只有当对内存占用要求高时才推荐使用 `BeanFactory`。（参考：[Difference Between BeanFactory and ApplicationContext](https://www.baeldung.com/spring-beanfactory-vs-applicationcontext)）

Spring也提供了其他实现了 `ApplicationContext` 接口的IoC容器，如 `ClassPathXmlApplicationContext`，`FileSystemXmlApplicationContext` 和 `WebApplicationContext`。 前两者适合独立应用，后者则适合web应用。

# Spring如何实现依赖注入

Spring中有三种依赖注入的方法：**构造器**（constructors）, **赋值函数**（setters）和**属性**（fields）。

## 基于构造器的依赖注入

基于构造器的依赖注入指IoC容器通过调用以我们想要注入的**依赖作为参数**的**构造函数**，来实现依赖注入的方式。使用基于构造器的依赖注入有三种方式：使用**注解**（annotation），使用**隐式构造器**（implicit constructor）和使用**XML**文件。具体可参考 [Constructor Dependency Injection in Spring [Baeldung]](https://www.baeldung.com/constructor-injection-in-spring)。

此种依赖注入的方法，在依赖设定之后无法更改。需要注意的是，如果构造函数的参数过多，则说明当前类的责任过多，需要重构。

## 基于赋值函数的依赖注入

基于赋值函数的依赖注入指IoC容器在使用**无参构造函数**或者**无参静态工厂方法**初始化bean之后，调用**类的赋值函数**实现依赖注入。此方法可以通过注解或者XML文件配置。

此种依赖注入的方法应用于可选的，有合理默认值的依赖，否则需在使用此依赖处添加非空检查。此方法的优点是可以重新设置依赖。

## 基于属性的依赖注入

基于属性的依赖注入指IoC容器使用反射（reflection）注入依赖，而不是用构造器或者赋值函数。此方法可以通过注解（ `@Autowired` ）或者XML配置。

### 缺点

此方法虽然看起来简便，但是也有一些缺点（参考：[What exactly is Field Injection and how to avoid it? [StackOverflow]](https://stackoverflow.com/questions/39890849/what-exactly-is-field-injection-and-how-to-avoid-it)，[Field Dependency Injection Considered Harmful
 [Vojtech Ruzicka]](https://www.vojtechruzicka.com/field-dependency-injection-considered-harmful/)）：

1. 破坏单一责任原则。该方法使得添加新的依赖非常简单，从而导致添加过多的依赖。此时类变得过于臃肿，但却难以发现并及时优化重构。
2. 隐藏依赖。前两种依赖注入方法可以看出注入的依赖是可选的或者必选的，而此种方法中依赖没有反映在接口（如构造器或赋值函数）上。
3. 与容器耦合。此种方法下，类与依赖注入容器（DI container）耦合，没有直接实例化配置好所需依赖的类的对象的方法。调用默认构造函数会由于缺少依赖导致NPE（空指针异常），而且该类无法在不使用DI容器的情况下进行单元测试。
4. 可变性（mutability)。当依赖被注入后，仍可以被改变，而不像构造器依赖注入一样赋值后不可改变。这是由于使用了反射。

事实上，Spring的文档中只提及了前两种依赖注入方法，也从另一方面说明了基于属性的依赖注入不是主流的依赖注入方法，也并不被推荐使用。（参考：[6.4.1 Dependency Injection [Spring docs]](https://docs.spring.io/spring-framework/docs/4.2.x/spring-framework-reference/html/beans.html)）

## 三种依赖注入方法的选择

Spring文档推荐对必选的依赖使用构造器注入，对可选的依赖选择赋值函数注入。需要注意的是，对赋值函数添加  `@Required` 则能使对应属性成为必选依赖。


# Beans

Spring中的bean指通过Spring容器初始化的对象。任意的Java POJO（Plain Old Java Object，普通的无特殊限制的Java对象）类如果被配置为通过容器初始化，则创建了一个Spring bean。

## 作用域（Spring Bean Scopes）

Spring Beans有五个作用域：

1. 单例（Singleton）。每个容器只创建一个bean的实例。
2. 原型（Prototype）。每次请求一个bean时都会创建一个新的实例。
3. 请求（Request）。与原型作用域相同，不过仅限于在web应用中使用。每次HTTP请求都会创建新的实例。
4. 会话（Session）。容器为每个HTTP会话创建一个新的bean实例。
5. 全局会话（Global-session）。用于为Portlet应用创建全局会话bean。

我们也可以扩展定义自己的作用域。

## Bean的配置

Spring提供了三种配置bean的方法：

1. 基于注解的配置。使用 `@Service` 或 `@Component`，对于作用域的指定可以使用 `@Scope`。
2. 基于XML的配置。使用XML对bean进行配置，使用了包括 `<beans>` 等一系列标签和包含的属性。
3. 基于Java的配置。从Spring 3.0起，开发者可以指定Java程序对bean进行配置，使用的注解包括 `@Configuration`，`@ComponentScan` 和 `@Bean`。使用可参考：[How to understand Spring @ComponentScan [StackOverflow]](https://stackoverflow.com/questions/28963639/how-to-understand-spring-componentscan)。

基于注解的配置和基于Java的配置看上去非常相似，都是用了注解。他们的区别是基于Java的配置**完全免除了配置文件**，而基于注解的配置**仍有配置文件**，仅使用注解来实现bean的装配（bean wiring）。（参考：[Java-based vs annotation-based configuration/autowiring [StackOverflow]](https://stackoverflow.com/questions/41615041/java-based-vs-annotation-based-configuration-autowiring)）。

# 参考资源
[Intro to Inversion of Control and Dependency Injection with Spring [Baeldung]](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring)
>一篇介绍Spring控制反转和依赖注入的文章。

[IoC原理 [廖雪峰]](https://www.liaoxuefeng.com/wiki/1252599548343744/1282381977747489)
>一篇介绍IOC原理的文章。

[Inversion of control [Wikipedia]](https://en.wikipedia.org/wiki/Inversion_of_control)
>IOC的维基百科。

[Spring IoC, Spring Bean Example Tutorial [JournalDev]](https://www.journaldev.com/2461/spring-ioc-bean-example-tutorial)
>一篇介绍Spring IoC以及Bean的文章。

[The IoC container [Spring docs]](https://docs.spring.io/spring-framework/docs/3.2.x/spring-framework-reference/html/beans.html)
>Spring的IoC容器的官方文档。

[Chapter 4. Creating and using bean definitions [Spring docs]](https://docs.spring.io/spring-javaconfig/docs/1.0.0.m3/reference/html/creating-bean-definitions.html)
>Spring的有关创建和使用bean的定义的文档。
