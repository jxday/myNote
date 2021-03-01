

# 类文件结构,方法区与常量池

[方法区与常量池](https://blog.csdn.net/wangbiao007/article/details/78545189)

虚拟机规范未规定 Class 对象的存储位置，对于 HotSpot 虚拟机而言，Class 对象比较特殊，它虽然是对象，但存放在方法区中。

<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200826142626538.png" alt="image-20200826142626538" style="zoom:50%;" />



### [字节码文件结构](https://www.cnblogs.com/chanshuyi/p/jvm_serial_05_jvm_bytecode_analysis.html)

<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200903145140898.png" alt="image-20200903145140898" style="zoom:50%;" />

class文件的魔数 1-4 字节是cafe babe

版本号是java 发行版本的编号





### [jvm类加载机制](https://www.cnblogs.com/chanshuyi/p/jvm_serial_07_jvm_class_loader_mechanism.html)

> 加载-验证-准备-解析-初始化-使用-卸载

#### 加载

加载阶段是类加载过程的第一个阶段。在这个阶段，JVM 的主要目的是将字节码从各个位置（网络、磁盘等）转化为二进制字节流加载到内存中，接着会为这个类在 JVM 的方法区创建一个对应的 Class 对象，这个 Class 对象就是这个类各种数据的访问入口。

将字节码文件读取到内存(方法区)

#### 验证

验证字节码文件：jvm规范校验，代码逻辑校验

#### 准备

分配内存对象：为类对象分配内存并初始化。

static修饰的变量初始化为初始值，比如0，null等

static final 修饰的变量初始化为希望被赋予的值。

#### 解析

当通过准备阶段之后，JVM 针对类或接口、字段、类方法、接口方法、方法类型、方法句柄和调用点限定符 7 类引用进行解析。这个阶段的主要任务是将其在常量池中的符号引用替换成直接其在内存中的直接引用。

#### 初始化

![image-20200903160606805](/Users/cty/Library/Application Support/typora-user-images/image-20200903160606805.png)	

**对于静态字段，只有直接定义这个字段的类才会被初始化（执行静态代码块）。**因此通过其子类来引用父类中定义的静态字段，只会触发父类的初始化而不会触发子类的初始化。

![image-20200903161455936](/Users/cty/Library/Application Support/typora-user-images/image-20200903161455936.png)



### 垃圾回收机制

> 标记清除算法，复制算法，标记整理算法。

![image-20200903162136752](/Users/cty/Library/Application Support/typora-user-images/image-20200903162136752.png)



java文件通过编译器变成了.class文件，接下来类加载器又将这些.class文件加载到JVM中。
其实可以一句话来解释：类的加载指的是将类的.class文件中的二进制数据读入到内存中，将其放在运行时数据区的方法区内，然后在堆区创建一个 java.lang.Class对象，用来封装类在方法区内的数据结构。