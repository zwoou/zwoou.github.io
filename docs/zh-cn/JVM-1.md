---
title: JVM
tags: JVM
---
1. 每个 JVM 都包含：方法区、Java 堆、Java 栈、本地方法栈、指令计数器及其他隐含计数器。
2. java 代码编译和执行的整个过程
    1. Java 代码编译是由Java源码编译器来完成，也就是 Java 代码到 JVM 字节码（.class文件)的过程
    2. Java 字节码的执行是由 JVM 执行引擎来完成。
Java 代码编译和执行的整个过程包含三个重要机制
    1. java 源码编译机制  三个过程：
        1. 分析和输入到符号表
        2. 注解处理
        3. 语义分析和生成 class 文件
        生成的文件结构组成：
        1. 结构信息 包括class文件格式版本号及
        2. 元数据 ： 对应源码中的声明与常量信息
        3. 方法信息
    2. 类加载机制
    JVM 的类加载是通过 ClassLoader 及其子类来完成的，类的层次和加载顺序如下：
    自底向上检查是否加载类， 自顶向下尝试加载类。
        1. Bootstrap ClassLoader 由c++实现，不是ClassLoader的子类。Load JRE/lib/rt.jar
        2. Extension ClassLoader 负责加载java平台扩展功能jar包。Load JRE/lib/ext/*.jar
        3. App ClassLoader 负责加载classpath中的jar包或class
        4. Custom ClassLoader java.long.ClassLoader的子类自定义加载class，如Tomcat、jboss会根据j2ee规范实现ClassLoader.
    3. 类执行机制
        JVM是基于堆栈的虚拟机。
        堆栈以帧为单位保存线程的状态。JVM对堆栈只进行两种操作：以帧为单位的压栈和出栈操作。        

3. JVM内存管理及垃圾回收机制
    JVM 内存结构分为：方法区（method),栈内存（stack),堆内存（heap),本地方法栈（java中的jni调用）
    1. 堆内存（heap）
      创建的对象在堆中分配，大小可以通过-Xmx和-Xms来控制。堆内存是向高地址扩展的数据结构，是不连续的内存区域。
      
    2. 栈内存（stack）
      栈是向低地址扩展的数据结构，是一块连续的内存区域。
    3. 本地方法栈（java中的jni调用）
        用于支持native方法的执行
    4. 方法区（method）
      保存方法代码（编译后的java代码）和符号表.存放了要加载的类信息、静态变量、final类型的常量、属性和方法信息。java通过持久代（Permanent Generation)来存放方法区，可通过-XX:PermSize和-XX:MaxPermSize来指定最小值和最大值
    垃圾回收机制
4. class文件结构
    javap -verbose Test.class
    1. 魔数 magic
    2. minor_version 和 major_version
    3. 常量池相关的数据项 
        文字字符串，常量值，当前类的类名，字段名，方法名，各个字段和方法的描述符，对当前类的字段和方法的引用信息，当前类对其他类的引用信息等等。
5. 方法区
    1. 在一个JVM实例内部，类型信息都会被存储在一个称为方法区的内存逻辑区。类型信息是类加载时，从类文件中提取出来的，类变量（类中静态变量）也存储在方法区中。
    2. JVM运行应用时要大量使用存储在方法区中的类型信息，其次方法区是被所有线程共享的，必须考虑数据的线程安全。
    方法区存储内容
        1. 类的类型信息
        2. 常量池
        3. 域信息（程序中的一个范围）
        4. 方法信息
        5. 类变量 （Class Variables） 类的静态变量
            - 类变量被类的所有实例共享，在JVM使用一个类之前，他必须在方法区为每个non-final类变量分配空间。
            - 常量（被final修饰的类变量）的处理方法不同，每个常量都会在常量池中有一个拷贝。
            注意：
            java类中的成员变量有静态和非静态，静态成员变量是共享数据，在共享区，也叫方法区。
            非静态成员变量在堆内存中，作用于整个类，在堆上创建对象时即给其分配区域。
            局部变量在栈内存内，jvm栈为每一个类都分配一个栈帧，用于存放函数中的局部变量（基本类型），对象的引用类型都会在此分配内存，引用指向的对象在堆上。

            
参考
1. https://www.cnblogs.com/lishun1005/p/6019678.html
2. http://blog.csdn.net/super_yc/article/details/71439673
3. http://blog.csdn.net/super_yc/article/details/71420110
4. https://www.jianshu.com/p/ee3e9dff5700






















