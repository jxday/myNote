###2.主程序类，主入口类

@SpringBootApplication：SpringBoot应用标注在某各类上，说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用。

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)})
public @interface SpringBootApplication{}
```

@**SpringBootConfiguration**：SpringBoot的配置类：

​	标注在某个类上，表示这是一个SpringBoot的配置类：

​	@**Configuration**：配置类上来标注这个注解

​		配置类----配置文件

​	@**EnableAutoConfiguration**：开启自动配置功能，整个应用没有做任何配置，mvc等都可以使用了。以前需要配置的东西，Spring Boot帮我们自动配置。

```java
@AutoConfigurationPackage
@Import({EnableAutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
```

​	@**AutoConfigurationPackage**:自动配置包

​		@Import({Registrar.class})：Spring 的底层注解，给容器中导入组件。



##### 将一个类声明为Spring的 bean 的注解有哪些?

我们一般使用 `@Autowired` 注解自动装配 bean，要想把类标识成可用于 `@Autowired` 注解自动装配的 bean 的类,采用以下注解可实现：

- `@Component` ：通用的注解，可标注任意类为 `Spring` 组件。如果一个Bean不知道属于哪个层，可以使用`@Component` 注解标注。
- `@Repository` : 对应持久层即 Dao 层，主要用于数据库相关操作。
- `@Service` : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao层。
- `@Controller` : 对应 Spring MVC 控制层，主要用户接受用户请求并调用 Service 层返回数据给前端页面。