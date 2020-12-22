# springMVC面试

spring的三个阶段

1. 配置阶段

    ​	web.xml  配置DispatcherServlet 是SpringMVC的总入口，用来加载spring的配置文件的。

    ​	配置初始化参数：classpath：application.xml，配置了无数个bean。

2. 初始化阶段

    调用init方法，找到配置文件所在的路径，加载application.xml的内容。扫描相关联的class。

    IOC容器的初始化（一个map，beanId和实例组成的键值对），进行依赖注入DI（如果声明类中定义了成员变量，并且需要赋值，通过依赖注入的三种方式赋值），初始化一个HandlerMapping（MVC使用，将URL和@Controller中的某一方法进行关联，保存到一个Map中）

3. 运行阶段

    service方法：doGet/doPost方法

    调用doDispatch方法：从已初始化的HandlerMappering中寻找到和请求URL相匹配的Method。

    通过反射机制，动态调用刚才获取的Method，拿到Method的返回值，通过Response输出该返回值。

## Spring MVC的工作原理