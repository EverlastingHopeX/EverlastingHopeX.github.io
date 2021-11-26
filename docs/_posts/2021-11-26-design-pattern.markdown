# Design pattern

## Builder pattern

Builder pattern can be use when you want to build a data class object that has many properties, and they are are immutable, and 
you want to add another layer to extract how you set values for these properties.

Actually, it is not a good idea to use builder pattern in Kotlin, it is an anti-pattern, it increase complexity with few advantages.

[Don't Use The Builder Pattern in Kotlin](https://code-held.com/2021/01/23/dont-use-builder-in-kotlin/)
