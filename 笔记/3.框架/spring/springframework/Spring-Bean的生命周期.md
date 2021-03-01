[【Spring】Bean的生命周期](https://yemengying.com/2016/07/14/spring-bean-life-cycle/)

[[Spring Bean的生命周期（非常详细）](https://www.cnblogs.com/zrtqsk/p/3735273.html)]



```java
// Prepare this context for refreshing.
prepareRefresh();

// Tell the subclass to refresh the internal bean factory.
ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

//加上内建bean对象，bean依赖和内建的非bean依赖 
// Prepare the bean factory for use in this context.
prepareBeanFactory(beanFactory);

  //自定义扩展，对容器的调整
   // Allows post-processing of the bean factory in context subclasses.
   postProcessBeanFactory(beanFactory);

   // Invoke factory processors registered as beans in the context.
   invokeBeanFactoryPostProcessors(beanFactory);

  //注册用户控制的BeanProcessor，对bean的调整
   // Register bean processors that intercept bean creation.
   registerBeanPostProcessors(beanFactory);

	//国际化	Application与Beanfactory不同的体现
   // Initialize message source for this context.
   initMessageSource();

	//应用事件广播/初始化事件的特性
   // Initialize event multicaster for this context.
   initApplicationEventMulticaster();

   // Initialize other special beans in specific context subclasses.
   onRefresh();

   // Check for listener beans and register them.
   registerListeners();

   // Instantiate all remaining (non-lazy-init) singletons.
   finishBeanFactoryInitialization(beanFactory);

   // Last step: publish corresponding event.
   finishRefresh();
```