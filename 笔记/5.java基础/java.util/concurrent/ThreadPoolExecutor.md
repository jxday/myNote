# ThreadPoolExecutor

<img src="https://res.weread.qq.com/wrepub/epub_25462418_111">



ThreadPoolExecutor实现的顶层接口是Executor，顶层接口Executor提供了一种思想：将任务提交和任务执行进行解耦。用户无需关注如何创建线程，如何调度线程来执行任务，用户只需提供Runnable对象，将任务的运行逻辑提交到执行器(Executor)中，由Executor框架完成线程的调配和任务的执行部分。ExecutorService接口增加了一些能力：（1）扩充执行任务的能力，补充可以为一个或一批异步任务生成Future的方法；（2）提供了管控线程池的方法，比如停止线程池的运行。

AbstractExecutorService则是上层的抽象类，将执行任务的流程串联了起来，保证下层的实现只需关注一个执行任务的方法即可。最下层的实现类ThreadPoolExecutor实现最复杂的运行部分，ThreadPoolExecutor将会一方面维护自身的生命周期，另一方面同时管理线程和任务，使两者良好的结合从而执行并行任务。







#### 生命周期：在ThreadPoolExecutor线程池的设计中，把整个任务执行框架线程池划分为5个生命周期。

Running：允许接受新的任务，并且处理队列中的任务

shutdown：不再接受新的任务，仅消化完队列中的任务

stop：不仅不再接受新的任务，连队列中的任务都不再处理，并且尝试中断正在执行任务的线程

tidying：所有任务被终止了，工作线程数workCount也被设为0，线程的状态也被设为tidying，并开始调用terminated()

terminated：函数terminated()执行完毕

<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200731171442472.png" alt="image-20200731171442472" style="zoom:50%;" />



### 线程池的创建

```ExecutorService executorService = Executors.newFixedThreadPool(2);```

**newFixedThreadPool**。该方法将用于创建一个固定大小的线程池（此时corePoolSize = maxPoolSize），每提交一个任务就创建一个线程池，直到线程池达到最大数量，线程池的规模在此后不会发生任何变化；

**newCachedThreadPool**。该方法创建了一个可缓存的线程池，（此时corePoolSize = 0，maxPoolSize = Integer.MAX_VALUE），空闲线程超过60秒就会被自动回收，该线程池存在的风险是，如果服务器应用达到请求高峰期时，会不断创建新的线程，直到内存耗尽；

**newSingleThreadExecutor**。该方法创建了一个单线程的线程池，该线程池按照任务在队列中的顺序串行执行（如：**FIFO**、**LIFO**、优先级）；

**newScheduledThreadPool**。该方法创建了一个固定长度的线程池，可以以延迟或者定时的方式执行任务；

### 任务提交/execute的执行过程

任务提交的大概逻辑如下：

1）当线程池小于corePoolSize时，新提交任务将创建一个新线程执行任务，即使此时线程池中存在空闲线程；

2）当线程池达到corePoolSize时，新提交任务将被放入workQueue中，等待线程池中任务调度执行；

3）当workQueue已满，且maximumPoolSize > corePoolSize时，新提交任务会创建新线程执行任务；

4）当提交任务数超过maximumPoolSize时，新提交任务由RejectedExecutionHandler处理；

5）当线程池中超过corePoolSize线程，空闲时间达到keepAliveTime时，关闭空闲线程；

### 任务拒绝

如果线程被提交到线程池时，当前线程池出现以下情况的任一一种情况： 1）线程池任务队列已经满了 2）线程池被关闭了（调用了`shutdown`函数或者`shutdownNow`函数） 都将会调用提前设置好的回调策略，`ThreadPoolExecutor`中总共提供了四种策略：

1）**AbortPolicy（中止）**：该策略将会直接抛出RejectedExecutionException异常，调用者将会获得异常；

2）**DiscardPolicy（抛弃）**：使用该策略，线程池将会悄悄地丢弃这个任务而不被调用者知道；

3）**CallerRunsPolicy（调用者运行）**：该策略既不会抛弃任务也不会抛出异常，而是将这个任务退回给调用者，从而降低新任务的流量；

4）**DiscardOldestPolicy（抛弃最旧的）**：该策略将会抛弃下一个即将轮到执行的任务，那么“抛弃最旧”的将导致抛弃优先级最高的任务，因此最好不要把“抛弃最旧的”饱和策略和优先级队列放在一起使用；

### 线程池销毁

```
ThreadPoolExecutor`提供了两种方法销毁线程池，分别是`shutdown()`和`shutdownNow()
```

`shutdown()`方法仅仅是把线程池的状态置为**SHUTDOWN**，并且拒绝之后尝试提交进来的所有请求，但是已经在任务队列里的任务会仍然会正常消费。

而`shutdownNow()`方法的表现显得更加简单粗暴，它会强行关闭`ExecutorService`，也会尝试取消正在执行的任务，并且返回所有已经提交但尚未开始的任务，开发者可以将这些任务写入日志保存起来以便之后进行处理，另外尝试取消正在执行的任务仅仅是尝试对执行线程进行中断，具体的线程响应中断策略需要用户自己编写。

线程池需要管理线程的生命周期，需要在线程长时间不运行的时候进行回收。线程池使用一张Hash表去持有线程的引用，这样可以通过添加引用、移除引用这样的操作来控制线程的生命周期。这个时候重要的就是如何判断线程是否在运行。

#### addWorker为什么需要持有mainLock？

因为workers是HashMap类型的，不能保证线程安全。

#### Execute方法的执行过程

1. 当`workerCount < corePoolSize`，创建线程执行任务。
2. 当`workerCount >= corePoolSize`&&阻塞队列`workQueue`未满，把新的任务放入阻塞队列。
3. 当`workQueue`已满，并且`workerCount >= corePoolSize`，并且`workerCount < maximumPoolSize`，创建线程执行任务。
4. 当workQueue已满，`workerCount >= maximumPoolSize`，采取拒绝策略,默认拒绝策略是直接抛异常。

#### 在runWorker方法中，为什么要在执行任务的时候对每个工作线程都加锁呢？

shutdown方法与getTask方法存在竞态条件.(这里不做深入，建议自己深入研究，对它比较熟悉的面试官一般会问)

#### runWorker方法的执行过程：

1. while循环中，不断地通过getTask()方法从workerQueue中获取任务
2. 如果线程池正在停止，则中断线程。否则调用3.
3. 调用task.run()执行任务；
4. 如果task为null则跳出循环，执行processWorkerExit()方法，销毁线程`workers.remove(w);`

![image-20200804155658845](/Users/cty/Library/Application Support/typora-user-images/image-20200804155658845.png)

submit方法——》execute方法，判断线程池状态——>addWorker方法——〉判断线程池状态，线程数。新建一个线程并启动——》worker封装run方法-runWorker方法——〉循环执行getTask方法，并执行task.run——》getTask方法，根据策略和线程池状态，不断从同步队列中获取任务，队列满时执行策略。