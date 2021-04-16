# tomcat

@TOC
Tomcat内核设计剖析


## Web服务器机制

- 通信协议
- Socket通信
- Web服务器模型

## 1. 通信协议

1. HTTP/HTTPS

[![mXYv0H.png](https://s2.ax1x.com/2019/08/30/mXYv0H.png)](https://imgchr.com/i/mXYv0H)

2. HTTPS工作原理及流程

![mXddde.png](https://s2.ax1x.com/2019/08/30/mXddde.png)

3. HTTP请求/响应模型

![mXgoAx.png](https://s2.ax1x.com/2019/08/30/mXgoAx.png)

请求头部常见典型属性

- User-Agent
- Accept
- Host
- Cookie 
- Referer

![mX4lng.png](https://s2.ax1x.com/2019/08/30/mX4lng.png)

常用响应报文头属性

- Cache-Control 
- Location
- Set-Cookie

## 服务器模型

1. 单线程非阻塞IO模型

    1. 应用程序遍历套接字的事件检测
    2. 内核遍历套接字的事件检测
    3. 内核基于回调的事件检测

2. 多线程非阻塞IO模型

Reactor模式
![https://i.postimg.cc/Y023bsy8/48.png](https://i.postimg.cc/Y023bsy8/48.png)

## Servlet规范

Servlet规范的核心接口就是Servlet接口,在javaAPI中定义了两个抽象类 GenericServlet和HttpServlet

- ServletRequest 接口
- ServletContext 接口
- ServletResponse 接口
- Filter 接口

## 端口被占用
两个命令：
```
netstat -ano | findstr 8080

taskkill -pid 进程pid -f
```

## 调优

- [https://www.cnblogs.com/sunfenqing/p/7339058.html](https://www.cnblogs.com/sunfenqing/p/7339058.html)