# Spring Security使用



## 认证（Authentication）和授权（Authorization）

认证：首先明确“你是谁”的问题，也就是对身份是否合法的认证

授权："你能做什么"

## Spring Security的架构

Spring Security 采用是管道-过滤器（Pipe-Filter）模式

项目一旦启动，过滤器链将会实现自动配置：如下

![spring1](https://gitee.com/zwoou//picgo/raw/master/pic//spring1.png)

![image-20220126172110733](https://gitee.com/zwoou//picgo/raw/master/pic//image-20220126172110733.png)







## 参考

- [springboot2集成security](https://blog.csdn.net/yuanlaijike/article/details/80249235)