## Class类文件的结构
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
## 虚拟机类加载机制
1. 类加载时机
  加载，验证，准备，解析，初始化，使用和卸载。

2. 类加载机制

  JVM 的类加载是通过 ClassLoader 及其子类来完成的，类的层次和加载顺序如下：
  自底向上检查是否加载类， 自顶向下尝试加载类。

    1. Bootstrap ClassLoader 由c++实现，不是ClassLoader的子类。Load JRE/lib/rt.jar
    2. Extension ClassLoader 负责加载java平台扩展功能jar包。Load JRE/lib/ext/*.jar
    3. App ClassLoader 负责加载classpath中的jar包或class
    4. Custom ClassLoader java.long.ClassLoader的子类自定义加载class，如Tomcat、jboss会根据j2ee规范实现ClassLoader.
### 双亲委派模型
从虚拟机的角度来讲，只存在两种不同的类加载器：
一种是启动类加载器（Bootstrap ClassLoader），这个是c++实现的，是虚拟机的一部分  
一种是所有其他的类加载器，是java语言实现的，独立于虚拟机外部，并且全都继承自抽象类java.long.ClassLoader。   
从java开发人员角度看，可以划分的更细些：
    - 启动类加载器（Bootstrap CLassLoader）
    - 扩展类加载器（Extension ClassLoader)
    - 应用程序类加载器（Application ClassLoader）
上述的类加载器之间的关系，称为类加载器的双亲委派模型（Parents Delegation Model)
双亲委派模型要求除了顶层的启动类加载器外，其余的类加载器都应当有自己的父类加载器。  
这里的加载器之间的父子关系一般不会以继承关系实现，而是以组合的关系来复用父加载器的代码。


























