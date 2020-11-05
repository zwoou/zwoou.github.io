# 类文件结构

[TOC]

## 1. Class类文件的结构

Class文件是一组为8位字节为基础单位的二进制流，各个数据项目严格按照顺序紧凑的排列在Class文件中。
包含两个部分：无符号数和表

- 前四个字节是魔数
- 56字节是次版本号
- 78字节是主版本号
JDK1.7主版本号最大是51.0
- 常量池
可以理解为Class文件之中的资源仓库。
由于数量不固定，在入口需要放置u2类型的数据，代表常量池容量记数值（constant_pool_count)。
常量池主要存放两大类常量：字面量和符号引用
- 访问标志
在常量池结束之后，紧接着的两个字节代表访问标志（access_flags)
用于识别类或接口层次的访问信息。  
- 类索引，父类索引和接口索引集合
- 字段表集合
- 方法表集合
- 属性表集合

## 2. 字节码(Bytecode)

  Java 所有的指令有 200 个左右，一个字节（ 位）可以存 256 种不同的指令信息，一个这样的字节称为字节码（ Bytecode ）
十六进制表示的二进制流通常是一个操作指令。使用助记符来替代纯数字.
字节码主要指令如下:
1. **加载或存储指令**
   - **将局部变量表加载到操作栈中**,如ILOAD(将int类型的局部变量压入栈)和ALOAD(将对象引用的局部变量压入栈)等
   - **从操作栈顶存储到局部变量表.**如ISTORE,ASTORE等.
   - **将常量加载到操作栈顶,这是极为高频使用的指令**.如ICONST,BIPUSH,SIPUSH,LDC等.
   
2. **运算指令**
   对两个操作栈帧上的值进行运算,并把结果写入操作栈顶.如IADD,IMUL等.

3. **类型转换指令**
   显示转换两种不同的数值类型.如I2L,D2F等.

4. **对象创建与访问指令**

   根据类进行对象的创建、初始化、方法调用相关指令，常见指令如下
   
   - 创建对象fft 令。 NEW NEWARRAY 等
   - 访问属性指令 GETF!ELD PUTFIELD GETSTATIC 等。
   - 检查实例类别指令。 INSTANCEOF, CHECKCAST 等。

5. **操作栈管理指令**
   JVM 提供了直接控制操作桔的指令，常见指令如下·

   - 出栈操作。如 POP 即一个元素， POP2 即两个元素。

   - 复制栈顶元素并压入栈 ,如DUP

6. **方法调用与返回指令**
   常见指令如下
   ( I ) INVOKEYIRTUAL 指令 调用对象的实例方法。
   ( 2) INVOKESPECIAL 指令 调用实例初始化方法、私有方法、父类方法等。
   ( 3 ) INVOKEST TIC指令 调用类静态方法。
   ( 4 ) RETURN 指令 返回 VOID 类型。
   
7. **同步指令**

   JVM使用方法结构中的 ACC_SYNCHRONIZED 标志同步方法,指令集中有 MONITORENTER和 MONJTOREXIT 支持 synchroni zed 语义。

## 3. 虚拟机类加载机制

1. 类加载时机
    加载，验证，准备，解析，初始化，使用和卸载。

2. 类加载机制

  JVM 的类加载是通过 ClassLoader 及其子类来完成的，类的层次和加载顺序如下：
  自底向上检查是否加载类， 自顶向下尝试加载类。

    1. Bootstrap ClassLoader 由c++实现，不是ClassLoader的子类。Load JRE/lib/rt.jar
    2. Extension ClassLoader 负责加载java平台扩展功能jar包。Load JRE/lib/ext/*.jar
    3. App ClassLoader 负责加载classpath中的jar包或class
    4. Custom ClassLoader java.long.ClassLoader的子类自定义加载class，如Tomcat、jboss会根据j2ee规范实现ClassLoader.
### 3.1 双亲委派模型（Parents Delegation Model)

从虚拟机的角度来讲，只存在两种不同的类加载器：
一种是启动类加载器（Bootstrap ClassLoader），这个是c++实现的，是虚拟机的一部分  
一种是所有其他的类加载器，是java语言实现的，独立于虚拟机外部，并且全都继承自抽象类java.long.ClassLoader。   
从java开发人员角度看，可以划分的更细些：

   - 启动类加载器（Bootstrap CLassLoader）
- 扩展类加载器（Extension ClassLoader)  在JDK9以上 改为Platform ClassLoader 即平台类加载器
- 应用程序类加载器（Application ClassLoader）
  上述的类加载器之间的关系，称为类加载器的双亲委派模型（Parents Delegation Model)
  双亲委派模型要求除了顶层的启动类加载器外，其余的类加载器都应当有自己的父类加载器。  
  这里的加载器之间的父子关系一般不会以继承关系实现，而是以组合的关系来复用父加载器的代码。

-XX:+TraceClassLoading 参数  启动时查看类的加载过程