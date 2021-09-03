暂存一些新遇到的注解，之后再总结。

# `@Scheduled`

用于配制和安排任务。用来注解的方法应该是无返回值（如果有则被忽略）且无参数。
可以实现的配置包括: 1. 在一定延迟后执行，用于强制任务在上个执行完成后才能再次执行的情况，2. 以一定的频率执行，需注意任务不会并发执行，只有在上次任务完成后
才会进行下一次（也可添加 `@Async` 来支持并发），3. 在初始延迟后执行，适用于初始任务需要完成的情况，4. 在特定的时间和频率执行，此种方式使用了cron表达式
（[Cron Expressions [Oracle](https://docs.oracle.com/cd/E12058_01/doc/doc.1014/e12030/cron_expressions.htm)）。

其他的使用方法还包括使用properties文件设置参数而非直接在代码嵌入固定值，不再赘述。

如下为各种用法示例：

```
@Scheduled(fixDelay = 1000)
@Scheduled(fixRate = 1000)
@Scheduled(fixDelay = 1000, initialDelay = 1000)
@Scheduled(cron = "* * * * * *")
```
# `@SchedulerLock`

为了避免使用 `@Scheduled` 的方法在并行环境下执行导致数据不一致，Spring提供了ShedLock来确保同一时间只执行一个任务。通过给任务加锁，其他实例在检测到任务已
上锁便会跳过，在下一次task schduling时再尝试获取锁。

ShedLock有四种属性：`name`，`lock_until`，`locked_at`，和`locked_by`。分别为一个独有名称，锁持续时间，节点获取锁的时间戳，获取锁的节点标识。

用法示例

```
@Scheduled(fixDelay = 1000)
@SchedulerLock(name = "lockName", lockAtMostFor = "50s", lockAtLeastFor = "30s")
```



# 参考资料

[The @Scheduled Annotation in Spring [Baeldung]](https://www.baeldung.com/spring-scheduled-tasks)

[Lock @Scheduled Tasks With ShedLock And Spring Boot](https://rieckpil.de/lock-scheduled-tasks-with-shedlock-and-spring-boot/)
