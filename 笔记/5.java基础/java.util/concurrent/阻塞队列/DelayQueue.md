## DelayQueue

无界阻塞延迟队列，队列中每个元素都有过期时间，只有过期元素才会出队列

```java
private Thread leader = null;
//指定用于等待队列头部元素的线程，Leader-Follower模式的变形，用于减去不必要的等待时间。每当队列的头替换为具有更早到期时间的元素时，领导者字段就会被重置为null，从而使字段无效，并且会唤醒等待线程。
```





### Leader-Follower线程模型

<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200729134549760.png" alt="image-20200729134549760" style="zoom:50%;" />

（1）线程有3种状态：领导leading，处理processing，追随following
（2）假设共N个线程，其中只有1个leading线程（等待任务），x个processing线程（处理），余下有N-1-x个following线程（空闲）
（3）有一把锁，谁抢到就是leading
（4）事件/任务来到时，leading线程会对其进行处理，从而转化为processing状态，**处理完成之后，又转变为following**
（5）丢失leading后，following会尝试抢锁，抢到则变为leading，否则保持following
（6）following不干事，就是抢锁，力图成为leading

优点：**不需要消息队列**

适用场景：**线程能够很快的完成工作任务**

