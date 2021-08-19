---
layout: single
title:  "Spring Boot"
date:   2021-08-19 17:16:00 +0800
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

<h2>Spring Boot Starters</h2>

[List of Spring Boot Starters](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using.build-systems.starters)
>Spring Boot官网文档列举了所有官方的starter

使用者可以依据项目的需要，在POM文件中添加对应的starter的依赖，而不需要繁琐的手动的管理配置依赖。如下是官方文档中的starter列表。至于具体的使用，如Redis, AOP等，我在使用到时再另写一篇。

Table 1. Spring Boot application starters
|Name|	Description|
|---|---|
|spring-boot-starter|Core starter, including auto-configuration support, logging and YAML|
|spring-boot-starter-activemq|Starter for JMS messaging using Apache ActiveMQ|
|spring-boot-starter-amqp|Starter for using Spring AMQP and Rabbit MQ|
|spring-boot-starter-aop|Starter for aspect-oriented programming with Spring AOP and AspectJ|
|spring-boot-starter-artemis|Starter for JMS messaging using Apache Artemis|
|spring-boot-starter-batch|Starter for using Spring Batch|
|spring-boot-starter-cache|Starter for using Spring Framework’s caching support|
|spring-boot-starter-data-cassandra|Starter for using Cassandra distributed database and Spring Data Cassandra|
|spring-boot-starter-data-cassandra-reactive|Starter for using Cassandra distributed database and Spring Data Cassandra Reactive|
|spring-boot-starter-data-couchbase|Starter for using Couchbase document-oriented database and Spring Data Couchbase|
|spring-boot-starter-data-couchbase-reactive|Starter for using Couchbase document-oriented database and Spring Data Couchbase Reactive|
|spring-boot-starter-data-elasticsearch|Starter for using Elasticsearch search and analytics engine and Spring Data Elasticsearch|
|spring-boot-starter-data-jdbc|Starter for using Spring Data JDBC|
|spring-boot-starter-data-jpa|Starter for using Spring Data JPA with Hibernate|
|spring-boot-starter-data-ldap|Starter for using Spring Data LDAP|
|spring-boot-starter-data-mongodb|Starter for using MongoDB document-oriented database and Spring Data MongoDB|
|spring-boot-starter-data-mongodb-reactive|Starter for using MongoDB document-oriented database and Spring Data MongoDB Reactive|
|spring-boot-starter-data-neo4j|Starter for using Neo4j graph database and Spring Data Neo4j|
|spring-boot-starter-data-r2dbc|Starter for using Spring Data R2DBC|
|spring-boot-starter-data-redis|Starter for using Redis key-value data store with Spring Data Redis and the Lettuce client|
|spring-boot-starter-data-redis-reactive|Starter for using Redis key-value data store with Spring Data Redis reactive and the Lettuce client|
|spring-boot-starter-data-rest|Starter for exposing Spring Data repositories over REST using Spring Data REST|
|spring-boot-starter-freemarker|Starter for building MVC web applications using FreeMarker views|
|spring-boot-starter-groovy-templates|Starter for building MVC web applications using Groovy Templates views|
|spring-boot-starter-hateoas|Starter for building hypermedia-based RESTful web application with Spring MVC and Spring HATEOAS|
|spring-boot-starter-integration|Starter for using Spring Integration|
|spring-boot-starter-jdbc|Starter for using JDBC with the HikariCP connection pool|
|spring-boot-starter-jersey|Starter for building RESTful web applications using JAX-RS and Jersey. An alternative to spring-boot-starter-web|
|spring-boot-starter-jooq|Starter for using jOOQ to access SQL databases. An alternative to spring-boot-starter-data-jpa or spring-boot-starter-jdbc|
|spring-boot-starter-json|Starter for reading and writing json|
|spring-boot-starter-jta-atomikos|Starter for JTA transactions using Atomikos|
|spring-boot-starter-mail|Starter for using Java Mail and Spring Framework’s email sending support|
|spring-boot-starter-mustache|Starter for building web applications using Mustache views|
|spring-boot-starter-oauth2-client|Starter for using Spring Security’s OAuth2/OpenID Connect client features|
|spring-boot-starter-oauth2-resource-server|Starter for using Spring Security’s OAuth2 resource server features|
|spring-boot-starter-quartz|Starter for using the Quartz scheduler|
|spring-boot-starter-rsocket|Starter for building RSocket clients and servers|
|spring-boot-starter-security|Starter for using Spring Security|
|spring-boot-starter-test|Starter for testing Spring Boot applications with libraries including JUnit Jupiter, Hamcrest and Mockito|
|spring-boot-starter-thymeleaf|Starter for building MVC web applications using Thymeleaf views|
|spring-boot-starter-validation|Starter for using Java Bean Validation with Hibernate Validator|
|spring-boot-starter-web|Starter for building web, including RESTful, applications using Spring MVC. Uses Tomcat as the default embedded container|
|spring-boot-starter-web-services|Starter for using Spring Web Services|
|spring-boot-starter-webflux|Starter for building WebFlux applications using Spring Framework’s Reactive Web support|
|spring-boot-starter-websocket|Starter for building WebSocket applications using Spring Framework’s WebSocket support|

Table 2. Spring Boot production starters

|Name|	Description|
|---|---|
|spring-boot-starter-actuator|Starter for using Spring Boot’s Actuator which provides production ready features to help you monitor and manage your application|

Table 3. Spring Boot technical starters
|Name|	Description|
|---|---|
|spring-boot-starter-jetty|Starter for using Jetty as the embedded servlet container. An alternative to spring-boot-starter-tomcat|
|spring-boot-starter-log4j2|Starter for using Log4j2 for logging. An alternative to spring-boot-starter-logging|
|spring-boot-starter-logging|Starter for logging using Logback. Default logging starter|
|spring-boot-starter-reactor-netty|Starter for using Reactor Netty as the embedded reactive HTTP server.|
|spring-boot-starter-tomcat|Starter for using Tomcat as the embedded servlet container. Default servlet container starter used by spring-boot-starter-web|
|spring-boot-starter-undertow|Starter for using Undertow as the embedded servlet container. An alternative to spring-boot-starter-tomcat

