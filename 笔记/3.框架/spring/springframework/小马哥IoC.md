# IoC

## 2.内容综述(可概括复习)

### 编程模型：

+ 面向对象编程：契约接口
+ 面向切面编程：
    + 动态代理
    + 字节码提升

+ 面向元编程
    + 配置元信息：placeholder
    + 注解
    + 属性配置
+ 面向模块编程
    + maven artifacts：
    + OSGI Bundles
    + java9 Automatic Modules：自动化模块
    + Spring @Enable 注解
+ 面向函数编程
    + Lambda
    + Reactive：异步非阻塞

<img src="../../../../../../Library/Application%20Support/typora-user-images/image-20210122135451864.png" alt="image-20210122135451864" style="zoom:50%;" />

<img src="../../../../../../Library/Application%20Support/typora-user-images/image-20210122114156411.png" alt="image-20210122114156411" style="zoom:50%;" />

### 3.准备

心态：戒骄戒躁，谨慎豁达，如履薄冰

方法：

+ 基础：夯实基础，了解动态
+ 思考：保持怀疑，验证一切
+ 分析：不拘小节，观其大意
+ 实践：思辨结合，学以致用

工具：JDK：Oracle JDK8	SpringFramework:5.2.2.RELEASE 	Maven:3.2 +

## 4.特性总览

### 1.核心特性：

+ IoC容器
+ Spring事件（Events）
+ 资源管理（Resource）
+ 国际化（i18n）
+ 校验（Validation）
+ 数据绑定（Data Binding）
+ 类型转换（Type Conversion）
+ SPring表达式（Spring Express Language）
+ 面向切面编程（AOP）

### 2.数据存储：

+ JDBC
+ 事务抽象（Tramsactions）
+ DAO支持（DAO Support）
+ O/R映射（O/R Mapping）：JPA
+ XML编列（XML Marshalling）：JAVA序列化，

### 3.WEB技术

+ WEB Servlet 技术栈
    + spring MVC
    + WebSocket
    + SockJS
+ Web Reactive 技术栈，在spring5之后引入
    + Spring WebFlux
    + WebClient
    + WebSocket

### 4.技术整合

+ 远程调用（Remoting）：RMI，java标准的远程方法调用；Hessian：社区开源方案，例如dubbo就是基于Hessian协议。通常远程调用都是同步的服务。
+ Java消息服务（JMS）：java的异步调用。比如ActiveMQ。不包含kafka，RocketMQ的实现。
+ JAVA连接架构（JCA）：java EE的架构，统一资源链接，例如JDBC，现在使用较少
+ Java管理拓展（JMX）：用于java管理，cup利用率等。
+ java邮件客户端（Email）：spring封装标准客户端
+ 本地任务（Tasks）：
+ 本地调度（Scheduling）：
+ 缓存抽象（Caching）：封装了注解来简化操作
+ Spring测试（Testing）：
    + 模拟对象（mock objects）：
    + testcontext 框架：
    + spring mvc 测试：
    + web测试客户端：

## 5.版本特性：各个版本引进了什么新特性？

<img src="../../../../../../Library/Application%20Support/typora-user-images/image-20210122142601362.png" alt="image-20210122142601362" style="zoom:50%;" />

java1.3引入了动态代理，从此开始针对接口进行动态代理，这是实现AOP的重要环节。JAVAEE 2.3版本支持事件，呼应spring的事件。Java 1.4.2更新了NIO。spring3引入了大量注解，java 5 提升到注解和枚举。spring3基本确定了framework的内核：注解驱动，事件驱动，AOP支持的完善。spring boot 1版本基于spring 4进行开发，spring boot 2基于spring 5。

## 6. spring如何在不同模块中组织

spring在某个版本将模块划分成jar包，可以更细粒度的按需分配进行依次依赖，大概将近20个模块。 

<img src="../../../../../../Library/Application%20Support/typora-user-images/image-20210122143511385.png" alt="image-20210122143511385" style="zoom:50%;" />

## 7. spring各版本运用java语法特性

<img src="../../../../../../Library/Application%20Support/typora-user-images/image-20210122164239569.png" alt="image-20210122164239569" style="zoom:50%;" />

## 8.spring如何取舍i/o，集合 ，反射，动态代理等API

<img src="../../../../../../Library/Application%20Support/typora-user-images/image-20210122230144198.png" alt="image-20210122230144198" style="zoom:50%;" />

## 9.spring 为什么要整合java EE

## 10.spring实现了那些编程模型

ApplicationEvent基于java标准的事件，是一个观察者模式的拓展，继承了EventObject类。

CompositeCacheManager是组合模式的实现，它实现了CacheManager，同时组合了更多的成员，用在多个缓存进行合并，按照一个缓存方式进行处理。

配置：Environment抽象、PropertySources、BeanDefinition等。

一个Environment对应多个PropertySources。

## 11.spring Framework 的经验和教训

+ 生态系统，都是围绕framework进行拓展的
    + spring boot
    + spring cloud
    + spring security
    + spring data
    + 其他
+ API抽象设计
    + AOP抽象
    + 事务抽象
    + Environment抽象
    + 生命周期
+ 编程模型：见第二章编程模型
+ 设计思想
    + Object-Oriented Programming	OOP
    + IoC / DI.    反转控制，依赖注入
    + Domain-Driven Development     DDD
    + Test-Driven Development     TDD
    + Event-Driven Development     EDP
    + Functional Programming     FP
+ 设计模式
    + 专属模式 
        + 前缀模式
            + Enable模式
            + Configurable模式
        + 后缀模式
            + 处理器模式：Processor，Resolver，Handler
            + 意识模式：Aware
            + 配置器模式：Configuror
            + 选择器模式：org.springframework.context.annotation.ImportSelector
    + GoF23 
+ 用户基础 

## 12.总结

## 13.IoC发展简介

> [[Spring 系列] 重温 IOC 设计理念](https://juejin.cn/post/6844904083384451080#heading-0)

## 14.IoC与DI

IoC通过两种主要方式去实现：1.依赖查询。2.依赖注入。

## 15.IoC除了依赖注入，还涵盖哪些职责？

+ 依赖处理
    + 依赖查找
    + 依赖注入
+ 生命周期管理
    + 容器
    + 托管的资源(java beans 或其他资源    (不仅仅是 Bean，还有一些非 Bean，例如说配置资源例如说在 IOC 发生事件的时候外部添加进去的，又或者是 Spring 重的事件监听，这些无法通过依赖注入或查找来进行管理的资源).  )
+ 配置
    + 容器
    + 外部化配置
    + 托管的资源(Java beans 或其他资源)

## 16.IoC容器是开源框架的专利吗

主要实现：

+ Java SE
    + Java Beans
    + Java ServoceLoader SPI
    + JNDI
+ Java EE
    + EJB
    + Servlet
+ 开源
    + Apache Avalon
    + PicoContainer
    + Google Guice
    + Spring Framework

## 17.JavaBeans 也是IoC容器嘛

> Code:BeanInfoDemo.class

Java Beans 作为IoC容器：

+ 特性：
    + 依赖查找
    + 生命周期管理
    + 配置元信息：Property，Descriptor，Event
    + 事件：
    + 自定义
    + 资源管理
    + 持久化
+ 规范
    + JavaBeans：
    + BeanContext：

## 18.轻量级IoC容器：如何界定容器的轻重

轻量级容器的特征：

+ 能够管理应用代码的运行，启停
+ 实现快速启动
+ 容器不需要一些特殊的配置(EJB)
+ 能够达到最小的内存占用和最小化的API依赖
+ 拥有一个渠道去管理容器细粒度的对象和粗粒度的组件

## 19.依赖查找/依赖注入

<img src="../../../../../../Library/Application%20Support/typora-user-images/image-20210126163651525.png" alt="image-20210126163651525" style="zoom:50%;" />

## 20.构造器注入/Setter注入

+ 构造器的参数的有序的，setter注入却没有顺序。

## 21.总结

什么是IoC：IoC是控制反转，主要有依赖查找和依赖注入。

依赖查找和依赖注入的区别：依赖查找是主动或手动的依赖查找的方式，通常需要依赖容器或标准API实现。而依赖注入则是手动或自动依赖绑定的方式，无需依赖特定的容器和API。

Spring作为IoC容器有什么优势：spring拥有典型的IoC管理，依赖查找和依赖注入。AOP抽象。事务抽象。事件机制。SPI扩张。强大的第三方整合。易测试性。更好的面向对象。

## 22.依赖查找

> org.geekbang.thinkinspring.ioc.overview.dependencylookup.DependencyLookUpDemo
>
> https://time.geekbang.org/comment/nice/184050
>
> ObjectFactory 通常是针对单类 Bean 做延迟获取的，BeanFactory 则是全局 Bean 管理的容器。
>
> ObjectFactoryCreatingFactoryBean 是 ObjectFactory 和 FactoryBean 组合形式，通过 FactoryBean 注册 ObjectFactory

+ 根据Bean名称查找
    + 实时查找
    + 延迟查找：比如在 BeanFactoryPostProcessor 接口中获取 ConfigurableListableBeanFactory 时，不马上获取，降低 Bean 过早初始化的情况
+ 根据Bean类型查找
    + 单个Bean对象
    + 集合Bean对象
+ 根据Bean名称+类型查找
+ 根据java注解查找
    + 单个bean对象
    + 集合bean对象

## 23.依赖注入

> org.geekbang.thinkinspring.ioc.overview.dependencyInject.DependenctInjectDemo

+ 根据bean名称注入
+ 根据bean类型注入
    + 单个bean对象
    + 集合bean对象
+ 注入容器内建bean对象
+ 注入非bean对象
+ 注入类型
    + 实时注入
    + 延迟注入

## 24.依赖注入和依赖查找的对象来源是哪里？

> https://blog.csdn.net/u013837825/article/details/106278285

+ 自定义bean：也就是我们自己定义的 Bean
+ 容器内建bean对象：指的是框架运行时内部必要的 Bean 例如 BeanFactory，不能通过依赖查找查询到。
+ 容器内建依赖： Environment 接口[一个具有环境参数性质的接口]，容器内建 Bean 对象是依赖于它的。

1.内建的 Bean 是普通的 Spring Bean，包括 BeanDefinitions 和 Singleton Objects，而内建依赖则是通过 AutowireCapableBeanFactory 中的 resolveDependency 方法来注册，这并非是一个 Spring Bean，无法通过依赖查找获取。

## 25.springIoC配置元信息有哪些方式

| 来源方向       | 来源途径                                    |
| -------------- | ------------------------------------------- |
| Bean 定义配置  | 基于 XML / Properties / Java 注解/ Java API |
| IOC 容器配置   | 基于 XML / Java 注解 / Java API             |
| 外部化属性配置 | 基于 Java 注解（@Value，属于元编程）        |

## 26. BeanFactory和Application谁才是Spring IoC容器

```java
//ClassPathXmlApplicationContext    ->      AbstractApplicationContext     ->|   ConfigurableApplicationContext     ->  BeanFactory
//ConfigurableApplicationContext#getBeanFactory()   返回的就是DefaultListableBeanFactory，依赖注入的BeanFactory
//AbstractApplicationContext#getBean()   return getBeanFactory().getBean();
```

AbstractApplicationContext实现了ConfigurableApplicationContext接口。

AbstractApplicationContext.getBean()调用了ConfigurableApplicationContext.getBeanFactory().getBean().

ConfigurableApplicationContext.getBeanFactory()返回的就是DefaultListableBeanFactory。也就是依赖注入的BeanFactory对象。

## 27.ApplicationContext除了IoC容器还有哪些特征

ApplicationContext除了IoC容器角色，还提供：

+ 面向切面AOP
+ 配置元信息Configuration Metadata
+ 资源管理 Resources
+ 事件Events
+ 国际化i18n
+ 注解 Annotations
+ Environment 抽象

## 28.选BeanFactory还是ApplicationContext

> org.geekbang.thinkinspring.ioc.overview.IoCContainer

+ BeanFactory是Sprong 底层IoC容器
+ ApplicationContext是具备应用特性的BeanFactory超集 

## 29.IoC容器启停过程发生了什么

+ 启动：AnnotationConfigApplicationContext.refresh方法-obtainFreshBeanFactory方法-AbstractRefreshableApplicationContext.refreshBeanFactory方法-DefaultListableBeanFactory beanFactory = createBeanFactory()
+ 运行：
+ 停止：

## 30.总结

什么是IoC容器：spring框架是实现控制反转的原则，IoC以依赖注入为人所知。

BeanFactory和FactoryBean的区别：BeanFactory是IoC的底层容器。FactoryBean是创建Bean的一种方式，帮助实现复杂的初始化逻辑。 

Spring IoC容器启动时做了哪些准备：IoC配置元信息读取和解析、IoC容器生命周期、Spring事件发布、国际化等。

## 31.什么是BeanDefinition

> spring bean的生命周期

+ 定义Spring Bean
+ BeanDefinition元信息
+ 命名Spring Bean
+ Spring Bean的别名
+ 注册Spring Bean
+ 实例化Spring Bean
+ 初始化Spring Bean
+ 延迟初始化Spring Bean
+ 销毁Spring Bean
+ 垃圾回收Spring Bean

什么是BeanDefinition：BeanDefinition是Spring Framework中定义Bean的配置元信息接口，包括：

+ Bean的类名：全限定类名，必须是一个具体的实现类
+ Bean行为配置元素，如作用域、自动绑定的模式、生命周期回调等：autowiring，初始化/销毁回调
+ 其他Bean引用，又可称作合作者(Collaborators)或者依赖(Dependencies)：Bean的引用关系/依赖，比如依赖注入
+ 配置设置，比如Bean属性(Properties)

## 32.除了Bean的名称和类名，还有哪些Bean元信息值得关注

| 属性Property             | 说明                                         |
| ------------------------ | -------------------------------------------- |
| Class                    | Bean全类名，必须是具体类，不能用接口或抽象类 |
| Name                     | Bean的名称或者ID                             |
| Scope                    | Bean的作用域(如：singleton、prototype等)     |
| Constructor arguments    | Bean构造器参数(用于依赖注入)                 |
| Properties               | Bean属性设置(用于依赖注入)                   |
| Autowiring mode          | Bean自动绑定模式(如：通过名称byName)         |
| Lazy initialization mode | Bean延迟初始化模式(延迟和非延迟)             |
| Initialization mode      | Bean初始化回调方法名称                       |
| Destruction mode         | Bean销毁回调方法名称                         |

BeanDefinition构建：

1.通过BeanDefinitionBuilder

2.通过AbstractBeanDefinition以及派生类

BeanDefinitionBuilder是建造者，通过genericBeanDefinition方法实例化，通过getBeanDefinition返回一个AbstractBeanDefinition（实际是GenericBeanDefinition）。

BeanDefinition是接口，AbstractBeanDefinition是抽象类实现BeanDefinition接口，GenericBeanDefinition是AbstractBeanDefinition的子类。这其中拥有一系列关于生命周期，property，construct等与Bean在spring生命周期内定义相关的方法。

## 33.id和name属性命名bean

+ Bean的名称

每个Bean拥有一个或多个识别符(identifiers)，这些标识符在Bean所在的容器必须是唯一的。通常，一个Bean仅有一个标识符，如果需要额外的，可考虑使用别名(Alias)来扩充。

在基于xml的配置元信息中，开发人员可用id或者name属性来规定bean的标识符。通常bean的标识符由字母组成，允许出现特殊字符。如果想引入bean的别名的话，可在name属性使用半角逗号(,)或者分号(;)来间隔。

Bean的id或name属性并非必须制定，如果留空的话，容器会为Bean自动生成一个唯一的名称。bean的命名尽管没有限制，不过官方建议采用驼峰的方式，更符合java命名约定。

接口定义：BeanNameGenerator：

两种方式：spring默认，id/name标签命名。

## 34.为什么命名Bean还需要别名

+ 复用现有的BeanDefinition
+ 更具场景化的命名方法

```xml
	<!-- 依赖查找通过id/name和alias获取的对象是相同的	-->
<alias name = "name" alias = "name-alias"/>
```

## 35.将BeanDefinition注册到IoC容器

> org.springframework.context.annotation.AnnotationConfigApplicationContext

+ BeanDefinition注册
    + xml配置元信息
        + <bean name = "..." ... />
+ Java注解配置元信息
    + @Bean
    + @Component
    + @Import
+ Java API 配置元信息
    + 命名方式：BeanDefinitionRegistry#registerBeanDefinition(String,BeanDefinition)
    + 非命名方式：BeanDefinitionReaderUtils#registerWithGeneratedName(AbstractBeanDefinition,BeanDefinitionRegistry)
    + 配置类方式：AnnotatedBeanDefinitionReader#register(Class...)

##  36.实例化 Spring Bean

+ Bean 实例化 Instantiation
    + 常规方法
        + 通过构造器（配置元信息：xml、java注解和java api）
        + 通过静态工厂方法（配置元信息：xml和java api）
        + 通过bean工厂方法（配置元信息：xml和java api）
        + 通过FactoryBean（配置元信息：xml、java注解和java api）
    + 特殊方法
        + 通过ServiceLoaderFactoryBean（配置元信息：xml、java注解和java api）
        + 通过AutowireCapableBeanFactory#createBean(java.lang.Class,int,boolean)
        + 通过BeanDefinitionRegistry#registerBeanDefinition(String,BeanDefinition)















