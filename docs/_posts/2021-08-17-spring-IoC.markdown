---
layout: single
toc: true
toc_label: "Table of contents"
title:  "Spring IoC"
date:   2021-08-17 18:09:00 +0800
categories: Spring IOC dependency-injection
---

Spring框架为Java开发提供了极大的便利，提供了IoC，MVC，AOP等机制。Spring framework 5.0 后提供了对Kotlin的支持，所以即使转向Kotlin，也需要继续加深学习Spring。希望我在实际工作中，能对Spring有更深层次的了解与认识。

[Spring framework(Core) for complete beginners](https://youtu.be/r2Q0Jzl2qMQ)
>Youtube上Selenium Express制作的Spring framework的视频教程，包含了IOC，注解，Beans等知识点的讲解，清晰易懂。

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

[Intro to Inversion of Control and Dependency Injection with Spring](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring)
>一篇介绍Spring控制反转和依赖注入的文章。

[IoC原理](https://www.liaoxuefeng.com/wiki/1252599548343744/1282381977747489)
>一篇介绍IOC原理的文章。

[Inversion of control [Wikipedia]](https://en.wikipedia.org/wiki/Inversion_of_control)
>IOC的维基百科。

 [Spring IoC, Spring Bean Example Tutorial](https://www.journaldev.com/2461/spring-ioc-bean-example-tutorial)
 >一篇介绍Spring IoC以及Bean的文章。

 [The IoC container [Spring docs]](https://docs.spring.io/spring-framework/docs/3.2.x/spring-framework-reference/html/beans.html)
 >Spring的IoC容器的官方文档。

# Spring如何实现依赖注入

1. setter injection
2. config default value for fields: <property>, specify name and value (must have a setter method)
3. constructor injection
4. config constructor arguments for fields: <constructor-arg>
5. inject objects: nested beans or ref

# Beans

1. objects: beans
2. IoC container read config file, create beans and manage beans
3. IoC container: 1. BeanFactory, 2. ApplicationContext (more advanced with more features)
4. ClassPathXmlApplicationContext: implementation of applicationcontext
