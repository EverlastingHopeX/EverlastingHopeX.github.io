虽然Java也有作用域函数（Scope functions），但是在使用Java的过程中我很少用到，所以对它的了解很少。正好现在学Kotlin就学习一下。

官方文档对作用域函数的解释如下：
>Kotlin 标准库包含几个函数，它们的唯一目的是在对象的上下文中执行代码块。当对一个对象调用这样的函数并提供一个 lambda 表达式时，它会形成一个临时作用域。
>在此作用域中，可以访问该对象而无需其名称。这些函数称为作用域函数。共有以下五种：`let`、`run`、`with`、`apply` 以及 `also`。

它们的功能都相差不多，区别在于调用该函数的对象在代码块中的引用方式（ `this` 或 `it` ）和返回值（上下文对象或lambda表达式的结果）。作用域函数没有引入新技术，仅仅让代码更简洁。

它们的区别可参考下表：
[函数选择 [Kotlin docs]](https://www.kotlincn.net/docs/reference/scope-functions.html#%E5%87%BD%E6%95%B0%E9%80%89%E6%8B%A9)

遇到过一次小坑，使用MockK的 `every{}` 时，在其内部也有 `this`。如果在外部使用 `apply{}` 就会导致`this` 被覆盖。

# 参考资源

[簡介 Kotlin: run, let, with, also 和 apply [Ray Yuan Liou]](https://louis383.medium.com/%E7%B0%A1%E4%BB%8B-kotlin-run-let-with-also-%E5%92%8C-apply-f83860207a0c)

[作用域函数 [Kotlin docs]](https://www.kotlincn.net/docs/reference/scope-functions.html)
