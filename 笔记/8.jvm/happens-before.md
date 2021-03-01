## Happens-before及jvm内存模型相关

happens-before原则是判断数据是否存在竞争、线程是否安全的主要依据。

##### 为什么重排序会导致错误？

为了使得处理器内部的运算单元能尽量被充分利用，处理器可能会对输入代码进行乱序执行（Out-Of-Order Execution）优化，处理器会在计算之后将乱序执行的结果重组，保证该结果与顺序执行的结果是一致的，但并不保证程序中各个语句计算的先后顺序与输入代码中的顺序一致，因此，如果存在一个计算任务依赖另外一个计算任务的中间结果，那么其顺序性并不能靠代码的先后顺序来保证。与处理器的乱序执行优化类似，Java虚拟机的即时编译器中也有类似的指令重排序（Instruction Reorder）优化。

##### 对于volatile变量的可见性：

volatile变量在各个线程中是一致的，但并不保证在并发下的运算安全(从微观上来说，是原子性操作，但由于依赖变量的当前值，因此不保证宏观上的运算安全)。

volatile禁止指令重排序优化。

long和double是64位数据结构，读写是非原子性协议，需要使用volatile修饰。

##### java内存模型是如何处理原子性，可见性，有序性？

原子性：lock，synchronized

可见性：volatile，synchronized，final

有序性：volatile，synchronized



##### 先行发生原则：

java内存模型中所有的有序性并非全部依靠volatile和synchronized来完成，因为java语言中有一个happens-before原则。happen-before原则是JMM中非常重要的原则，它是判断数据是否存在竞争、线程是否安全的主要依据，保证了多线程环境下的可见性。

happens-before的定义如下：

1. 如果一个操作happens-before另一个操作，那么第一个操作的执行结果将对第二个操作可见，而且第一个操作的执行顺序排在第二个操作之前。
2. 两个操作之间存在happens-before关系，并不意味着一定要按照happens-before原则制定的顺序来执行。如果重排序之后的执行结果与按照happens-before关系来执行的结果一致，那么这种重排序并不非法。

happens-before规则：

+ 程序次序规则 program order rule：在一个线程内，按照程序代码顺序，书写在前面的操作先行发生于书写在后面的操作。准确的说是控制流程顺序(分支，循环等)。
+ 管程锁定规则 monitor lock rule：一个unlock操作先行发生于后面(时间先后)对同一个锁的lock操作。
+ volatile变量规则 volatile variable rule：对一个volatile变量的写操作先行发生于后面对这个变量的读操作。
+ 线程启动规则 thread start rule：thread对象的start方法先行发生于此线程的每一个动作。
+ 线程终止规则 thread termination rule：线程中的所有操作都先行发生于对此线程的终止检测，可以通过Thread.join方法结束，Thread.isAlive的返回值等手段检测到线程已经终止执行。[ˌtɜːmɪˈneɪʃn]
+ 线程中断规则 thread interruption rule：对线程interrupt方法的调用 先行发生于被中断线程的代码检测到中断事件的发生，可以通过Thread.interrupted方法检测到是否有中断发生。[ˌɪntəˈrʌpʃn]
+ 对象终结规则 finalizer rule：一个对象的初始化完成先行发生于它的finalize方法的开始。[ˈfaɪnəlaɪz]
+ 传递性 transitivity：如果操作a先行发生于操作b，操作b先行发生于操作c，那就可以得出操作a先行发生于操作c的结论。



```java
/**
 * 〈volatile 变量自增测试〉 结论：volatile运算在并发下并不是安全的，因此仍需要加锁
 * 场景1：运算结果并不依赖变量的当前值，或者能够确保只有单一线程修改变量的值
 * 场景2：变量不需要与其他的状态变量共同参与不变约束
 *
 * @author cty
 * @ClassName test20210301
 * @create 3/1/21 3:59 PM
 * @Version 1.0.0
 */
public class test20210301 {

    public static volatile int race = 0;
    private static final int THREADS_COUNT = 20;
    

    public static void main(String[] args) {
        Thread[] threads = new Thread[THREADS_COUNT];
        CountDownLatch countDownLatch = new CountDownLatch(THREADS_COUNT);
        for (int i = 0; i < THREADS_COUNT; i++) {
            threads[i] = new Thread(new Runnable() {
                @Override
                public void run() {
                    for (int i1 = 0; i1 < 10000; i1++) {
                        increase();
                    }
                    countDownLatch.countDown();
                }
            });
            threads[i].start();
        }

        System.out.println("main thread start !");
        
        try {
            countDownLatch.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
            System.err.println("countDownLatch异常");
        }

        //因为使用idea，因此活动线程的数量并不是1，下面代码打印出相关数据
        ThreadGroup group = Thread.currentThread().getThreadGroup();
        System.err.println(Thread.activeCount());
        Thread.currentThread().getThreadGroup().list();
        System.err.println(group);
        System.err.println(group.getName());
        System.err.println(group.getParent());
        
        //Thread类的activeCount()方法用于返回当前线程的线程组中活动线程的数量。
        // 返回的值只是一个估计值，因为当此方法遍历内部数据结构时，线程数可能会动态更改
//        while (Thread.activeCount() > 1){
//            Thread.yield();
//        }
        
        System.out.println("race是：" + race);

    }
    
    public static void increase() {
        race++;
    }
}
```

