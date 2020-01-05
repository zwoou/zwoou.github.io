---
title: RabbitMQ
tags:
    - RabbitMQ
---
## 安装
1. 首先安装Erlang OTP 
地址 [http://www.erlang.org/downloads](http://www.erlang.org/downloads)
 **注意：**版本一致。 
2. 配置环境变量
    - ERLANG_HOME
    - RABBITMQ_HOME
3. 查看Erlang版本 ,DOS下输入erl
```
erl
```
4. 安装RabbitMQ-server 
地址 [http://www.rabbitmq.com/install-windows.html ](http://www.rabbitmq.com/install-windows.html )
## 运行
进入RabbitMQ 安装路径sbin下
1. 开启插件
rabbitmq_managemen是管理后台的插件、我们要开启这个插件才能通过浏览器访问登录页面

进入到sbin目录下：rabbitmq-plugins enable rabbitmq_managemen
2. 开启服务：rabbitmq-server start
3. 进入管理后台
开启浏览器访问http://localhost:15672

默认userName:guest    password:guest
## 创建用户并授权角色cc
1. 创建用户
rabbitmqctl.bat add_user cc 123456
2. 授权角色
    (1) 超级管理员(administrator)
      可登陆管理控制台(启用management plugin的情况下)，可查看所有的信息，并且可以对用户，策略(policy)进行操作。
    (2) 监控者(monitoring)
      可登陆管理控制台(启用management plugin的情况下)，同时可以查看rabbitmq节点的相关信息(进程数，内存使用情况，磁盘使用情况等) 
    (3) 策略制定者(policymaker)
      可登陆管理控制台(启用management plugin的情况下), 同时可以对policy进行管理。
    (4) 普通管理者(management)
       仅可登陆管理控制台(启用management plugin的情况下)，无法看到节点信息，也无法对策略进行管理。
    (5) 其他的
无法登陆管理控制台，通常就是普通的生产者和消费者。 

    - 添加角色
rabbitmqctl.bat set_user_tags cc administrator
    - 查看
    rabbitmqctl.bat list_users
    - 修改密码
    rabbitmqctl change_password userName newPassword
    - 删除
    rabbitmqctl.bat delete_user username
## 错误处理
1、rabbit服务未启动

rabbitmqctl status
解决方式：进入到sbin目录下执行命令

rabbitmq-server stop

rabbitmq-server start



再次运行：rabbitmqctl status
## 参考
- [https://www.cnblogs.com/ericli-ericli/p/5902270.html](https://www.cnblogs.com/ericli-ericli/p/5902270.html)
- [https://blog.csdn.net/qq_33382113/article/details/78853680](https://blog.csdn.net/qq_33382113/article/details/78853680)













