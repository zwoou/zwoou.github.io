# spring缓存

[TOC]

## cache

**保存一些临时性的数据。常用的方法有两种JSR107规范和Spring自己定义的规范**

java的cacheing定义了5个接口，分别是CacheProvider, CacheManager, Cache, Entry 和 Expiry。

​		1、 CacheingProvider可以管理(创建、配置、获取、管理和控制)多个CacheManager，并且一个程序在运行期间可以访问多个CacheProvider。
​        2、 CacheManager可以管理(创建、配置、获取、管理和控制)多个唯一的key的Cache，这些Cache存在于CacheManager的上下文中。一个CacheManager仅被一个CachingProvider所拥有。CacheManager可以管理不同的缓存(Redis的缓存、Ehcache的缓存等，下面Spring缓存中有写)。
​        3、 Cache是一个类似Map的数据结构并将key作为临时索引加以保存，一个Cache仅被一个CacheManager所拥有。每个Cache分别管理不同业务的缓存，如下图的3，左边管理员工的缓存、中间管理部门的缓存、右边管理商品的等。Cache和CacheManager相当于数据库连接池(池中获取连接)。
​        4、 Entry是Cache的k-v键值对。
​        5、 Expiry定义Cache的有效期，超时就过期，过期就不能访问、更新和删除。缓存有效期可以通过ExpiryPolicy设置。
**Spring自己的缓存抽象：** 简化了缓存开发技术

## Cache Abstraction

从spring3.1开始支持cache,从spring4.1开始支持注解和支持JSR-107注解和自定义选项





## maven引入

redis实现

```
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-cache</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
```

## 启动文件打开使用

添加注解 

```
@EnableCaching
```

## 使用

### @CacheConfig

类级别注解，可以配置缓存名称

### The `@Cacheable` Annotation

结果存储在缓存方法中，@Cacheable设置缓存名称，则@CacheConfig设置的缓存名称就会失效，@Cacheable设置的缓存名称为主

方法执行前先看缓存中是否有数据，如果有直接返回。如果没有就调用方法，并将方法返回值放入缓存







#### The `@CachePut` Annotation

无论怎样都会执行方法，并将方法返回值放入缓存

### @CacheEvict

将数据从缓存中删除

### @Caching

可通过此注解组合多个注解策略在一个方法上面









