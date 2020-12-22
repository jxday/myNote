# SynchronousQueue

`ynchronousQueue` 支持公平访问队列，根据构造函数的参数不同，有两种实现方式：`TransferQueue` 和 `TransferStack`，默认情况下是 false（TransferStack）：

`SynchronousQueue` 是一个**不存储元素**的阻塞队列。这里的“不存储元素”指的是，`SynchronousQueue` 容量为 0，每添加一个元素必须等待被取走后才能继续添加元素。

