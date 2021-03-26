# Spring 事务

[TOC]

## 1. 事务管理器

Spring并不直接管理事务，而是提供各种事务管理器
- 管理器接口 **org.springframework.transaction.PlatformTransactionManager**
## 2. 事务属性的定义

 - 传播行为
 - 隔离级别
 - 是否只读
 - 是否超时
 - 回滚
### 2.1 传播行为（propagation behavior）
spring定义了7种传播行为
1. **PROPAGATION_REQUIRED** 如果存在一个事务，则支持当前事务。如果没有事务则开启一个新的事务。

2. **PROPAGATION_SUPPORTS** 如果存在一个事务，支持当前事务。如果没有事务，则非事务的执行。但是对于事务同步的事务管理器，PROPAGATION_SUPPORTS与不使用事务有少许不同。
3. **ROPAGATION_MANDATORY** 如果已经存在一个事务，支持当前事务。如果没有一个活动的事务，则抛出异常。
4. **PROPAGATION_REQUIRES_NEW** 总是开启一个新的事务。如果一个事务已经存在，则将这个存在的事务挂起。
5. **PROPAGATION_NOT_SUPPORTED** 总是非事务地执行，并挂起任何存在的事务。使用PROPAGATION_NOT_SUPPORTED,也需要使用JtaTransactionManager作为事务管理器。（代码示例同上，可同理推出）
6. **PROPAGATION_NEVER** 总是非事务地执行，如果存在一个活动事务，则抛出异常。
7. **PROPAGATION_NESTED**如果一个活动的事务存在，则运行在一个嵌套的事务中. 如果没有活动事务, 则按TransactionDefinition.PROPAGATION_REQUIRED 属性执行。这是一个嵌套事务,使用JDBC 3.0驱动时,仅仅支持DataSourceTransactionManager作为事务管理器。需要JDBC 驱动的java.sql.Savepoint类。有一些JTA的事务管理器实现可能也提供了同样的功能。使用PROPAGATION_NESTED，还需要把PlatformTransactionManager的nestedTransactionAllowed属性设为true;而 nestedTransactionAllowed属性值默认为false。
### 2.2 隔离级别（isolation level）

隔离级别定义了一个事务可能受其他并发事务的影响程度

1. 并发事务引起的问题
   - 脏读(Dirty reads) : 一个事务读取了另一个事务改写但尚未提交的事务.如果改写被回滚了,那么第一个事务获取的数据是无效的.
   - 不可重复读(Norepeatable read): 一个事务执行相同的查询两次或两次以上,但是每次都得到不同的数据时. 这通常是因为另一个并发事务在两次查询期间进行了更新。
   - 幻读(Phantom read)：它发生在一个事务读取了几行数据，接着另一个并发事务插入了几行数据，在随后的查询中就会发现原本不存在的记录。

2. 隔离级别分为以下几种
   - ISOLATION_DEFAULT:使用后端数据库默认的隔离级别
   - ISOLATION_READ_UNCOMMITTED:最低的隔离级别，允许读取尚未提交的数据变更，可能会导致脏读、幻读或不可重复读
   - ISOLATION_READ_COMMITTED:允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读或不可重复读仍有可能发生
   - ISOLATION_REPEATABLE_READ:对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读和不可重复读，但幻读仍有可能发生
   - ISOLATION_SERIALIZABLE:最高的隔离级别，完全服从ACID的隔离级别，确保阻止脏读、不可重复读以及幻读，也是最慢的事务隔离级别，因为它通常是通过完全锁定事务相关的数据库表来实现的












## 参考
 - [https://blog.csdn.net/baidu_28283827/article/details/53198987](https://blog.csdn.net/baidu_28283827/article/details/53198987)
 - [https://docs.spring.io/spring-framework/docs/5.2.13.RELEASE/spring-framework-reference/data-access.html#spring-data-tier](https://docs.spring.io/spring-framework/docs/5.2.13.RELEASE/spring-framework-reference/data-access.html#spring-data-tier)