## springIoC依赖来源

https://blog.csdn.net/u013837825/article/details/106278285

有三个来源：

+ 自定义bean：也就是我们自己定义的 Bean
+ 容器内建bean对象：指的是框架运行时内部必要的 Bean 例如 BeanFactory，不能通过依赖查找查询到。
+ 容器内建依赖： Environment 接口[一个具有环境参数性质的接口]，容器内建 Bean 对象是依赖于它的。



