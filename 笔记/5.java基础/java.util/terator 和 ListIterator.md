## Iterator

**forEachRemaining(Consumer<? super E> action)**：为每个剩余元素执行给定的操作,直到所有的元素都已经被处理或行动将抛出一个异常

**hasNext()**：如果迭代器中还有元素，则返回true。

**next()**：返回迭代器中的下一个元素

**remove()**：删除迭代器新返回的元素。

（1）Iterator只能单向移动。

（2）Iterator.remove()是唯一安全的方式来在迭代过程中修改集合；如果在迭代过程中以任何其它的方式修改了基本集合将会产生未知的行为。而且每调用一次next()方法，remove()方法只能被调用一次，如果违反这个规则将抛出一个异常。



## ListIterator

add方法：在当前位置后面新增一个值/inserts the specified element into the list

```java
public void add(E e) {
    checkForComodification();

    try {
        int i = cursor;
        ArrayList.this.add(i, e);
        cursor = i + 1;
        lastRet = -1;
        expectedModCount = modCount;
    } catch (IndexOutOfBoundsException ex) {
        throw new ConcurrentModificationException();
    }
}
```

set方法：替换上一个操作的值/remove和set方法会校验lastRet < 0

```java
public void set(E e) {
    if (lastRet < 0)
        throw new IllegalStateException();
    checkForComodification();

    try {
        ArrayList.this.set(lastRet, e);
    } catch (IndexOutOfBoundsException ex) {
        throw new ConcurrentModificationException();
    }
}
```



### Iterable

iterator为Java中的迭代器对象，是能够对List这样的集合进行迭代遍历的底层依赖。而iterable接口里定义了返回iterator的方法，相当于对iterator的封装，同时实现了iterable接口的类可以支持for each循环。

##### for each原理:

其实for each循环内部也是依赖于Iterator迭代器，只不过Java提供的语法糖，Java编译器会将其转化为Iterator迭代器方式遍历。我们对以下for each循环进行反编译：

```java
 for (Integer i : list) {
       System.out.println(i);
   }
```

反编译后：

```java
Integer i;
for(Iterator iterator = list.iterator(); iterator.hasNext(); System.out.println(i)){
        i = (Integer)iterator.next();        
    }
```

##### *terator 和 ListIterator 的不同点*

1.使用范围不同，Iterator可以应用于所有的集合，Set、List和Map和这些集合的子类型。而ListIterator只能用于List及其子类型。

2.ListIterator有add方法，可以向List中添加对象，而Iterator不能。

3.ListIterator和Iterator都有hasNext()和next()方法，可以实现顺序向后遍 历，但是ListIterator有hasPrevious()和previous()方法，可以实现逆向（顺序向前）遍历。Iterator不可以。

4.ListIterator可以定位当前索引的位置，nextIndex()和previousIndex()可以实现。Iterator没有此功能。

5.都可实现删除操作，但是ListIterator可以实现对象的修改，set()方法可以实现。Iterator仅能遍历，不能修改。

Java Collections 框架中提供了一个 RandomAccess 接口，用来标记 List 实现是否支持 Random Access。

- 如果一个数据集合实现了该接口，就意味着它支持 Random Access，按位置读取元素的平均时间复杂度为 O(1)，如ArrayList。
- 如果没有实现该接口，表示不支持 Random Access，如LinkedList。

推荐的做法就是，支持 Random Access 的列表可用 for 循环遍历，否则建议用 Iterator 或 foreach 遍历。



https://juejin.im/post/6856550047338332168#heading-4

https://thinkwon.blog.csdn.net/article/details/104588551