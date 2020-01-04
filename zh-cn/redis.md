---
title: redis
tags:
    - redis
    - springboot
    - NOSQL
---

# 《redis 实战》

## 安装

windos： [https://github.com/MicrosoftArchive/redis/releases](https://github.com/MicrosoftArchive/redis/releases)
linux:

```shell
    sudo apt-get install redis-server
```

### 检查Redis服务器系统进程

```shell
ps -aux|grep redis
```

### 通过启动命令检查Redis服务器状态

```shell
netstat -nlt|grep 6379
```

### 访问客户端

```shell
redis-cli
```

## 概述

Redis 是速度非常快的非关系型（NoSQL）内存键值数据库，可以存储键和五种不同类型的值之间的映射。
官方介绍：

> Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker. It supports data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs and geospatial indexes with radius queries. Redis has built-in replication, Lua scripting, LRU eviction, transactions and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster.

## 运行

redis-server.exe redis.windows.conf

## 配置

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

## 数据类型

1. string 一个键最大存储512MB, 值是二进制安全的。
2. hash 是一个键值对集合
3. list
4. set
5. zset 有序集合

- [https://redislabs.com/ebook/part-1-getting-started/chapter-1-getting-to-know-redis/1-2-what-redis-data-structures-look-like/](https://redislabs.com/ebook/part-1-getting-started/chapter-1-getting-to-know-redis/1-2-what-redis-data-structures-look-like/)

## 命令

1. 设置过期时间
Redis有四个不同的命令用于设置键的生存时间或过期时间
EXP|RE <key> <ttl> 
PEXPIRE <key> <ttl>

- [Redis 命令参考中文文档](http://doc.redisfans.com/index.html)

## 备份与恢复

1. 备份 save 在redis安装目录创建dump.rdb文件
2. 恢复 将备份文件移动到安装目录，重启服务即可

## 数据结构

### 字典

dictht 是一个散列表结构，使用拉链法保存哈希冲突的dictEntry
Redis 的字典 dict 中包含两个哈希表 dictht，这是为了方便进行 rehash 操作。在扩容时，将其中一个 dictht 上的键值对 rehash 到另一个 dictht 上面，完成之后释放空间并交换两个 dictht 的角色。

### 跳跃表

是有序集合的底层实现之一。

跳跃表是基于多指针有序链表实现的，可以看成多个有序链表。

## 使用场景

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

## 参考

- [菜鸟教程](http://www.runoob.com/redis/redis-tutorial.html)
- [https://github.com/CyC2018/Interview-Notebook/blob/master/notes/Redis.md](https://github.com/CyC2018/Interview-Notebook/blob/master/notes/Redis.md)

- [springboot-redis-cache](https://blog.csdn.net/fanpeizhong/article/details/79998164)
- [https://docs.spring.io/spring-data/redis/docs/2.0.9.RELEASE/reference/html/#get-started](https://docs.spring.io/spring-data/redis/docs/2.0.9.RELEASE/reference/html/#get-started)





