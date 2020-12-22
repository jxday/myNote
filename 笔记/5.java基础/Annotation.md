# Annotation注解

@Target表示注解的目标，ElementType是一个枚举，主要可选值有：❑ TYPE：表示类、接口（包括注解），或者枚举声明；❑ FIELD：字段，包括枚举常量；❑ METHOD：方法；❑ PARAMETER：方法中的参数；❑ CONSTRUCTOR：构造方法；❑ LOCAL_VARIABLE：本地变量；❑ MODULE：模块（Java 9引入的）。

@Retention表示注解信息保留到什么时候，取值只能有一个，类型为RetentionPolicy，它是一个枚举，有三个取值。❑ SOURCE：只在源代码中保留，编译器将代码编译为字节码文件后就会丢掉。❑ CLASS：保留到字节码文件中，但Java虚拟机将class文件加载到内存时不一定会在内存中保留。❑ RUNTIME：一直保留到运行时。

