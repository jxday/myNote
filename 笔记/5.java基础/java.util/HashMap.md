# HashMap

实际是由数组+链表/红黑树构成，内部维护了一个

```java
/**
 * 哈希桶数组，分配的时候，table的长度总是2的幂
 */
transient Node<K, V>[] table;
```

其余重要参数：

```java
/**
 * HashMap将数据转换成set的另一种存储形式，这个变量主要用于迭代功能
 */
transient Set<Map.Entry<K, V>> entrySet;

/**
 * 实际存储的数量，则HashMap的size()方法，实际返回的就是这个值，isEmpty()也是判断该值是否为0
 */
transient int size;

/**
 * hashmap结构被改变的次数，fail-fast机制
 */
transient int modCount;

/**
 * HashMap的扩容阈值，在HashMap中存储的Node键值对超过这个数量时，自动扩容容量为原来的二倍
 *
 * @serial
 */
int threshold;

/**
 * HashMap的负加载因子，可计算出当前table长度下的扩容阈值：threshold = loadFactor * table.length
 *
 * @serial
 */
final float loadFactor;
```