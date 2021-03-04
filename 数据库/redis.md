# redis 实战

[TOC]



## 1. 安装

windos： [https://github.com/MicrosoftArchive/redis/releases](https://github.com/MicrosoftArchive/redis/releases)
linux:

```shell
    sudo apt-get install redis-server
```

### 1.1 检查Redis服务器系统进程

```shell
ps -aux|grep redis
```

### 1.2 通过启动命令检查Redis服务器状态

```shell
netstat -nlt|grep 6379
```

### 1.3 访问客户端

```shell
redis-cli
```

## 2. 概述

Redis 是速度非常快的非关系型（NoSQL）内存键值数据库，可以存储键和五种不同类型的值之间的映射。
官方介绍：

> Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker. It supports data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs and geospatial indexes with radius queries. Redis has built-in replication, Lua scripting, LRU eviction, transactions and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster.

## 3. 运行

redis-server.exe redis.windows.conf

## 4. 配置

- config get * 查看
- config set CONFIG_SETTING_NAME NEW_CONFIG_VALUE

1. 数据库数量 默认数据库为0，可以select <dbid> 命令
2. 设置密码
    requirepass "xxx"
    客户端连接时需要 auth <password>
3. 开启远程访问
   修改redis.conf
   在redis3.2之后，redis增加了protected-mode
   修改原protected-mode yes为protected-mode no

## 5. 数据类型

1. string 一个键最大存储512MB, 值是二进制安全的。
2. hash 是一个键值对集合
3. list
4. set
5. zset 有序集合

- [https://redislabs.com/ebook/part-1-getting-started/chapter-1-getting-to-know-redis/1-2-what-redis-data-structures-look-like/](https://redislabs.com/ebook/part-1-getting-started/chapter-1-getting-to-know-redis/1-2-what-redis-data-structures-look-like/)

## 6. 命令

1. 设置过期时间
Redis有四个不同的命令用于设置键的生存时间或过期时间
EXP|RE <key> <ttl> 
PEXPIRE <key> <ttl>

- [Redis 命令参考中文文档](http://doc.redisfans.com/index.html)

## 7. 备份与恢复

1. 备份 save 在redis安装目录创建dump.rdb文件
2. 恢复 将备份文件移动到安装目录，重启服务即可

## 8. 数据结构

### 8.1 字典

dictht 是一个散列表结构，使用拉链法保存哈希冲突的dictEntry
Redis 的字典 dict 中包含两个哈希表 dictht，这是为了方便进行 rehash 操作。在扩容时，将其中一个 dictht 上的键值对 rehash 到另一个 dictht 上面，完成之后释放空间并交换两个 dictht 的角色。

### 8.2 跳跃表

是有序集合的底层实现之一。

跳跃表是基于多指针有序链表实现的，可以看成多个有序链表。

## 9. 使用场景

1. 计数器
可以对 String 进行自增自减运算，从而实现计数器功能。

Redis 这种内存型数据库的读写性能非常高，很适合存储频繁读写的计数量。
2. 缓存
将热点数据放到内存中，设置内存的最大使用量以及淘汰策略来保证缓存的命中率。
3. 消息队列
List 是一个双向链表，可以通过 lpop 和 lpush 写入和读取消息。


不过最好使用 Kafka、RabbitMQ 等消息中间件
4. 会话缓存
在分布式场景下具有多个应用服务器，可以使用 Redis 来统一存储这些应用服务器的会话信息。

当应用服务器不再存储用户的会话信息，也就不再具有状态，一个用户可以请求任意一个应用服务器。
5. 分布式锁实现
在分布式场景下，无法使用单机环境下的锁来对多个节点上的进程进行同步。

可以使用 Reids 自带的 SETNX 命令实现分布式锁，除此之外，还可以使用官方提供的 RedLock 分布式锁实现。

## 10 . Redisson 分布式锁实现

github地址:[https://github.com/redisson/redisson](https://github.com/redisson/redisson)

设计者考虑了按照功能特性不同,设计了多个功能组件.

- 可重入锁(Reentrant Lock)

- 公平锁(Fair Lock)

- 联锁(MultiLock)

- 红锁(RedLock)

- 读写锁(ReadWriteLock)

- 信号量(Semaphore)

- 闭锁(CountDownLatch)

  

  ### 10.1 可重入锁

  可重入锁功能组件有一次性与可重入两种实现方式.

#### 10.1.1 一次性锁

```
RLock lock=redissonClient.getLock(lockName);
// 10秒后会自动释放
lock.lock(10,TimeUnit.SECONDS);
// 释放锁
lock.unlock();
// 在某些严格的业务场景下,也可以强制释放
lock.forceUnlock();
```

####   10.1.2 可重入锁

```
RLock lock=redissonClient.getLock(lockName);
// 尝试加锁,最多等待100秒,上锁以后10秒自动释放
boolean res = lock.tryLock(100,10,TimeUnit.SECONDS);
```



## 参考

- [菜鸟教程](http://www.runoob.com/redis/redis-tutorial.html)
- [https://github.com/CyC2018/Interview-Notebook/blob/master/notes/Redis.md](https://github.com/CyC2018/Interview-Notebook/blob/master/notes/Redis.md)

- [springboot-redis-cache](https://blog.csdn.net/fanpeizhong/article/details/79998164)
- [https://docs.spring.io/spring-data/redis/docs/2.0.9.RELEASE/reference/html/#get-started](https://docs.spring.io/spring-data/redis/docs/2.0.9.RELEASE/reference/html/#get-started)





