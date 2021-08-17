---
layout: single
title:  "Spring Boot"
date:   2021-08-17 17:02:00 +0800
categories: Spring SpringBoot
---

>“Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run"."

如上是Spring官网对Spring Boot用途的简洁解释。Spring Boot集成了Spring平台的一部分和一些第三方库，力求帮助使用者简单快捷的搭建项目。

[Spring Boot官方主页](https://spring.io/projects/spring-boot)
>Spring Boot的官方主页。此页面包括对Spring Boot的简单介绍，文档链接以及一些帮助使用者上手的指南链接。

<h2>Spring Boot的特性</h2>

1. 创建独立的Spring应用。
2. 直接嵌入了Tomcat，Jetty或者Undertow。
3. 提供了Spring认为适合新手的依赖，以简化开发配置流程。
4. 尽可能地自动配置Spring和第三方库。
5. 提供现成的特性，诸如metrics（各项指标），health check（健康检查），和externalized configuration（外部独立存储配置文件）。
6. 完全不需要生成代码，也不需要使用XML配置。

<h2>比较Tomcat，Jetty和Undertow</h2>

[Comparing Embedded Servlet Containers in Spring Boot](https://www.baeldung.com/spring-boot-servlet-containers)

[Tomcat vs Jetty vs Undertow性能对比](https://www.cnblogs.com/nuccch/p/13672294.html)

这两篇文章都这三个Servlet容器进行了比较，Jetty和Undertow均在多个方面优于Tomcat。在实际场景中，需要根据情况分析使用哪一种Servlet容器。

