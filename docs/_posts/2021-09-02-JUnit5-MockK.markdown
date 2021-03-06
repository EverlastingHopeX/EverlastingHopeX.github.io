---
layout: single
toc: true
toc_label: "Table of contents"
title:  "JUnit5 and MockK"
date:   2021-09-02 19:18:00 +0800
categories: JUnit5 Kotlin MockK
---

熟悉项目先从测试开始。开始学习整理一下JUnit5和其他测试框架的心得和资料。之前使用过JUnit5和Mockito对Java项目进行测试，在现在这个项目中因为采用的是Kotlin，
所以框架用的是MockK。MockK和Mockito有诸多相似之处，而网上Mockito相关的信息更加丰富，因此在查找资料时，查看Mockito的相似语法也提供了很大帮助。

# 注解（Annotations）

先从遇到的开始。

1. `@ExtendWith(MockKExtension::class)`

`@ExtendWith` 用于声明式的注册扩展，详细见本文 扩展 - 扩展的注册 - 声明式。MockK是面向Kotlin的模拟库（Mocking library），MockKExtension用于初始化模拟。

2. `@RelaxedMockK`

在此处标注了成员变量，用于指定该变量为relaxed mock对象。

3. `@InjectMockKs`

在此处修饰一个`lateinit`的成员变量，用于指明此变量对应的Mock对象在创建时应该被注入其他对象（当前测试类中使用 `@RelaxedMockK` 修饰的对象）。`@InjectMockKs` 默认只注入`lateinit`变量或者还未赋值的变量，但可以通过设定改变。

需要注意的是，如果部分属性无法使用Mock，则不要使用 `@InjectMockKs`，而是在 `init` 部分手动使用构造函数初始化该变量。

4. `@Test`

修饰方法，指明该方法是一个测试方法。

5. `@TestInstance`

用于指定测试用例的生命周期。默认为每次测试都创建新的当前类的测试实例。


## 元注解与组合注解（Meta Annotations and Composed Annotations）

元注解是可以应用于其他注解的注解，组合注解则是由一或多个元注解标注的自定义注解。Junit Jupiter注解可以用作元注解
（参考： [What Are Meta-Annotations in Java? [DZone]](https://dzone.com/articles/what-are-meta-annotations-in-java)，
[Spring 组合注解(Composed Annotation) [GitHub]](https://nanlei.github.io/my-notes/SpringFramework/spring-annotation-composed-annotation/)

## 扩展（Extension）

扩展的目的是用于扩展测试类或者测试方法的行为。扩展与测试运行的特定活动相关联，该活动被称为扩展点（Extension point）。当抵达特定的生命周期阶段时，JUnit引擎会调用被注册的扩展。

有五种主要的扩展点。

（参考：[A Guide to JUnit 5 Extensions [Baeldung]](https://www.baeldung.com/junit-5-extensions)

### 扩展的注册（Register）

扩展的注册分为三类：1. 声明式（Declarative），2. 编程式（Programmatical），3. 自动式（Automatic）。声明式使用 `@ExtendWith`，编程式使用 `@RegisterExtension`，
自动式使用Java的ServiceLoader机制。

#### 声明式

通过给一个测试接口（或测试类，测试方法）添加注解，或者采用 `@ExtendWith` 自定义组合注解并提类的引用，可以声明式地注册扩展。

#### 编程式

#### 自动式

# 变量类型

1. `lateinit`

在此处修饰成员变量。在单元测试中，非空类型的成员变量无法在构造函数中初始化，但仍需要在类内部引用时避免null检查。这时就可以使用 `lateinit` 修饰符，当在此变量未初始化时访问会
抛出一个特殊的异常，指明被访问的变量并指出它还未被初始化。

（参考：[Late-initialized properties and variables [Kotlin]](https://kotlinlang.org/docs/properties.html#late-initialized-properties-and-variables)）


# MockK

MockK是面向Kotlin的模拟库（Mocking library。

（参考：[MockK: A Mocking Library for Kotlin [Baeldung]](https://www.baeldung.com/kotlin/mockk)）

## 函数

1. `verify`

与Mockito相似，用于验证方法的调用。详细见本文 - MockK - 验证

2. `every`

用于打桩（stubbing）代码块，指定调用某方法的返回值。

3. `clearMocks()`

清除特定的Mock。需要注意clear和unmock的区别，clear会清除对象内部的状态，使对象变为一个空的对象（Empty object），而unmock则会将对象变为mock之前的状态。具体说，clear会清除通过 `every` 设定的行为和调用计数。

4. `answers()`


5. `wasNot`

`wasNot` is used for verify if the whole mock is called, should not be used to verify a single method.



其他函数及其功能可见：
[常用函数表（Top level functions）[MockK docs]](https://mockk.io/#top-level-functions)


## Relaxed Mock

一个relaxed mock（感觉翻译为放松的模拟有些奇怪，故之后均不翻译）是为所有方法返回一些简单的值的模拟，这可以省略为每个方法指定行为。使用 `RelaxedMockK` 注解的对象
的方法被调用时会返回对应类型的默认值。

## Spy



## 验证（Verify）

MockK使用 `verify()` 验证方法的调用，在 `()`中写入需要验证的条件，在 `{}` 中写入需要验证的方法。需要注意，无法验证mock的属性的属性，即**只能验证被mock的对象的方法**，而不具有传递性。

验证条件可以使用的关键词包括 `atLeast`，`atMost`，和`exactly`，可以用来验证调用的次数。还可以使用 `verifyAll()`来验证是否所有的调用都发生了，使用 `wasNot Called`判断
Mock对象的任何方法是否被调用，使用 `verifySequence()`和`verifyOrder()`验证调用的顺序（后者指明的顺序中可以存在遗漏，例如方法1，2，3发生了，两者都指明顺序为1，3，前者无法
通过而后者可以通过）。

##  匹配器（Matcher）

匹配器相当于通配符或者正则表达式，用来匹配特定的值，用于打桩（stubs）或者验证。

`any()` 的使用非常方便，当遇到overload时，可以使用 `any<T>()` 来指定特定的类。（虽然我认为提供一个诸如 `anyList()` 的匹配器对集合类型进行匹配会直观的多，但是目前Mockk只采用 
`any<List<T>>()` 来匹配列表类型）

匹配器及其功能可见：
[匹配器（Matchers）[MockK docs]](https://mockk.io/#matchers)

Mockito中的匹配器：
[Different Types Of Matchers Provided By Mockito [Software Testing Help]](https://www.softwaretestinghelp.com/mockito-matchers/)

## 应答（Answer）

Mockito对应答的定义为

>Answer specifies an action that is executed and a return value that is returned when you interact with the mock.

应答指定了当与mock互动时执行的行为和返回值。常用的包括 `returns value`，`answers { code }`等。 

应答及其功能可见：
[应答（Answers）[MockK docs]](https://mockk.io/#answers)

如果想在应答中访问方法的参数，可以使用 `firstArg()`，`secondArg()`等，更多可见 [Answers [MockK Guidebook]](https://notwoods.github.io/mockk-guidebook/docs/mocking/answers/)

## 集合

在做测试的过程中，有的类的属性包括了集合的实例。在这种情况下，似乎应该Mock一个集合，事实上这不是什么好主意，因为集合的方法很多，而且互相依赖，很难保证对每个方法都Mock了相应的
方法，更推荐的方法是创建一个真实的集合，而在其中放入Mock的集合的元素。

参考 [Mockito: mocking an arraylist that will be looped in a for loop [StackOverflow]](https://stackoverflow.com/questions/18483176/mockito-mocking-an-arraylist-that-will-be-looped-in-a-for-loop)


# 参考资料
[JUnit5 User Guide [JUnit docs]](https://junit.org/junit5/docs/current/user-guide/)
> JUnit5的官方文档https://www.baeldung.com/junit-5-extensions

[MockK：一款強大的 Kotlin Mocking Library [Joe Blog]](https://medium.com/joe-tsai/mockk-%E4%B8%80%E6%AC%BE%E5%BC%B7%E5%A4%A7%E7%9A%84-kotlin-mocking-library-part-1-4-39a85e42b8)
>

[MockK Home[MockK]](https://mockk.io/)
>

[MockK Guidebook [Tiger Oakes]](https://notwoods.github.io/mockk-guidebook/)
>

[@TestInstance Annotation in JUnit 5 [Baeldung]](https://www.baeldung.com/junit-testinstance-annotation)
