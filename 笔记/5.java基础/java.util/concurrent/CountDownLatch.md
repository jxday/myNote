# CountDownLatch

内部通过aqs实现，state表示需唤醒次数。实现的共享锁，可被中断

例：一个主线程A，n个子线程a1,a2...；新建两个计数器，计数器startSignal参数为1，计数器doneSignal参数为n。

在主线程中调用:startSignal.countDown();	doneSignal.await();

子线程的run方法中调用:startSignal.await();	doWork();	doneSignal.countDown();

来完成互相控制。



await():state不是0的时候，进入同步队列。就像一个阀门，运行到这里的时候去看state是否是0。

countDown():是0的时候，释放一个节点。

因为次数是初始化的，所以先countDown还是先state并没区别。