---
title: mybatis
tags:
    - mybatis
---
## 概述

MyBatis is a first class persistence framework with support for custom SQL, stored procedures and advanced mappings. MyBatis eliminates almost all of the JDBC code and manual setting of parameters and retrieval of results. MyBatis can use simple XML or Annotations for configuration and map primitives, Map interfaces and Java POJOs (Plain Old Java Objects) to database records. 

## 整体架构
从功能流程层次描述MyBatis的整体架构图
[![PJOyIs.png](https://s1.ax1x.com/2018/07/24/PJOyIs.png)](https://imgchr.com/i/PJOyIs)
下面是MyBatis源码包对应的架构图
[![PJXnoj.png](https://s1.ax1x.com/2018/07/24/PJXnoj.png)](https://imgchr.com/i/PJXnoj)
## 功能架构
1. API接口层：提供给外部使用的接口API，开发人员通过这些本地API来操纵数据库。接口层一接收到调用请求就会调用数据处理层来完成具体的数据处理。
2. 数据处理层：负责具体的SQL查找，SQL解析，SQL执行和结果映射，主要目的是根据调用的请求完成一次数据库操作
3. 基础支撑层：负责最基础的功能支撑，包括连接管理、事务管理、配置加载、和缓存处理，这些都是公用的东西。
## SqlSession
SqlSession的实现类有DefaultSqlSession、SqlSessionManager和mybatis-spring的实现SqlSessionTemplate
1. DefaultSqlSession的线程不安全性
2. SqlSessionTemplate是如何使用DefaultSqlSession的
SqlSessionManager和SqlSessionTemplate都可以作SqlSession，并且在各自类里都对SqlSession做了动态代理。区别是SqlSessionTemplate的动态代理更高效（有SqlSession的引用计数），并且有对Spring框架的配合。动态代理的实现不容易复用，所以干脆分开，做到低耦合。  
## 参考  
- [https://www.cnblogs.com/mengheng/p/3739610.html](https://www.cnblogs.com/mengheng/p/3739610.html)
- [https://blog.csdn.net/luanlouis/article/details/40422941](https://blog.csdn.net/luanlouis/article/details/40422941)
- [SqlSession](https://mp.weixin.qq.com/s?__biz=MzI1NDQ3MjQxNA==&mid=2247485368&idx=1&sn=791fd662059713c6dfa97cef8580294e&chksm=e9c5fe09deb2771fc191655c7554efd83dd6b170976730369f430e5457dd479c7832aea937c3&scene=21#wechat_redirect)






