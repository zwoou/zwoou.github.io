---
title: spring-cloud02-服务消费者（rest+ribbon）
tags:
    - springcloud
---
## ribbon简介
ribbon是一个负载均衡客户端，可以很好的控制htt和tcp的一些行为。Feign默认集成了ribbon。

ribbon 已经默认实现了这些配置bean：主要实现通过LoadBalancerClient

    - IClientConfig ribbonClientConfig: DefaultClientConfigImpl
    
    - IRule ribbonRule: ZoneAvoidanceRule
    
    - IPing ribbonPing: NoOpPing
    
    - ServerList ribbonServerList: ConfigurationBasedServerList
    
    - ServerListFilter ribbonServerListFilter: ZonePreferenceServerListFilter
    
    - ILoadBalancer ribbonLoadBalancer: ZoneAwareLoadBalancer
## 建一个服务消费者
1. 重新新建一个spring-boot工程，取名为：service-ribbon;
在它的pom.xml文件分别引入起步依赖spring-cloud-starter-eureka、spring-cloud-starter-ribbon、spring-boot-starter-web
2. 在工程的配置文件指定服务的注册中心地址为http://localhost:8761/eureka/，程序名称为 service-ribbon，程序端口为8764。配置文件application.yml如下：
```
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/
server:
  port: 8764
spring:
  application:
    name: service-ribbon
```
3. 在工程的启动类中,通过@EnableDiscoveryClient向服务中心注册；并且向程序的ioc注入一个bean: restTemplate;并通过@LoadBalanced注解表明这个restRemplate开启负载均衡的功能。
```
@SpringBootApplication
@EnableDiscoveryClient
public class ServiceRibbonApplication {

    public static void main(String[] args) {
        SpringApplication.run(ServiceRibbonApplication.class, args);
    }

    @Bean
    @LoadBalanced
    RestTemplate restTemplate() {
        return new RestTemplate();
    }

}

```
4. 写一个测试类HelloService，通过之前注入ioc容器的restTemplate来消费service-hi服务的“/hi”接口，在这里我们直接用的程序名替代了具体的url地址，在ribbon中它会根据服务名来选择具体的服务实例，根据服务实例在请求的时候会用具体的url替换掉服务名，代码如下：
```
@Service
public class HelloService {

    @Autowired
    RestTemplate restTemplate;

    public String hiService(String name) {
        return restTemplate.getForObject("http://SERVICE-HI/hi?name="+name,String.class);
    }

}
```
5. 写一个controller，在controller中用调用HelloService 的方法，代码如下：
```
@RestController
public class HelloControler {

    @Autowired
    HelloService helloService;
    @RequestMapping(value = "/hi")
    public String hi(@RequestParam String name){
        return helloService.hiService(name);
    }


}
```