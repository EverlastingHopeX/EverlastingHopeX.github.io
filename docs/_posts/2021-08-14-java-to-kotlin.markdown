---
layout: single
title:  "Java to Kotlin"
date:   2021-08-15 18:26:00 +0800
categories: Java Kotlin
---

上学期间最常用的就是Java，对于Kotlin的了解仅限于使用Android Studio对Java直接转换成Kotlin。现在由于工作原因要学习Kotlin，就记录一下学习过程中遇到的比较好的文档等资源，和自己学习过程中的想法。

基于我初步的了解，Kotlin相对Java加了一些语法糖，使得程序更简洁。在这方面，Kotlin有一点Python的感觉，尤其是变量类型自动判断。

# Kotlin与Java相比的改变

Kotlin相对Java除了语法的细微改变，有值得注意的几点。
1. 变量与常量的定义。`var`表示变量，`val`表示常量（相当于Java中的`final`）。
2. 基本数据类型（primitive types）。Kotlin虽然相对应于Java的八种基本数据类型，但是并不能直接创建一个基本数据类型的对象，而是创建一个普通的封装好的对象，类似于Java中的`Integer`。（参考：[Kotlin is not primitive](https://medium.com/@przemek.materna/kotlin-is-not-primitive-primitives-in-kotlin-and-java-f35713fda5cd))
3. 类型检测。Kotlin使用`is`检测对象的类型，而且在通过`is`判断后会自动的将对象类型转换为对应类型。
4. Null检查机制。Kotlin的空安全设计（Null Safety）提供使用 `?` 和 `!!` 限定类型是否可为空，更好的处理了`NullPointerException`。
5. 类型转换（cast）。Kotlin中使用`as`进行显式的类型转换，`as?`则会在转换不可行时返回`null`来避免异常的产生。
6. 区间（range）。使用了`..`表示一个区间，可以配合`in`对检查是否属于某区间或对某区间进行遍历，还可以用`step`限定遍历的步长。
7. `when`。`when`类似于Java中的`switch`，但是条件判断更简洁，功能也更强大。`when`的分支不需要`break`，而且可以使用条件语句。
8. Lambda表达式。相较于Java，Kotlin需要显式指明lambda表达式中的数据类型。
9. 可选参数（optional arguments）。Kotlin的可选参数避免了不必要的重载（overload），而在Java中没有可选参数。
10. 解构（destructuring）。Kotlin也提供了Java目前也不支持的解构，可用于将一个对象解构成多个变量来方便的获取对象的属性。
11. 类修饰符`open`。`open`是相对于`final`定义的，Kotlin中类默认是`final`的，即不可继承。Java中不加`final`则默认可以继承。
12. 单例（singleton）。使用`object`关键字来定义一个单例，可以用来创建一个匿名内部类的对象。创建的对象可以直接用类名（对象名？）调用或者用一个变量保存后调用。

13. 构造器（constructor）。Kotlin中的类可以有一个主构造函数（primary constructor）和多个次构造函数（secondary constructor）。主构造器属于类的声明的头部（header）的一部分，例如
    
    ```Kotlin
     class Person contructor (firstName: String){}
     ```
     
    当声明类时列出类中包含的属性，将会自动提供对应的默认构造器作为主构造器。如果不需要注解或者可见性修饰符，则可以省略`constructor`。主构造器不包含任何代码，但可以使用`init`创建初始化代码在对象实例化时执行必要的初始化。初始化代码可以看作**主构造器的一部分**，可以存在**多块**，将按照顺序执行。次级构造器的声明在类的内部，必须直接或间接的**调用主构造器**。
14. `new`。Kotlin中没有`new`，创建对象可以直接调用构造函数创建。
15. `Any`。Kotlin中的`Any`类似于Java中的`Object`，实际上，在JVM上运行时，`Any`和`Object`被视作相同。不过，如之前提到的，Java中有基本数据类型，而Kotlin没有，所以Kotlin中的`Any`更符合“万物始源”。（参考：[Does Any == Object](https://stackoverflow.com/questions/38761021/does-any-object)）
16. 数据类（Data class）。只包含数据的类，使用`data`修饰，例如
    
    ```Kotlin
    data class User(val name: String, val age: Int)
    ```

    Kotlin会自动生成对应函数，包括`equals()` / `hashCode()`， `toString()`，`componentN()`（如`compnent1()`返回第一个属性，用于解构对象），`copy()`（深复制）。
17. 密封类（sealed class）。封装类和封装接口使用`sealed`修饰，用于限制类的继承结构来更好的控制继承。所有封装类的子类在编译时已知，封装类的子类只能存在于该封装类所在的编译单元和包下。当需要有限个不同的选项时，可以使用密封类。此情况下，密封类像是**枚举类的扩展**，不同的是枚举类只有一个值，而密封类可以有不同数目的值和方法。（参考：[Sealed class in Kotlin](https://www.baeldung.com/kotlin/sealed-classes))
18. 泛型（generic）。Kotlin泛型的使用类似Java。Kotlin使用`:`对泛型的类型上限进行约束，还使用了`in`和`out`来实现声明处型变（declaration-site variance）。声明处型变指定了类型参数为协变的（covariant）或逆变的（contravariant）（理解协变与逆变：[Covariance and Contravariance In Java](https://dzone.com/articles/covariance-and-contravariance))。我对于Java泛型，乃至协变与逆变的理解较浅，以后可能会深入学习，另写一篇。（参考：[Generics](https://kotlinlang.org/docs/generics.html)）
19. 委托（Delegation）。委托模式是继承的替代品，Java中可以通过聚合，即在类中存有另一个类的示例来实现委托，Kotlin仅仅提供了更简洁的语法。相较于继承，委托可以直接使用已有的接口的实现来实现多个接口，或者来加强一个已有的实现。（参考：[Delegation pattern](https://www.baeldung.com/kotlin/delegation-pattern)，[When to use delegation instead of inheritance?](https://stackoverflow.com/questions/832536/when-to-use-delegation-instead-of-inheritance) ）

# 参考资源

[Kotlin Documentation](https://kotlinlang.org/docs/home.html)

>Kotlin的官方文档，适合细致全面的了解，以及查漏补缺。可以用来验证网上可能存在错误的信息，或者当网上相关资讯较少时作为参考。

[菜鸟教程的Kotlin教程](https://www.runoob.com/kotlin/kotlin-tutorial.html)

>很方便的文档，提供了示例代码，适合用来初步了解Kotlin，不过对有其他语言基础的会显得比较繁琐。

[From Java to Kotlin](https://fabiomsr.github.io/from-java-to-kotlin/)

>适合有Java基础的人。从基础，函数和类三个方面用示例代码进行简单直观的比较。需要注意的是，有些Java和Kotlin示例并不能完全一一对应。

[Clean Code with Kotlin [Philipp Hauer]](https://phauer.com/2017/clean-code-kotlin/)

>讲了Kotlin的邮件，使用了Java作为对比。

[Idiomatic Kotlin. Best Practices. [Philipp Hauer]](https://phauer.com/2017/idiomatic-kotlin-best-practices/)

>
