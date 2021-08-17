---
layout: single
title:  "Spring IoC"
date:   2021-08-17 15:46:00 +0800
categories: Spring IOC dependency-injection
---

Spring框架为Java开发提供了极大的便利，提供了IOC，MVC，AOP等机制。Spring framework 5.0 后提供了对Kotlin的支持，所以即使转向Kotlin，也需要继续加深学习Spring。希望在实际工作中，能对Spring有更深层次的了解与认识。

[Spring framework(Core) for complete beginners](https://youtu.be/r2Q0Jzl2qMQ)
>Youtube上Selenium Express制作的Spring framework的视频教程，包含了IOC，注解，Beans等知识点的讲解，清晰易懂。



<h2>IOC （inversion of control，控制反转）</h2>

1. Give the control of type of implementation to use to external code, instead of specifying it locally.
2. How: dependency injection, use the implementation provided (injected) by external code.
3. What spring do: with annotation, it will search implementation automatically and inject. (@autowired) 

<h2>Spring如何实现依赖注入</h2>

1. setter injection
2. config default value for fields: <property>, specify name and value (must have a setter method)
3. constructor injection
4. config constructor arguments for fields: <constructor-arg>
5. inject objects: nested beans or ref

<h2>Beans</h2>

1. objects: beans
2. IoC container read config file, create beans and manage beans
3. IoC container: 1. BeanFactory, 2. ApplicationContext (more advanced with more features)
4. ClassPathXmlApplicationContext: implementation of applicationcontext
