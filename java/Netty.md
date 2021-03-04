# Netty

[TOC]



## 1. 四种主要的IO模型

1. 同步阻塞IO(Blocking IO)

2. 同步非阻塞IO(Non-blocking IO)

3. IO多路复用(IO Multiplexing)

   即经典的Reactor 反应器设计模式 ,有时也称为异步阻塞IO

4. 异步IO(Asyncchronous IO)

## 2. java NIO

三大核心组件

- Buffer
- Channel
- Selector

### 2.1 Buffer

1. 本质上一个内存块 ,是一个非线程安全的类

2. 重要属性

   | 属性     | 说明     |
   | -------- | -------- |
   | capacity | 内部容量 |
   | limit    | 上限     |
   | position | 位置     |
   | mark     |          |

   

### 2.2 Channel

四种Channel实现

- FileChannel 阻塞模式,不能设置为非阻塞模式
- SocketChannel
- ServerSocketChannel
- DatagramChannel

## 2. Reactor反应器模式


反应器模式由Reactor反应器线程,Handlers处理器两大角色组成.

- Reactor反应器线程的职责:负责响应IO事件,并且分发到Handlers处理器.
- Handlers处理器的职责:非阻塞的执行业务逻辑.