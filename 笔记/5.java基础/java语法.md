### java语言的特点

<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200111094817138.png" alt="image-20200111094817138" style="zoom:50%;" />

### 引用类型

Java中有`strong、soft、weak、phantom`四种引用类型，下面介绍一下soft引用和weak引用：

- `Soft Reference`: 当对象是`Soft reference`可达时，向系统申请更多内存，GC不是直接回收它，而是当内存不足的时候才回收它。因此Soft reference适合用于构建一些缓存系统。
- `Weak Reference`: 弱引用的强度比软引用更弱一些，被弱引用关联的对象只能生存到下一次GC发生之前。当垃圾收集器工作时，无论当前内存是否足够，都会回收掉只被弱引用关联的对象。

#### Java 只有值传递，没有引用传递。

```java
public static void main(String[] args) {
    String x = new String("沉默王二");
    change(x);
    System.out.println(x);
}

public static void change(String x) {
    x = "沉默王三";
}

//结果：“沉默王二”
//资料：https://mp.weixin.qq.com/s/Iq6C56_H6CThR5w6v_sQ7Q
```





## Lambda表达式的结构

让我们检查lambda表达式的结构。

- Lambda表达式可以具有零个，一个或多个参数。
- 参数的类型可以显式声明，也可以从上下文中推断出来。例如`(int a)`与`(a)`
- 参数用括号括起来，并用逗号分隔。例如，`(a, b)`或`(int a, int b)`或`(String a, int b, float c)`
- 空括号用于表示空参数集。例如`() -> 42`
- 当有单个参数时，如果推断出其类型，则不强制使用括号。例如`a -> return a*a`
- Lambda表达式的主体可以包含零个，一个或多个语句。
- 如果lambda表达式的主体具有单个语句，则不必使用大括号，并且匿名函数的返回类型与主体表达式的返回类型相同。
- 如果正文中有多个语句，则必须将这些语句括在大括号（一个代码块）中，并且匿名函数的返回类型与该代码块内返回的值的类型相同；如果没有，则返回void回。



