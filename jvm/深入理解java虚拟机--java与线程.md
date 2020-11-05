---
title: 深入理解java虚拟机--java与线程  
tags: 
    - 多线程
    - JVM
---
## java内存模型
1. 缓存一致性问题
处理器，高速缓存器，主内存之间的关系。
2. java内存模型
java线程、工作内存、主内存之间的关系。
- 内存间交互操作，java定义了8种。每一种都是原子性的。
	1. lock(锁定)：作用于主内存的变量，他把一个变量标识为一条线程独占的状态。
	2. unlock(解锁）：作用于主内存的变量，他把一个处于锁定状态的变量释放出来。
	3. read(读取)：作用于主内存的变量，他把一个变量的值传输到工作内存中。
	4. load(载入)：作用于工作内存中的变量，他把read操作从主内存中得到的变量值放入工作内存的变量副本中。
	5. use(使用)：作用于工作内存的变量，他把工作内存中一个变量的值传递给执行引擎。
	6. assign(赋值)：作用于工作内存的变量，他把一个从执行引擎接收到的值赋给工作内存的变量
	7. store(存储)：作用于工作内存中的变量，他把工作内存中的变量传到主内存中。
	8. write(写入)：作用于主内存的变量，他把store操作从工作内存中得到的变量放入到主内存的变量中。
<!--more-->
### 对于volatile型变量的特殊规则
当一个变量定义为volatile之后，它将具备两种特性
    1. 保证此变量对所有线程的可见性。
    2. 禁止指令重排序优化，普通的变量仅仅会保证在该方法的执行过程中所有依赖赋值结果的地方都能获得正确的结果， 
    而不能保证变量赋值操作的顺序与程序代码的执行顺序一致。
### 对于long和double型变量的特殊规则
几乎各平台都选择把64位数据的读写操作作为原子操作来对待。   
### 原子性、可见性和有序性
    - 原子性（Atomicity）保证原子性变量操作包括read、load、assign,use,store和write,基本类型的访问读写是具有原子性的。
    如果应用场景更大范围，字节码指令monitorenter和monitorexit来隐式使用这两个操作，java代码中就是 
    synchronized关键字
   - 可见性（Visibility）是指当一个线程修改了共享变量的值，其他线程都能立即得知这个修改。
    除了volatile之外，java中还有两个关键字可以实现可见性，即synchronized和final.
    - 有序性（Ordering）
### 先行发生原则
主要判断数据是否存在竞争，线程是否安全的依据
天然的先行发生关系
    - 程序次序规则（Program Order Rule）：在一个线程内，控制流顺序。
    - 管程锁定规则（Monitor Lock Rule）：一个unlock操作，先行发生于后面对同一个锁的lock操作。后面指时间上顺序。
    - volatile变量规则（Volatile Variable Rule）： 对一个volatile变量写操作先行发生于后面对这个变量的读操作。
    - 线程启动规则（Thread Start Rule）：Thread对象的是他start（）方法先行发生于此线程的每一个动作。
    - 线程终止规则（Thread Termination Rule）：线程中的所有操作都先行发生于对此线程的终止检测，可以通过Thread.join()方法结束，Thread.isAlive()方法检测线程已经终止。
    - 线程中断规则（Thread Interruption Rule）：对线程interrup()方法调用先行发生于被中断线程的代码检测到中断事件的发生。
    - 对象终结规则（Finallzer Rule) : 一个对象的初始化完成（构造方法完成）先行发生于他的finalize()方法的开始。
    - 传递性（Transitivity）：
## java与线程

### 线程的实现
    - 使用内核线程实现（Kernel-Level Thread ， KLT）
       轻量级进程（Light Weight Process ,LWP)
    - 使用用户线程实现
    - 使用用户线程加轻量级进程混合实现
### 状态转换
Java语言定义了5种线程状态：
1. 新建（New）：创建后尚未启动的线程处于这种状态。
2. 运行（Runable）：包括了操作系统线程状态中的Running和Ready.可能正在运行或等待CPU调度。
3. 无限期等待（Waiting）
    处于这种状态的线程不会被分配CPU执行时间，他们要等待被其他线程显示的唤醒，以下方法会让线程 
陷入无限期等待的状态
    - 没有设置Timeout参数的Object.wait() 方法
    - 没有设置Timeout参数的Thread.join() 方法
    - LockSupport.park()方法
4. 限期等待（Timed Waiting）：处于这种状态的线程也不会被分配CPU执行时间，不过无须等待被其他线程显示唤醒。
    - Thread.sleep()
    - 设置了Timeout参数的Object.wait() 方法
    - 设置了Timeout参数的Thread.join() 方法
    - LockSupport.parkNanos()
    - LockSupport.parkUntil()
5. 阻塞（Blocked）：阻塞状态和等待状态的区别是，阻塞状态在等待着一个排它锁，这个事件是另外一个线程放弃这个锁的时候发生。
6. 结束（Terminatcd)
    




	