---
title: java 网络编程01——javaIO和NIO
tags: 
    - IO
    - java
---

# java IO和NIO,netty

- BIO: 传统的IO又称BIO，即阻塞式IO
- NIO：非阻塞式IO，同步
- AIO： 异步非阻塞式IO

## 流与块

## 磁盘操作

File类可以表示文件和目录，只用于表示文件信息，不表示内容。
## 字节操作
Java I/O 使用了装饰者模式来实现。以 InputStream 为例，InputStream 是抽象组件，FileInputStream 是 InputStream 的子类，属于具体组件，提供了字节流的输入操作。FilterInputStream 属于抽象装饰者，装饰者用于装饰组件，为组件提供额外的功能，例如 BufferedInputStream 为 FileInputStream 提供缓存的功能。
实例化一个具有缓存功能的字节流对象时，只需要在 FileInputStream 对象上再套一层 BufferedInputStream 对象即可。

``` java
BufferedInputStream bis = new BufferedInputStream(new file (file));
```
DataInputStream 装饰者提供了对更多数据类型进行输入的操作，比如 int、double 等基本类型。
PrintStream 是有害的

## 字符操作

GBK 编码中，中文占 2 个字节，英文占 1 个字节；
UTF-8 编码中，中文占 3 个字节，英文占 1 个字节；
Java 使用双字节编码 UTF-16be，中文和英文都占 2 个字节。

如果编码和解码过程使用不同的编码方式那么就出现了乱码。
## 对象操作
序列化就是将一个对象转换成字节序列，方便存储和传输。

序列化：ObjectOutputStream.writeObject()

反序列化：ObjectInputStream.readObject()

序列化的类需要实现 Serializable 接口，它只是一个标准，没有任何方法需要实现。

transient 关键字可以使一些属性不会被序列化。
ArrayList 序列化和反序列化的实现 ：ArrayList 中存储数据的数组是用 transient 修饰的，因为这个数组是动态扩展的，并不是所有的空间都被使用，因此就不需要所有的内容都被序列化。通过重写序列化和反序列化方法，使得可以只序列化数组中有内容的那部分数据。

private transient Object[] elementData;


## 通道和缓冲区

1. 通道 Channel
通道 Channel是对IO流的模拟，可以读取和写入数据，双向。
2. 缓冲区
发送给一个通道的所有数据都必须放到缓冲区，接收也一样。
## 缓冲区状态变量
- capacity: 最大容量;
- position: 当前已经读写的字节数;
- limit: 还可以读写的字节数。
## Reactor模式
讲到高性能IO绕不开Reactor模式，它是大多数IO相关组件如Netty、Redis在使用的IO模式，为什么需要这种模式，它是如何设计来解决高性能并发的呢？
最最原始的网络编程思路就是服务器用一个while循环，不断监听端口是否有新的套接字连接，如果有，那么就调用一个处理函数处理，类似：
while(true){
socket = accept();
handle(socket)
}
这种方法的最大问题是无法并发，效率太低，如果当前的请求没有处理完，那么后面的请求只能被阻塞，服务器的吞吐量太低。
omcat服务器的早期版本确实是这样实现的。多线程的方式确实一定程度上极大地提高了服务器的吞吐量，因为之前的请求在read阻塞以后，不会影响到后续的请求，因为他们在不同的线程中。这也是为什么通常会讲“一个线程只能对应一个socket”的原因。最开始对这句话很不理解，线程中创建多个socket不行吗？语法上确实可以，但是实际上没有用，每一个socket都是阻塞的，所以在一个线程里只能处理一个socket，就算accept了多个也没用，前一个socket被阻塞了，后面的是无法被执行到的。
线程池本身可以缓解线程创建-销毁的代价，这样优化确实会好很多，不过还是存在一些问题的，就是线程的粒度太大。每一个线程把一次交互的事情全部做了，包括读取和返回，甚至连接，表面上似乎连接不在线程里，但是如果线程不够，有了新的连接，也无法得到处理，所以，目前的方案线程里可以看成要做三件事，连接，读取和写入。
线程同步的粒度太大了，限制了吞吐量。应该把一次连接的操作分为更细的粒度或者过程，这些更细的粒度是更小的线程。整个线程池的数目会翻倍，但是线程更简单，任务更加单一。这其实就是Reactor出现的原因，
在Reactor中，这些被拆分的小线程或者子过程对应的是handler，每一种handler会出处理一种event。这里会有一个全局的管理者selector，我们需要把channel注册感兴趣的事件，那么这个selector就会不断在channel上检测是否有该类型的事件发生，如果没有，那么主线程就会被阻塞，否则就会调用相应的事件处理函数即handler来处理。
**改进：采用基于事件驱动的设计，当有事件触发时，才会调用处理器进行数据处理。**
[![PNepz6.png](https://s1.ax1x.com/2018/07/26/PNepz6.png)](https://imgchr.com/i/PNepz6)
Reactor：负责响应IO事件，当检测到一个新的事件，将其发送给相应的Handler去处理。

Handler：负责处理非阻塞的行为，标识系统管理的资源；同时将handler与事件绑定。

Reactor为单个线程，需要处理accept连接，同时发送请求到处理器中。

由于只有单个线程，所以处理器中的业务需要能够快速处理完。
** 改进：使用多线程处理业务逻辑。
[![PNetWq.png](https://s1.ax1x.com/2018/07/26/PNetWq.png)](https://imgchr.com/i/PNetWq)


** 继续改进：对于多个CPU的机器，为充分利用系统资源，将Reactor拆分为两部分。

Using Multiple Reactors继续改进：对于多个CPU的机器，为充分利用系统资源，将Reactor拆分为两部分。

Using Multiple Reactors
[![PNeDw4.png](https://s1.ax1x.com/2018/07/26/PNeDw4.png)](https://imgchr.com/i/PNeDw4)

mainReactor负责监听连接，accept连接给subReactor处理，为什么要单独分一个Reactor来处理监听呢？因为像TCP这样需要经过3次握手才能建立连接，这个建立连接的过程也是要耗时间和资源的，单独分一个Reactor来处理，可以提高性能。
## 两种IO模式：Proactor与Reactor模式
在高性能的I/O设计中，有两个比较著名的模式Reactor和Proactor模式，其中Reactor模式用于同步I/O，而Proactor运用于异步I/O操作。



## 多路复用IO模型

## 信号驱动IO模型

## 异步IO模型



参考
[https://github.com/CyC2018/Interview-Notebook/blob/master/notes/Java%20IO.md#%E4%B8%83nio](https://github.com/CyC2018/Interview-Notebook/blob/master/notes/Java%20IO.md#%E4%B8%83nio)