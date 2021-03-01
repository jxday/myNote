## 垃圾收集器

##### 如何判断对象是否可以被回收：

1、引用计数器法：为每个对象创建一个引用计数，有对象引用时计数器+1，引用被释放时计数-1，当计数器为0时就可以被回收。缺点是不能解决循环引用的问题。2、可达性分析算法：从GCRoots开始向下搜索，搜索所走过的路径称为引用链。当一个对象到GC Roots没有任何引用链相连时，证明对象是可以被回收的。

##### 垃圾回收算法：

1、标记-清除算法：标记无用对象，然后进行清除回收。缺点是效率低，造成垃圾碎片。2、复制算法：按照容量划分两个大小相等的内存区域，当一块用完的时候将活着的对象复制到另一块上，然后再把已使用的内存空间一次清理掉。缺点是内存使用效率只有原来的一半。3、标记-整理算法：标记无用对象，让所有存活的对象都向一端移动，然后清除掉端边界以外的内存。4、分代算法：根据对象存活周期的不同将内存划分为几块，一般是新生代和老年代，新生代基本采用复制算法，老年代采用标记-整理算法。

##### 分代回收器是如何工作的：

分代回收器有两个分区：新生代和老年代，新生代默认的空间占比总空间的1/3，老年代默认占比是2/3。新生代使用复制算法，划分为3个分区：Eden、To Survivor、From Survivor，默认占比8:1:1。回收时将 Eden + From Survivor存活的对象放入 To Survivor区，清空Eden 和 From Survivor，将From Survivor和To Survivor 分区交换。每经历一次回收年龄就+1，默认当年龄到达15时，移动到老年代，大对象也会直接进入老年代，动态对象年龄判定(相同年龄的对象大小总和大于Survivor空间的一半，年龄大于等于该年龄的对象进入老年代)，空间分配担保(和设置相关，检查老年代最大可用连续空间大小判断进行minor gc甚至full gc）。新生代采用空闲指针的方式来控制GC触发，指针保持最后一个分配的对象在新生代区间的位置，当有新的对象要分配内存时，用于检查空间是否足够，不够就触发GC。当连续分配对象时，对象会逐渐从Eden到Survivor，最后到老年代。老年代对象存活时间比较长，采用标记-整理算法进行回收。

##### JVM有哪些垃圾回收器：

<img src="../../../../../Library/Application%20Support/typora-user-images/image-20210218140929183.png" alt="image-20210218140929183" style="zoom:25%;" />

Serial收集器：复制算法，新生代单线程收集器

ParNew收集器：复制算法，新生代并行收集器，是Serial的多线程版本，多核CPU环境下表现更好

Parallel Scavenge收集器：复制算法，高吞吐，高效利用CPU，新生代并行收集器

Serial Old收集器：标记-整理算法，老年代单线程收集器，Serial的老年代版本

Parallel Old收集器：标记-整理算法，老年代并行收集器，吞吐量优先，Parallel Scavenge收集器的老年代版本

CMS收集器：标记-清除算法，老年代并行收集器，具有高并发、地停顿的特点，追求最短GC停顿时间

G1收集器：标记-整理算法，Java堆并行收集器，是JDK7提供的心收集器，回收范围是整个java堆。

当前用的最多的是 CMS和G1。

##### GC的策略：

Young GC：只收集young gen的GC

Old GC：只收集old gen的GC，只有CMS的concurrent collection是这个模式

Mixed GC：收集整个young gen以及部分old gen的GC。只有G1是这个模式

Full GC：收集整个堆，包括young gen，old gen，perm gen等所有部分的模式

新生代采用空闲指针的方式来控制GC触发，指针保持最后一个分配的对象在新生代区间的位置，当有新的对象要分配内存时，用于检查空间是否足够，不够就触发GC。当连续分配对象时，对象会逐渐从Eden到Survivor，最后到老年代。

Full GC：在执行机制上JVM提供了串行GC(Serial MSC)、并行GC(Parallel MSC)和并发标记清理垃圾回收(Concurrent Mark and Sweep GC）。

- 串行GC（Serial MSC）
    client模式下的默认GC方式，可通过-XX:+UseSerialGC强制指定。每次进行全部回收，进行Compact，非常耗费时间。
- 并行GC（Parallel MSC）(吞吐量大，但是GC的时候响应很慢)
    server模式下的默认GC方式，也可用-XX:+UseParallelGC=强制指定。
- 并发GC（CMS）(响应比并行gc快很多，但是牺牲了一定的吞吐量)

如下原因可能导致Full GC。

- 年老代（Tenured）被写满
    当准备要触发一次young GC时，如果发现统计数据说之前young GC的平均晋升大小比目前old gen剩余的空间大，则不会触发young GC而是转为触发full GC（因为HotSpot VM的GC里，除了CMS的concurrent collection之外，其它能收集old gen的GC都会同时收集整个GC堆，包括young gen，所以不需要事先触发一次单独的young GC）。
- 持久代（Perm）被写满
- System.gc()被显示调用
- 上一次GC之后Heap的各域分配策略动态变化

 [关于OopMap、SafePoint(安全点)以及安全区域](https://my.oschina.net/u/1757225/blog/1583822)