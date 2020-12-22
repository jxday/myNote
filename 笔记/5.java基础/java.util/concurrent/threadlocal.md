# ThreadLocal

[ThreadLocal出现OOM内存溢出的场景和原理分析](https://www.cnblogs.com/jobbible/p/13364292.html)







#### InheritableThreadLocal

[demo](https://zhuanlan.zhihu.com/p/101780720)

当我们在主线程start一个子线程时，会new 一个Thread。首先我们调用的是Thread（Runnable target）这个方法。再点进去的init方法中，对InheritableThreadLocal进行判空，if (inheritThreadLocals && parent.inheritableThreadLocals != null)，第一项inheritThreadLocals 是传进来的boolean值，重载时传的是true，所以满足条件。第二项就是判断父线程中的inheritableThreadLocals 是不是空，如果不是空就满足条件。当同时满足if的两个条件后，就执行`this.inheritableThreadLocals = ThreadLocal.createInheritedMap(parent.inheritableThreadLocals);`新创建出来的子线程的inheritableThreadLocals 变量就和父线程的inheritableThreadLocals 的内容一样了。





Thread

​	ThreadLocalMap

​		Node[ThreadLocal,Value]

ThreadLocal

​	ThreadLocalMap

​		Node[ThreadLocal,Value]

Remove()：当前线程的链表中，去除调用该方法的ThreadLocal实例。







