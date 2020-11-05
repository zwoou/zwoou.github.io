# 内存结构
[TOC]
## 1. 内存结构



![image-20201031222328584](https://gitee.com/zwoou/picgo/raw/master/pic/20201031222328.png)



java程序执行过程图
[![PYkIc4.png](https://s1.ax1x.com/2018/07/24/PYkIc4.png)](https://imgchr.com/i/PYkIc4)

## 2. 运行时数据区

[![PYAMbn.png](https://s1.ax1x.com/2018/07/24/PYAMbn.png)](https://imgchr.com/i/PYAMbn)
Java虚拟机栈、本地方法栈、程序计数器是线程私有。

堆和方法区 是所有线程共享的数据区



[![PYATG8.png](https://s1.ax1x.com/2018/07/24/PYATG8.png)](https://imgchr.com/i/PYATG8)

- **程序计数器 （Program Counter Register）**
    可以看作是当前线程行号指示器，
    如果是java方法，这个计数器记录的是正在执行的字节码指令的地址，
    如果是Native方法，这个记录器的值为空。
    
- **Java虚拟机栈 java Virtual Machine Stack**
[![PYE0yQ.png](https://s1.ax1x.com/2018/07/24/PYE0yQ.png)](https://imgchr.com/i/PYE0yQ)  

  它的生命周期与线程相同.虚拟机栈描述的是Java方法执行的线程内存模型:  
  
  每个方法在执行时都会创建一个栈帧（Stack Frame）
    用于存储局部变量表(Local Variables)、操作数栈(Operand Stack)、指向运行时常量池的引用（Reference to runtime constant pool）、方法返回地址(Return Address)、附加信息等信息，
    每一个方法从调用到执行完成的过程，就对应着一个栈帧在虚拟机栈中入栈到出栈的过程。
  
  
  
    - 局部变量表
        - 存放编译期可知的各种基本数据类型、对象引用类型和returnAddress类型
        - long、double占用两个局部变量槽(Slot)，其余占用一个
    - 可能抛出以下异常
        - 当线程请求的栈深度超过最大值，会抛出 StackOverflowError 异常；
        - 栈进行动态扩展时如果无法申请到足够内存，会抛出 OutOfMemoryError 异常。
  
- **本地方法栈(Native Method Stacks)**

    与java虚拟机栈类似，区别是本地方法栈为native方法服务
    
- **Java堆(Heap)**

    创建的对象在堆中分配，大小可以通过-Xmx和-Xms来控制。
    Java堆是向高地址扩展的数据结构，是不连续的内存区域。
    
    这块区域是垃圾收集器管理的主要区域（"GC 堆 "）。
    现在收集器基本都是采用分代收集算法，
    Java 堆还可以分成：新生代和老年代（新生代还可以分成 Eden 空间、From Survivor (幸存者)、To Survivor 空间等）。
    
    绝大部分对象在Eden区生成,当Eden区装填满的时候,会触发Young Gtarbage Collection ,即YGC
    
- **方法区(Method Area)**
    保存方法代码（编译后的java代码）和符号表.
    存放了要加载的类信息(包括类的名称、方法信息、字段信息)、静态变量、final类型的常量、属性和方法信息。
    

    可通过-XX:PermSize和-XX:MaxPermSize来指定最小值和最大值,JDK8 废弃了永久代的概念,改用本地内存(Native Memory)实现,也就是Metaspace(元空间)
    
- **运行时常量池(Runtime Constant Pool)**

    运行时常量池是方法区的一部分.
    类加载后，Class 文件中的常量池（用于存放编译期生成的各种字面量和符号引用）就会被放到这个区域。
    在运行期间也可以用过 String 类的 intern() 方法将新的常量放入该区域。
    java通过持久代（Permanent Generation)来存放方法区，
    JDK1.7之后，Hotspot虚拟机便将运行时常量池从永久代移除了，用metaspace(元数据)区替代。
    **java中几种常量池的区分**
    
    - 全局字符串池（String pool也有叫做String literal pool)
    - class文件常量池（class constant pool）
    - 运行时常量池（runtime constant pool)
    
- **直接内存**

    直接内存（Direct Memory)并不是虚拟机运行时数据区的一部分，也不是java虚拟机规范中定义的内存区域。
    在 JDK 1.4 中新加入了 NIO 类，引入了一种基于通道（Channel）与缓冲区（Buffer）的 I/O 方式，
    它可以使用 Native 函数库直接分配堆外内存，
    然后通过一个存储在 Java 堆里的 DirectByteBuffer 对象作为这块内存的引用进行操作。
    这样能在一些场景中显著提高性能，因为避免了在 Java 堆和 Native 堆中来回复制数据。
    NIO的Buffer提供了一个可以不经过JVM内存直接访问系统物理内存的类——DirectBuffer
    直接内存的读写操作比普通Buffer快，但它的创建、销毁比普通Buffer慢。
    因此直接内存使用于需要大内存空间且频繁访问的场合，不适用于频繁申请释放内存的场合。
    
- **Metaspace(元空间)**

    

## 3. HotSpot对象揭秘
在java堆中对象分配，布局和访问的全过程
- 对象的访问定位 两种
    - 使用句柄
    - 直接指针
## 4. 内存溢出
使用eclipse memory analyzer内存映像分析工具分析堆内存。来判断是内存泄漏还是内存溢出。    

## 参考

- [jvm的内存区域划分](https://www.cnblogs.com/dolphin0520/p/3613043.html)
















