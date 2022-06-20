# spring源码

## 核心类

### DefaultListableBeanFactory

XmlBeanFactory继承自DefaultListableBeanFactory ，而DefaultListableBeanFactory 是整个bean 加载的核心部分，是spring注册及加载bean的默认实现。

XmlBeanFactory中使用了自定义的XML读取器XmlBeanDefinitionReader实现了个性化的BeanDefinitionReader读取。

DefaultListableBeanFactory继承AbstractAutowireCapableBeanFactory并实现ConfigurableListableBeanFactory，BeanDefinitionRegistry

容器加载相关类图如下：

![image-20220127165724453](https://gitee.com/zwoou//picgo/raw/master/pic//image-20220127165724453.png)



- **AliasRegistry** 定义对alias的简单增删改等操作。
- **SimpleAliasRegistry** 主要使用ConcurrentHashMap作为alias的缓存，并对接口AliasRegistry 进行实现
- **SingletonBeanRegistry** 定义对单例的注册及获取
- **BeanFactory** 定义获取bean及bean的各种属性
- **DefaultSingletonBeanRegistry** 对接口SingletonBeanRegistry 个函数的实现。
- **HierarchicalBeanFactory** 继承BeanFactory ，在BeanFactory的基础上增加对parentFactory的支持。
- **BeanDefinitionRegistry**  定义对BeanDefinition的增删改操作
- **FactoryBeanRegistrySupport** 在DefaultSingletonBeanRegistry基础上增加对FactoryBean的特殊处理功能。

DefaultListableBeanFactory  主要是对bean注册后的处理。

### XmlBeanDefinitionReader

1. 通过继承自AbstractBeanDefinitionReader中的方法，来使用ResourceLoader讲资源文件路径转换为对应的Resource文件

2. 通过DocumentLoader对Resource文件进行转换为Document文件

3. 通过实现接口BeanDefinitionDocumentReader的DefaultBeanDefinitionDocumentReader类对Document进行解析，并使用BeanDefinitionParserDelegate对Elemnet进行解析。

   

## 容器的基础XmlBeanFactory

分析代码实现

```
BeanFactory bf = new XmlBeanFactory(new ClassPathResource("spring-bean.xml"));

```

内部实现

```
private final XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(this);


/**
 * Create a new XmlBeanFactory with the given resource,
 * which must be parsable using DOM.
 * @param resource the XML resource to load bean definitions from
 * @throws BeansException in case of loading or parsing errors
 */
public XmlBeanFactory(Resource resource) throws BeansException {
   this(resource, null);
}

/**
 * Create a new XmlBeanFactory with the given input stream,
 * which must be parsable using DOM.
 * @param resource the XML resource to load bean definitions from
 * @param parentBeanFactory parent bean factory
 * @throws BeansException in case of loading or parsing errors
 */
public XmlBeanFactory(Resource resource, BeanFactory parentBeanFactory) throws BeansException {
   super(parentBeanFactory);
   this.reader.loadBeanDefinitions(resource);
}
```

首先回调用super(parentBeanFactory); 跟踪代码回调用

```
public AbstractAutowireCapableBeanFactory() {
   super();
   ignoreDependencyInterface(BeanNameAware.class);
   ignoreDependencyInterface(BeanFactoryAware.class);
   ignoreDependencyInterface(BeanClassLoaderAware.class);
   if (NativeDetector.inNativeImage()) {
      this.instantiationStrategy = new SimpleInstantiationStrategy();
   }
   else {
      this.instantiationStrategy = new CglibSubclassingInstantiationStrategy();
   }
}
```

```
ignoreDependencyInterface 会忽略给定接口的自动装配功能
```

## 加载Bean

1. 封装资源文件。当进入XmlBeanDefinitionReader后首先对参数Resource使用EncodeResource进行封装。
2. 获取输入流。从Resource获取对应的InputStream并构造InputSource
3. 通过构造的InputSource实例和Resource实例继续调用函数doLoadBeanDefinitions.

```
protected Document doLoadDocument(InputSource inputSource, Resource resource) throws Exception {
   return this.documentLoader.loadDocument(inputSource, getEntityResolver(), this.errorHandler,
         getValidationModeForResource(resource), isNamespaceAware());
}
```

## 获取XML的验证方式

比较常用的验证方式DTD和XSD

### DTD和XSD的区别

DTD(Document Type Definition) 即文档类型定义。是一种XML约束模式语言。

如spring bean2.0

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE beans PUBLIC "-//Spring//DTD BEAN 2.0//EN" "http://Springframework.org/dtd/Spring-beans-2.0.dtd">
<beans>
</beans>
```

XML Schema语言就是XSD（XML Schemas Definition） .XML Schema 描述了XML文档的结构。

如spring bean 3.0

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="myTestBean" class="com.example.springdemo.bean.MyTestBean"></bean>
</beans>
```

## 解析及注册BeanDefinition

```
public int registerBeanDefinitions(Document doc, Resource resource) throws BeanDefinitionStoreException {
   BeanDefinitionDocumentReader documentReader = createBeanDefinitionDocumentReader();
   int countBefore = getRegistry().getBeanDefinitionCount();
   documentReader.registerBeanDefinitions(doc, createReaderContext(resource));
   return getRegistry().getBeanDefinitionCount() - countBefore;
}
```

## 默认标签解析

```
private void parseDefaultElement(Element ele, BeanDefinitionParserDelegate delegate) {
   if (delegate.nodeNameEquals(ele, IMPORT_ELEMENT)) {
      importBeanDefinitionResource(ele);
   }
   else if (delegate.nodeNameEquals(ele, ALIAS_ELEMENT)) {
      processAliasRegistration(ele);
   }
   else if (delegate.nodeNameEquals(ele, BEAN_ELEMENT)) {
      processBeanDefinition(ele, delegate);
   }
   else if (delegate.nodeNameEquals(ele, NESTED_BEANS_ELEMENT)) {
      // recurse
      doRegisterBeanDefinitions(ele);
   }
}
```

## Bean的加载

1. 转换对应beanName

2. 尝试从缓存中加载单例

3. bean的实例化

4. 原型模式的依赖检查

5. 检测parentBeanFactory

6. 将存储XML配置文件的GernericBeanDefinition转换为RootBeanDefinition

7. 寻找依赖

8. 针对不同的scope进行bean的创建

9. 类型转换

   





