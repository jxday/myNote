### Unsafe类

#### 简介

Unsafe类是在**sun.misc**包下，不属于Java标准。但是很多Java的基础类库，包括一些被广泛使用的高性能开发库都是基于Unsafe类开发的，比如Netty、Cassandra、Hadoop、Kafka等。Unsafe类在提升Java运行效率，增强Java语言底层操作能力方面起了很大的作用。Unsafe类使Java拥有了像C语言的指针一样操作内存空间的能力，同时也带来了指针的问题。过度的使用Unsafe类会使得出错的几率变大，因此Java官方并不建议使用的，官方文档也几乎没有。

#### unsafe的创建

**通常我们最好也不要使用Unsafe类，除非有明确的目的，并且也要对它有深入的了解才行。**要想使用Unsafe类需要用一些比较tricky的办法。Unsafe类使用了单例模式，需要通过一个静态方法getUnsafe()来获取。但Unsafe类做了限制，如果是普通的调用的话，它会抛出一个SecurityException异常；只有由主类加载器加载的类才能调用这个方法。其源码如下：

```java
public static Unsafe getUnsafe() {
    Class var0 = Reflection.getCallerClass();
    if(!VM.isSystemDomainLoader(var0.getClassLoader())) {
        throw new SecurityException("Unsafe");
    } else {
        return theUnsafe;
    }
	}
```

网上也有一些办法来用主类加载器加载用户代码，比如设置bootclasspath参数。但更简单方法是利用Java反射，方法如下：

```java
Field f = Unsafe.class.getDeclaredField("theUnsafe");
f.setAccessible(true);
Unsafe unsafe = (Unsafe) f.get(null);
```

#### unsafe类的作用

**一、内存管理**	包括分配内存、释放内存等。

**二、非常规的对象实例化。**

**三、操作类、对象、变量。**

**四、数组操作。**

**五、多线程同步**	包括锁机制、CAS操作等。

**六、挂起与恢复。**

**七、内存屏障。**

<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200113111315220.png" alt="image-20200113111315220" style="zoom:50%;" />

### unsafe的方法

####  long allocateMemory(long bytes)

​    申请分配内存

#### long getAddress(long address)

对直接内存进行读

#### void putAddress(long address, long x)

对直接内存进行写





查阅资料：

https://zhuanlan.zhihu.com/p/56772793	Java魔法类：Unsafe应用解析

https://www.iteye.com/blog/bijian1013-2232847	java直接内存读写