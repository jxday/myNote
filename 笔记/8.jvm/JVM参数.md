垃圾回收器

-XX:+PrintGCDetails		打印日志

-XX:UseSerialGC





虚拟机性能监控、故障处理工具

开启JMX管理功能：-Dcom.sun.management.jmxremote





##### JPS：虚拟机进程状况工具

jps [options] [hostid]

jps -l



##### Jstat：虚拟机统计信息监视工具

jstat [ option vmid [interval [s|ms] [count] ]  ]



##### jinfo：java配置信息工具

实时查看和调整虚拟机各项参数。

jinfo [option] pid



##### jmap：java内存映像工具





可视化工具：

##### JConsole





##### HSDB：

```ruby
//启动
$ sudo java -cp ,:/Library/Java/JavaVirtualMachines/jdk1.8.0_211.jdk/Contents/Home/lib/sa-jdi.jar sun.jvm.hotspot.HSDB

//windows-console
//查询内存地址内的clas实例
scanoops 0x00000007bf600000 0x00000007bf900000 com.jxday.testDetail.test20200911$ObjectHolder

//查询引用指针	
revptrs 0x00000007bf716020


```

