zab协议是为分布式协调服务zookeeper专门设计的一种支持崩溃恢复的一致性协议。基于该协议，zookeeper实现了一种主从模式的系统架构来保持集群中各个副本之间的数据一致性。



选主流程：

1. 候选节点A初始化自身的zxid和epoch：updateProposal()；
2. 向其他所有节点发送选主通知：sendNotifications()；
3. 等待其他节点的回复：recvqueue.poll()；
4. 如果来自B节点的回复不为空，且B是一个有效节点，判断B此时的运行状态是LOOKING（也在发起选主）还是LEADING/FOLLOWING（正常请求处理过程）