# AbstractQueuedSynchronizer(AQS)

>在同步组件的实现中，AQS是核心部分，同步组件的实现者通过使用AQS提供的模板方法实现同步组件语义，AQS则实现了对**同步状态的管理，以及对阻塞线程进行排队，等待通知**等等一些底层的实现处理。AQS的核心也包括了这些方面:**同步队列，独占式锁的获取和释放，共享锁的获取和释放以及可中断锁，超时等待锁获取这些特性的实现**

**是一个用于构建锁和同步容器的框架**。事实上concurrent包内许多类都是基于AQS构建，**例如ReentrantLock**，Semaphore，CountDownLatch，ReentrantReadWriteLock，**FutureTask**等。AQS解决了在实现同步容器时设计的大量细节问题。

AQS维护了一个volatile int state（代表共享资源）和一个FIFO线程等待队列（多线程争用资源被阻塞时会进入此队列）

**AQS使用一个FIFO的队列表示排队等待锁的线程，队列头节点称作“哨兵节点”或者“哑节点”，它不与任何线程关联。其他的节点与等待线程关联，每个节点维护一个等待状态waitStatus。**

**AQS中还有一个表示状态的字段state，例如ReentrantLocky用它表示线程重入锁的次数，Semaphore用它表示剩余的许可数量，FutureTask用它表示任务的状态。对state变量值的更新都采用CAS操作保证更新操作的原子性**。



![image-20200710105143820](/Users/cty/Library/Application Support/typora-user-images/image-20200710105143820.png)

![image-20200710105157691](/Users/cty/Library/Application Support/typora-user-images/image-20200710105157691.png)



### 同步队列

AQS中的同步队列则是**通过链式方式**进行实现。同步队列是一个双向队列，aqs通过持有头尾指针管理同步队列。

aqs内部有一个静态内部类Node，用来实现同步队列。

内部实现尾插法。



#### Node节点：![image-20200710140906885](/Users/cty/Library/Application Support/typora-user-images/image-20200710140906885.png)

<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200717133317929.png" alt="image-20200717133317929" style="zoom:50%;" />

<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200717133615434.png" alt="image-20200717133615434" style="zoom:50%;" />

#### 独占锁

**在获取同步状态时，AQS维护一个同步队列，获取同步状态失败的线程会加入到队列中进行自旋；移除队列（或停止自旋）的条件是前驱节点是头结点并且成功获得了同步状态。在释放同步状态时，同步器会调用unparkSuccessor()方法唤醒后继节点。**



#### 可中断式获取锁（acquireInterruptibly方法）





## condition

Condition的Node，只有**CONDITION/CANCELLED**两种状态。















>[【深入AQS原理】我画了35张图就是为了让你深入 AQS](https://cloud.tencent.com/developer/article/1624354)
>
>[Condition的await/signal源码实现简析](https://juejin.im/post/5def445af265da33c4280639#heading-4)
>
>[AQS源码分析及核心方法解析](https://juejin.im/post/5dea57f3518825122c4c9ba2#heading-43)

