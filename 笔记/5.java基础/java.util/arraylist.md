# Arraylist

> 注释：

```java
概述：
List接口可调整大小的数组实现。实现所有可选的List操作，并允许所有元素，包括null，元素可重复。
除了列表接口外，该类提供了一种方法来操作该数组的大小来存储该列表中的数组的大小。
  
时间复杂度：
方法size、isEmpty、get、set、iterator和listIterator的调用是常数时间的。
添加删除的时间复杂度为 O(N)。其他所有操作也都是线性时间复杂度。
  
容量：
每个ArrayList都有容量，容量大小至少为List元素的长度，默认初始化为10。
容量可以自动增长。
如果提前知道数组元素较多，可以在添加元素前通过调用 ensureCapacity()方法提前增加容量以减小后期容量自动增长的开销。
也可以通过带初始容量的构造器初始化这个容量。
线程不安全：
ArrayList不是线程安全的。
如果需要应用到多线程中，需要在外部做同步
modCount：
定义在AbstractList中：protected transient int modCount = 0;
已从结构上修改此列表的次数。从结构上修改是指更改列表的大小，或者打乱列表，从而使正在进行的迭代产生错误的结果。
此字段由iterator和listiterator方法返回的迭代器和列表迭代器实现使用。
如果意外更改了此字段中的值，则迭代器（或列表迭代器）将抛出concurrentmodificationexception来响应next、remove、previous、set或add操作。
在迭代期间面临并发修改时，它提供了快速失败 行为，而不是非确定性行为。
子类是否使用此字段是可选的。
如果子类希望提供快速失败迭代器（和列表迭代器），则它只需在其 add(int,e)和remove(int)方法（以及它所重写的、导致列表结构上修改的任何其他方法）中增加此字段。
对 add(int, e)或 remove(int)的单个调用向此字段添加的数量不得超过 1，否则迭代器（和列表迭代器）将抛出虚假的 concurrentmodificationexceptions。
如果某个实现不希望提供快速失败迭代器，则可以忽略此字段。
transient：
默认情况下,对象的所有成员变量都将被持久化.在某些情况下,如果你想避免持久化对象的一些成员变量,你可以使用transient关键字来标记他们,transient也是java中的保留字(JDK 1.8)
```



> 常用api

```java
public boolean add(E e) //添加元素到末尾
  public boolean isEmpty() //判断是否为空
  public int size() //获取长度
  public E get(int index) //访问指定位置的元素
  public int indexOf(Object o) //查找元素， 如果找到，返回索引位置，否则返回-1
  public int lastIndexOf(Object o) //从后往前找
  public boolean contains(Object o) //是否包含指定元素，依据是equals方法的返回值
  public E remove(int index) //删除指定位置的元素， 返回值为被删对象
  //删除指定对象，只删除第一个相同的对象，返回值表示是否删除了元素
  //如果o为null，则删除值为null的元素
  public boolean remove(Object o)
  public void clear() //删除所有元素
  //在指定位置插入元素，index为0表示插入最前面，index为ArrayList的长度表示插到最后面
  public void add(int index, E element)
  public E set(int index, E element) //修改指定位置的元素内容
```

##### 如何实现数组和 List 之间的转换？

- 数组转 List：使用 Arrays. asList(array) 进行转换。
- List 转数组：使用 List 自带的 toArray() 方法。



[ArrayList源码和多线程安全问题分析](https://cloud.tencent.com/developer/article/1156905)

ArrayList中包含对共享变量操作的方法同样会有并发安全问题

每次扩容1.5倍或者此次addAll之后的容量，最大容量Integer.MAX



ArrayList的优点如下：

- ArrayList 底层以数组实现，是一种随机访问模式。ArrayList 实现了 RandomAccess 接口，因此查找的时候非常快。
- ArrayList 在顺序添加一个元素的时候非常方便。

ArrayList 的缺点如下：

- 删除元素的时候，需要做一次元素复制操作。如果要复制的元素很多，那么就会比较耗费性能。
- 插入元素的时候，也需要做一次元素复制操作，缺点同上。