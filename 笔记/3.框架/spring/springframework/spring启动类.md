+ AnnotationConfigApplicationContext

    + ```java
        AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext();
        //加载配置  将当前类作为配置类
        applicationContext.register(AnnotationApplicationContextAsIocContainerDemo.class);
        ```

    + ```java
        //四种构造方法
        /**
         * Create a new AnnotationConfigApplicationContext that needs to be populated
         * through {@link #register} calls and then manually {@linkplain #refresh refreshed}.
         */
        public AnnotationConfigApplicationContext() {
           this.reader = new AnnotatedBeanDefinitionReader(this);
           this.scanner = new ClassPathBeanDefinitionScanner(this);
        }
        
        /**
         * Create a new AnnotationConfigApplicationContext with the given DefaultListableBeanFactory.
         * @param beanFactory the DefaultListableBeanFactory instance to use for this context
         */
        public AnnotationConfigApplicationContext(DefaultListableBeanFactory beanFactory) {
           super(beanFactory);
           this.reader = new AnnotatedBeanDefinitionReader(this);
           this.scanner = new ClassPathBeanDefinitionScanner(this);
        }
        
        /**
         * Create a new AnnotationConfigApplicationContext, deriving bean definitions
         * from the given component classes and automatically refreshing the context.
         * @param componentClasses one or more component classes &mdash; for example,
         * {@link Configuration @Configuration} classes
         */
        public AnnotationConfigApplicationContext(Class<?>... componentClasses) {
           this();
           register(componentClasses);
           refresh();
        }
        
        /**
         * Create a new AnnotationConfigApplicationContext, scanning for components
         * in the given packages, registering bean definitions for those components,
         * and automatically refreshing the context.
         * @param basePackages the packages to scan for component classes
         */
        public AnnotationConfigApplicationContext(String... basePackages) {
           this();
           scan(basePackages);
           refresh();
        }
        ```

+ ClassPathXmlApplicationContext

    + ```java
        BeanFactory beanFactory = new ClassPathXmlApplicationContext("classpath:/META-INF/dependency.xml");
        ```

+ DefaultListableBeanFactory

    + ```java
        DefaultListableBeanFactory beanFactory = new DefaultListableBeanFactory();
        XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(beanFactory);
        //加载配置
        int definitions = reader.loadBeanDefinitions("classpath:/META-INF/dependency.xml");
        ```