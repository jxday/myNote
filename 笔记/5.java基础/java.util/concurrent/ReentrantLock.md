# ReentrantLock

>共享锁/独占锁：
>
>公平锁/非公平锁：
>
>非公平锁：lock方法会先直接cas lock的state，如果失败再调用acquire，再进行完整的锁争夺过程，如果失败进入同步队列。
>
>公平锁：lock方法直接进入acquire，锁争夺的时候先判断hasQueuedPredecessors，是否在同步队列前列。
>
>是否可中断：
>
>是否自旋：