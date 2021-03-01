# java双大括号初始化

### 什么是Java双大括号初始化？

通常情况下，初始化Java集合并向其中添加几个元素的步骤如下：

```java
Set<Integer> set = new HashSet<>();
set.add(1);
set.add(2);
set.add(3);
```

或者我们可以在静态初始化块中向作为静态变量的集合添加元素：

```java
private static final Set<Integer> set = new HashSet<>();
static {
    set.add(1);
    set.add(2);
    set.add(3);
};
```

从语法上来看，这样的初始化方法虽然格式清晰明了，但语法上略显冗余。事实上，Java允许一种精简的双大括号初始化方法：

```java
Set<Integer> set = new HashSet<Integer>() {{
    add(1);
    add(2);
    add(3);
}};
```

或是在接收集合作为输入的函数中直接初始化：

```java
someFunction(new HashSet<Integer>() {{
    add(1);
    add(2);
    add(3); 
}}
);
```

## 语法解读

事实上，如下双大括号初始化`Set`的代码

```java
Set<Integer> set = new HashSet<Integer>() {{
    add(1);
    add(2);
    add(3);
}};
```

是以下这段代码的改写：

```java
Set<Integer> set = new HashSet<Integer>() {
    {
        add(1);
        add(2);
        add(3);
    }
};
```

改写之后的代码就非常容易理解了。显然这是在HashSet的构造器中写了一个匿名内部类，这个匿名内部类含有一个实例初始化块，初始化块的内容是三个`add()`函数，向被初始化的`this`指向的`HashSet`中添加了三个元素。

## 效率问题和产生的`.class`文件结构

利用双大括号初始化集合从效率上来说可能不如标准的集合初始化步骤。原因在于使用双大括号初始化会导致内部类文件的产生，而这个过程就会影响代码的执行效率。

例如如下代码：

```java
// Double brace initialization
class Test1 {
    public static void main(String[] args) {
        long st = System.currentTimeMillis();

    Set<Integer> set0 = new HashSet<Integer>() {{
        add(1);
        add(2);
    }};

    Set<Integer> set1 = new HashSet<Integer>() {{
        add(1);
        add(2);
    }};

    /* snip */

    Set<Integer> set999 = new HashSet<Integer>() {{
        add(1);
        add(2);
    }};

      System.out.println(System.currentTimeMillis() - st);
  }
}12345678910111213141516171819202122232425
// Normal initialization
class Test2 {
    public static void main(String[] s) {
        long st = System.currentTimeMillis();

        Set<Integer> set0 = new HashSet<Integer>();
        set0.add(1);
        set0.add(2);

        Set<Integer> set1 = new HashSet<Integer>();
        set1.add(1);
        set1.add(2);

        /* snip */

        Set<Integer> set999 = new HashSet<Integer>();
        set999.add(1);
        set999.add(2);  
        System.out.println(System.currentTimeMillis() - st);
  }
}
```

用`javac`命令compile第一段代码会产生1000个`.class`文件，如下所示：

```java
Test1$1.class
Test1$2.class
...
Test1$1000.class1234
```

同时从程序输出的运行时间来看， 双大括号初始化也显著慢于普通初始化的代码。差值大约在100ms左右。