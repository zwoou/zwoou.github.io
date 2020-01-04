---
title: spring-cloud05-消息总线（Spring Cloud Bus) 
tags: 
    - springcloud
---
## spring-cloud05-消息总线（Spring Cloud Bus）
Spring Cloud Bus 将分布式的节点用轻量的消息代理连接起来。它可以用于广播配置文件的更改或者服务之间的通讯，也可以用于监控。本文要讲述的是用Spring Cloud Bus实现通知微服务架构的配置文件的更改。
## 改造config-client
1. 在pom文件加上起步依赖spring-cloud-starter-bus-amqp，完整的配置文件如下：
```
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-bus-amqp</artifactId>
        </dependency>
        
            <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
```
2. 在配置文件application.properties中加上RabbitMq的配置，包括RabbitMq的地址、端口，用户名、密码，代码如下：
```
spring.rabbitmq.host=localhost
spring.rabbitmq.port=5672
# spring.rabbitmq.username=
# spring.rabbitmq.password=
```
3. 发送post请求：http://localhost:8881/bus/refresh，你会发现config-client会重新读取配置文件
4. 另外，/bus/refresh接口可以指定服务，即使用”destination”参数，比如 “/bus/refresh?destination=customers:**” 即刷新服务名为customers的所有服务，不管ip。

## Spring Boot 2.0.2坑有以下：
1、在config-client的Controller上要添加注解@RefreshScope。
2、在config-client配置文件中（properties或者yml）添加management.endpoints.web.exposure.include= *
management:
  endpoints:
    web:
      exposure:
        include: bus-refresh
3、请求的地址是（POST）http://host:port/actuator/refresh (可以在spring boot监控（mappings中查询refresh）)
 - 所有
 /actuator/bus-refresh
 - 指定
 /actuator/bus-refresh/customers:9000

 ## 参考

 - [https://www.fangzhipeng.com/springcloud/2018/08/08/sc-f8-bus.html](https://www.fangzhipeng.com/springcloud/2018/08/08/sc-f8-bus.html)