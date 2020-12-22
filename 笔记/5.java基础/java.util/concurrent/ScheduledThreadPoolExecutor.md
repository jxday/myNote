## ScheduledThreadPoolExecutor

ScheduledThreadPoolExecutor的实现，这是一个可以在指定一定延迟时间后或者定时进行任务调度执行的线程池。

ScheduledThreadPoolExecutor继承了ThreadPoolExecutor并实现了ScheduledExecutorService接口。线程池队列是DelayedWorkQueue，其和DelayedQueue类似，是一个延迟队列。ScheduledFutureTask是具有返回值的任务，继承自FutureTask。FutureTask的内部有一个变量state用来表示任务的状态，一开始状态为NEW。

<img src = "https://res.weread.qq.com/wrepub/epub_25462418_114">

ScheduledFutureTask内部还有一个变量period用来表示任务的类型，任务类型如下：● period=0，说明当前任务是一次性的，执行完毕后就退出了。● period为负数，说明当前任务为fixed-delay任务，是`固定延迟`的定时可重复执行任务。● period为正数，说明当前任务为fixed-rate任务，是`固定频率`的定时可重复执行任务。

ScheduledThreadPoolExecutor的一个构造函数如下，由该构造函数可知线程池队列是DelayedWorkQueue。

● schedule（Runnable command, long delay, TimeUnit unit）

一次延迟的定时任务

● scheduleWithFixedDelay（Runnable command, long initialDelay, long delay, TimeUnitunit）

让其延迟固定时间后再次运行（fixed-delay任务）。其中initialDelay表示提交任务后延迟多少时间开始执行任务command, delay表示当任务执行完毕后延长多少时间后再次运行command任务，unit是initialDelay和delay的时间单位。任务会一直重复运行直到任务运行中抛出了异常，被取消了，或者关闭了线程池。

● scheduleAtFixedRate（Runnable command, long initialDelay, long period, TimeUnitunit）

该方法相对起始时间点以固定频率调用指定的任务（fixed-rate任务）。当把任务提交到线程池并延迟initialDelay时间（时间单位为unit）后开始执行任务command。然后从initialDelay+period时间点再次执行，而后在initialDelay + 2 * period时间点再次执行，循环往复，直到抛出异常或者调用了任务的cancel方法取消了任务，或者关闭了线程池。当前任务执行完毕后，调用setNextRunTime设置任务下次执行的时间时执行的是time += p而不再是time = triggerTime（-p）

差别：相对于fixed-delay任务来说，fixed-rate方式执行规则为，时间为initdelday +n*period时启动任务，但是如果当前任务还没有执行完，下一次要执行任务的时间到了，则不会并发执行，下次要执行的任务会延迟执行，要等到当前任务执行完毕后再执行。

