# String类

> 常用api

```java
    public boolean isEmpty() //判断字符串是否为空
    public int length() //获取字符串长度
    public String substring(int beginIndex) //取子字符串
    public String substring(int beginIndex, int endIndex) //取子字符串
    public int indexOf(int ch) //查找字符，返回第一个找到的索引位置，没找到返回-1
    public int indexOf(String str) //查找子串，返回第一个找到的索引位置，没找到返回-1
    public int lastIndexOf(int ch) //从后面查找字符
    public int lastIndexOf(String str) //从后面查找子字符串
    public boolean contains(CharSequence s) //判断字符串中是否包含指定的字符序列
    public boolean startsWith(String prefix) //判断字符串是否以给定子字符串开头
    public boolean endsWith(String suffix) //判断字符串是否以给定子字符串结尾
    public boolean equals(Object anObject) //与其他字符串比较，看内容是否相同
    public boolean equalsIgnoreCase(String anotherString) //忽略大小写比较是否相同
    public int compareTo(String anotherString) //比较字符串大小
    public int compareToIgnoreCase(String str) //忽略大小写比较
    public String toUpperCase() //所有字符转换为大写字符，返回新字符串，原字符串不变
    public String toLowerCase() //所有字符转换为小写字符，返回新字符串，原字符串不变
    public String concat(String str) //字符串连接，返回当前字符串和参数字符串合并结果
    public String replace(char oldChar, char newChar) //字符串替换，替换单个字符
    //字符串替换，替换字符序列，返回新字符串，原字符串不变
    public String replace(CharSequence target, CharSequence replacement)
    public String trim() //删掉开头和结尾的空格，返回新字符串，原字符串不变
    public String[] split(String regex) //分隔字符串，返回分隔后的子字符串数组
```



### 字符串常量

> 在 JDK 1.8 之后已经不允许 Object 和 int 类型用 == 相比较了，实际开发中也强烈不推荐用 == 符号判断两个字符串是否相等，应该用 equals() 方法。
>
> 在 JDK 1.7 之后(包括1.7)，字符串常量池已经从方法区移到了堆中。



##### 1. 字面量赋值

```java
String s1 = "古时的风筝";
```

这是我们平时声明字符串变量的最常用的方式，这种方式叫做字面量声明，也就用把字符串用双引号引起来，然后赋值给一个变量。

这种情况下会直接将字符串放到字符串常量池中，然后返回给变量。

<img src="https://oscimg.oschina.net/oscnet/up-a71b573957258fcda994f67e633ccbf7075.png" alt="img" style="zoom:25%;" />

那这是我再声明一个内容相同的字符串，会发现字符串常量池中已经存在了，那直接指向常量池中的地址即可。

![img](https://oscimg.oschina.net/oscnet/up-80e9cc27833f193bd1bdbd6baf73031e133.png)

例如上图所示，声明了 s1 和 s2，到最后都是指向同一个常量池的地址，所以 s1== s2 的结果是 true。



##### 2. new String() 方式

与之对应的是用 new String() 的方式，但是基本上不建议这么用，除非有特殊的逻辑需要。

```java
String a = "古时的";
String s2 = new String(a + "风筝");
```

使用这种方式声明字符串变量的时候，会有两种情况发生。

##### 第一种情况，字符串常量池之前已经存在相同字符串

比如在使用 new 之前，已经用字面量声明的方式声明了一个变量，此时字符串常量池中已经存在了相同内容的字符串常量。

1. 首先会在堆中创建一个 s2 变量的对象引用；
2. 然后将这个对象引用指向字符串常量池中的已经存在的常量；

![img](https://oscimg.oschina.net/oscnet/up-750091833155a2d1b90d943dd30915cb167.png)

##### 第二种情况，字符串常量池中不存在相同内容的常量

之前没有任何地方用到了这个字符串，第一次声明这个字符串就用的是 new String() 的方式，这种情况下会直接在堆中创建一个字符串对象然后返回给变量。

![img](https://oscimg.oschina.net/oscnet/up-693125cbe67b47b0cf41461a398d496ab20.png)

**我看到好多地方说，如果字符串常量池中不存在的话，就先把字符串先放进去，然后再引用字符串常量池的这个常量对象，这种说法是有问题的，只是 new String() 的话，如果池中没有也不会放一份进去。**

基于 new String() 的这种特性，我们可以得出一个结论：

```
String s1 = "古时的风筝";
String a = "古时的";
String s2 = new String(a + "风筝");
String s3 = new String(a + "风筝");
System.out.println(s1==s2); // false
System.out.println(s2==s3);  // false 
```

以上代码，肯定输出的都是 false，因为 new String() 不管你常量池中有没有，我都会在堆中新建一个对象，新建出来的对象，当然不会和其他对象相等。



##### 3. intern() 池化

那什么时候会放到字符串常量池呢，就是在使用 intern() 方法之后。

intern() 的定义：如果当前字符串内容存在于字符串常量池，存在的条件是使用 equas() 方法为ture，也就是内容是一样的，那直接返回此字符串在常量池的引用；如果之前不在字符串常量池中，那么在常量池创建一个引用并且指向堆中已存在的字符串，然后返回常量池中的地址。

##### 第一种情况，准备池化的字符串与字符串常量池中的字符串有相同(equas()判断)

```
String s1 = "古时的风筝";
String a = "古时的";
String s2 = new String(a + "风筝");
s2 = s2.intern();
```

这时，这个字符串常量已经在常量池存在了，这时，再 new 了一个新的对象 s2，并在堆中创建了一个相同字符串内容的对象。

![img](https://oscimg.oschina.net/oscnet/up-2d33d91e4aa86f95bd0f6cabbaf7d8b86d5.png)

这时，s1 == s2 会返回 fasle。然后我们调用 s2 = s2.intern()，将池化操作返回的结果赋值给 s2，就会发生如下的变化。

![img](https://oscimg.oschina.net/oscnet/up-4b60e9e6f7eeab1ed4b4b598bc71d9c09cf.png)

此时，再次判断 s1 == s2 ，就会返回 true，因为它们都指向了字符串常量池的同一个字符串。

##### 第二种情况，字符串常量池中不存在相同内容的字符串

使用 new String() 在堆中创建了一个字符串对象

![img](https://oscimg.oschina.net/oscnet/up-cebdd182a59d3c990a76a37fd7ac552efe0.png)

使用了 intern() 之后发生了什么呢，在常量池新增了一个对象，但是 **并没有** 将字符串复制一份到常量池，而是直接指向了之前已经存在于堆中的字符串对象。因为在 JDK 1.7 之后，字符串常量池不一定就是存字符串对象的，还有可能存储的是一个指向堆中地址的引用，现在说的就是这种情况，注意了，下图是只调用了 `s2.intern()`，并没有返回给一个变量。其中字符串常量池（0x88）指向堆中字符串对象（0x99）就是intern() 的过程。

![img](https://oscimg.oschina.net/oscnet/up-2869ea4e2f7455cf31c48696c1dc3b370ec.png)

只有当我们把 s2.intern() 的结果返回给 s2 时，s2 才真正的指向字符串常量池。

![img](https://oscimg.oschina.net/oscnet/up-3d5bb6f4d9fac141184b2a46157c191ca42.png)

### 我明白了

通过以上的介绍，我们来看下面的一段代码返回的结果是什么

```
public class Test {

    public static void main(String[] args) {
        String s1 = "古时的风筝";
        String s2 = "古时的风筝";
        String a = "古时的";
      
        String s3 = new String(a + "风筝");
        String s4 = new String(a + "风筝");
        System.out.println(s1 == s2); // 【1】 true
        System.out.println(s2 == s3); // 【2】 false
        System.out.println(s3 == s4); // 【3】 false
        s3.intern();
        System.out.println(s2 == s3); // 【4】 false
        s3 = s3.intern();
        System.out.println(s2 == s3); // 【5】 true
        s4 = s4.intern();
        System.out.println(s3 == s4); // 【6】 true
    }
}
```

【1】：s1 == s2 返回 ture，因为都是字面量声明，全都指向字符串常量池中同一字符串。

【2】: s2 == s3 返回 false，因为 new String() 是在堆中新建对象，所以和常量池的常量不相同。

【3】: s3 == s4 返回 false，都是在堆中新建对象，所以是两个对象，肯定不相同。

【4】: s2 == s3 返回 false，前面虽然调用了 intern() ，但是没有返回，不起作用。

【5】: s2 == s3 返回 ture，前面调用了 intern() ，并且返回给了 s3 ，此时 s2、s3 都直接指向常量池的同一个字符串。

【6】: s3 == s4 返回 true，和 s3 相同，都指向了常量池同一个字符串。