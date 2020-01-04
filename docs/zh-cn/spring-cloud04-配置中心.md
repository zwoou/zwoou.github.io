---
title: spring-cloud04-配置中心
tags: 
    - springcloud
---
## 分布式配置中心(Spring Cloud Config)
在分布式系统中，由于服务数量巨多，为了方便服务配置文件统一管理，实时更新，所以需要分布式配置中心组件。在Spring Cloud中，有分布式配置中心组件spring cloud config ，它支持配置服务放在配置服务的内存中（即本地），也支持放在远程Git仓库中。在spring cloud config 组件中，分两个角色，一是config server，二是config client。
## 构建Config Server
1. maven
```
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
        </dependency>
                <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka</artifactId>
        </dependency>
```
2. 入口Application类加上@EnableConfigServer注解开启配置服务器的功能
3. application.properties文件配置以下：
```
spring.application.name=config-server
server.port=8888


spring.cloud.config.server.git.uri=https://github.com/forezp/SpringcloudConfig/
spring.cloud.config.server.git.searchPaths=respo
spring.cloud.config.label=master
spring.cloud.config.server.git.username=your username
spring.cloud.config.server.git.password=your password
```
如果Git仓库为公开仓库，可以不填写用户名和密码
4. 启动程序：访问http://localhost:8888/foo/dev

http请求地址和资源文件映射如下:

   - /{application}/{profile}[/{label}]
   - /{application}-{profile}.yml
   - /{label}/{application}-{profile}.yml
   -  /{app lication}-{profile}.properties
   - /{label}/{application}-{profile}.properties
   - 
## 构建一个config client
1. maven
```
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
```
2. 其配置文件bootstrap.properties：
```
spring.application.name=config-client
spring.cloud.config.label=master
spring.cloud.config.profile=dev
spring.cloud.config.uri= http://localhost:8888/
server.port=8881
```
spring.cloud.config.uri= http://localhost:8888/ 指明配置服务中心的网址。

## 高可用配置中心改造
将配置中心做成一个微服务，将其集群化，从而达到高可用
1. 准备服务注册中心
2. 引入Eureka
3. 配置文件application.yml，指定服务注册地址为http://localhost:8889/eureka/
4. 客户端配置文件bootstrap.properties，注意是bootstrap。加上服务注册地址为http://localhost:8889/eureka/
```
spring.application.name=config-client
spring.cloud.config.label=master
spring.cloud.config.profile=dev
#spring.cloud.config.uri= http://localhost:8888/

eureka.client.serviceUrl.defaultZone=http://localhost:8889/eureka/
spring.cloud.config.discovery.enabled=true
spring.cloud.config.discovery.serviceId=config-server
server.port=8881
``` 

    spring.cloud.config.discovery.enabled 是从配置中心读取文件。
    spring.cloud.config.discovery.serviceId 配置中心的servieId，即服务名。
