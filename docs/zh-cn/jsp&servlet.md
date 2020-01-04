---
title: servlet
date: 2017-08-29 15:27:21 
tags: web
---

## 1. 简介 ##
    
- sun公司开发的用于显示动态web资源技术
- 定义了servlet接口，实现接口
- 发布到服务器用浏览器打开
## 2. 运行过程 ##
	
- servlet由web服务器调用，web服务器受到客户端的servlet请求后做：
- ①Web服务器首先检查是否已经装载并创建了该Servlet的实例对象。如果是，则直接执行第④步，否则，执行第②步。
- ②装载并创建该Servlet的一个实例对象。 
- ③调用Servlet实例对象的init()方法。
- ④创建一个用于封装HTTP请求消息的HttpServletRequest对象和一个代表HTTP响应消息的HttpServletResponse对象，然后调用Servlet的service()方法并将请求和响应对象作为参数传递进去。
- ⑤WEB应用程序被停止或重新启动之前，Servlet引擎将卸载Servlet，并在卸载之前调用Servlet的destroy()方法。 
## 3. servlet接口实现类 ##

- HttpServlet 通常继承该类，重写doPost方法
- GenericServlet
## 4. servlet的开发细节 ##

### 1. Servlet访问URL映射配置 ###
    <servlet>
    <servlet-name>ServletDemo1</servlet-name>
       <servlet-class>gacl.servlet.study.ServletDemo1</servlet-class>
    </servlet>
    <servlet-mapping>
       <servlet-name>ServletDemo1</servlet-name>
       <url-pattern>/servlet/ServletDemo1</url-pattern>
    </servlet-mapping>
### 2. servlet与普通Java类的区别 ###
Servlet是一个供其他Java程序（Servlet引擎）调用的Java类，它不能独立运行，它的运行完全由Servlet引擎来控制和调度。  
　　  针对客户端的多次Servlet请求，通常情况下，服务器只会创建一个Servlet实例对象，也就是说Servlet实例对象一旦创建，它就会驻留在内存中，为后续的其它请求服务，直至web容器退出，servlet实例对象才会销毁。  
　　  在Servlet的整个生命周期内，Servlet的init方法只被调用一次。而对一个Servlet的每次访问请求都导致Servlet引擎调用一次servlet的service方法。对于每次访问请求，Servlet引擎都会创建一个新的HttpServletRequest请求对象和一个新的HttpServletResponse响应对象，然后将这两个对象作为参数传递给它调用的Servlet的service()方法，service方法再根据请求方式分别调用doXXX方法。  
　　  如果在<servlet>元素中配置了一个<load-on-startup>元素，那么WEB应用程序在启动时，就会装载并创建Servlet的实例对象、以及调用Servlet实例对象的init()方法。
### 3. 线程安全 ###
- 1，加锁synchronized（this）
- 2,标记接口SingleThreadModel,创建多实例，无法解决
- 3，尽量少用全局变量。

1.Servlet概述
     1，运行在服务器端的java程序，通过http协议用于接收来自客户端的请求并发出响应。
     2，Servlet中的方法
     public void service()
    编写一个Java类实现servlet接口
    一旦创建就驻留内存（单例）
	Servlet核心类图
	参数配置 
      3,web.xml地址映射
	ServletConfig
    ServletContext 
  必须继承HttpServlet ,响应客户端的请求
	doGet
	doPost
	doPut
	doDelete
	必须重写doGet(),doPost()两个方法
	只需重写service()方法，即可响应客户端的所有请求
ServletContext application的java类
在线人数统计  绑定httpSessionBindingListerner
新建类 实现  绑定httpSessionBindingListerner接口 
## 5. URI和URL的区别 ##
URI 统一资源标识符，抽象概念，
URL 统一资源定位符
URN 统一资源名称
URI=URL+URN
























































