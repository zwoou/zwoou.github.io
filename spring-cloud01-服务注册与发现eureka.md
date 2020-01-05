---
title: springcloud01-服务注册与发现(eureka)
tags:
    - springcloud
---

# springcloud01-eureka

1. 创建maven工程，然后创建2个model工程:一个model工程作为服务注册中心，即Eureka Server,另一个作为Eureka Client。
maven

```xml
        <!--eureka server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka-server</artifactId>
        </dependency>
 ```

1. 启动一个服务注册中心，在启动类上添加

 ```java
 @EnableEurekaServer
 ```

 1. eureka是一个高可用的组件，它没有后端缓存，每一个实例注册之后需要向注册中心发送心跳（因此可以在内存中完成），在默认情况下erureka server也是一个eureka client ,必须要指定一个 server。eureka server的配置文件appication.yml：

 ```yml
 server:
  port: 8761

eureka:
  instance:
    hostname: localhost
  client:
    registerWithEureka: false
    fetchRegistry: false
    serviceUrl:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

通过eureka.client.registerWithEureka：false和fetchRegistry：false来表明自己是一个eureka server. **防止自己注册自己**  

1. eureka server 是有界面的，启动工程,打开浏览器访问：

[http://lcalhost:8761](http://lcalhost:8761) ,界面如下：
<!-- more -->

## 创建一个服务提供者 (eureka client)

1. 当client向server注册时，它会提供一些元数据，例如主机和端口，URL，主页等。Eureka server 从每个client实例接收心跳消息。 如果心跳超时，则通常将该实例从注册server中删除。
maven

```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
        </dependency>
```

**注意：** spring cloud F版本 引入的包不太一样。
2. 通过注解@EnableEurekaClient 表明自己是一个eurekaclient.
3. 仅仅@EnableEurekaClient是不够的，还需要在配置文件中注明自己的服务注册中心的地址，application.yml配置文件如下：

```yml
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
server:
  port: 8762
spring:
  application:
    name: service-hi
```

## 参考

- [https://blog.csdn.net/forezp/article/details/81040925](https://blog.csdn.net/forezp/article/details/81040925)
