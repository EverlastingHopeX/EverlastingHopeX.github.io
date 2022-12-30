---
layout: single
toc: true
toc_label: "Table of contents"
title:  "Spring AoP"
date:   2022-12-30 11:42:00 +0800
categories: Spring AOP
---

# Terminology

## JoinPoint

A Joinpoint is a point during the execution of a program, such as the execution of a method or the handling of an exception.

## Pointcut

A Pointcut is a predicate that helps match an Advice to be applied by an Aspect at a particular JoinPoint.

## Advice

An Advice is an action taken by an aspect at a particular Joinpoint. Different types of advice include “around,” “before,” and “after.”

# Annotations

## @Aspect
Declare a class as Aspect.

## @Order
Declare the order of the target components are taken into account (either from their target class or from their @Bean method).

## @Around

`@Around("@annotation(org.springframework.scheduling.annotation.Scheduled)")`

Declare the cutting point for execution.

The pointcut expression `@annotation(org.springframework.scheduling.annotation.Scheduled)` declares where to bind the advice

# Reference

[Spring AOP [Baeldung]](https://www.baeldung.com/spring-aop)

[Implementing a Custom Spring AOP Annotation [Baeldung]](https://www.baeldung.com/spring-aop-annotation)
