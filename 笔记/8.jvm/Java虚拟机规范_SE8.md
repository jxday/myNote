# Java虚拟机规范-se8

>源代码、字节码、机器码、加载到内存、垃圾回收

<img src="../../../../Library/Application%20Support/typora-user-images/image-20210218090423032.png" alt="image-20210218090423032" style="zoom:50%;" />

## 第二章	java虚拟机结构

> [jvm内存模型-](https://juejin.im/post/6844903592374042637)
>
> 

<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200826134721653.png" alt="image-20200826134721653" style="zoom:50%;" />

<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200826135711197.png" alt="image-20200826135711197" style="zoom:50%;" />本规范描述的是一种抽象化的虚拟机的行为。

原始类型：数值类型numeric type，boolean类型，returnAddress类型

引用类型：类类型class type，数组类型array type，接口类型interface type

### 运行时数据区：

1. pc(program counter)寄存器：任意时刻，一条java虚拟机线程只会执行一个方法的代码，这个正在被线程执行的方法被称为该线程的当前方法。如果这个方法不是native的，那pc寄存器就保存Java虚拟机正在执行的字节码指令的地址，如果该方法是native的，那pc寄存器的值就是undefined。
    可以看做是当前线程所执行的字节码的行号指示器

 	2. java虚拟机栈：每一条java虚拟机线程都有自己私有的java虚拟机栈 java virtual machine stack，这个栈与线程同时创建，用于存储`栈帧Frame`。用于存储局部变量与一些尚未算好的结果。除了栈帧的出栈和入栈之外，java虚拟机不再受其他因素的影响，所以栈帧可以在堆中分配，java虚拟机栈所使用的内存不需要保证是连续的。如果线程请求分配的栈容量超过java虚拟机栈允许的最大容量，java虚拟机将会抛出StackOverflowError异常。如果java虚拟机栈可以动态扩展，并且尝试扩展的时候无法申请到足够的内存或者在创建新的线程时没有足够的内存区创建对应的虚拟机栈，那java虚拟机抛出OutOfMemoryError异常。
 	3. java堆：堆heap是可供各个线程共享的运行时内存区域，也是供所有类实例和数组对象分配内存的区域。java堆在虚拟机启动的时候就被创建，它存储了被自动内存管理系统 （automatic storage management system 也就是garbage collector）所管理的各个对象。java堆的容量可以是固定的，也可以是动态扩展，并在不需要过多空间时自动收缩。java堆内存不需要保证是连续的。如果实际所需的堆超过了自动内存管理系统能提供的最大容量，那java虚拟机抛出OutOfMemoryError异常。
   	4. 方法区 method area：方法区是可供各个线程共享的运行时内存区域。方法区与传统语言中的编译代码存储区类似，存储了每一个类的结构信息。例如：运行时常量池runtime constant pool，字段和方法数据，构造函数和普通方法的字节码内容，还包括一些在类、实例、接口初始化时用到的特殊方法（2.9）。方法区在虚拟机启动的时候创建，是堆的逻辑组成部分。如果方法区的内存空间不能满足内存分配请求，那么java虚拟机将抛出OutOfMemoryError异常。
   	5. 运行时常量池runtime constant pool：动态常量池，是class文件中每一个类或接口的常量池表(4.4)的运行时表示形式。包括了若干种不同的常量，从编译期可知的数值字面量到必须在运行期解析后才能获得的方法或字段引用。每一个运行时常量池都在java虚拟机的`方法区`中分配，在加载类和接口到虚拟机后，就创建对应的运行时常量池。抛出OutOfMemoryError异常。
   	6. 本地方法栈：与虚拟机栈非常相似，其区别不过是虚拟机栈为虚拟机执行Java方法（也就是字节码）服务，而**本地方法栈则是为虚拟机使用到的Native 方法服务**。虚拟机规范中对本地方法栈中的方法使用的语言、使用方式与数据结构并没有强制规定，因此具体的虚拟机可以自由实现它。甚至有的虚拟机（譬如Sun HotSpot 虚拟机）直接就把本地方法栈和虚拟机栈合二为一。与虚拟机栈一样，本地方法栈区域也会抛出StackOverflowError和OutOfMemoryError异常。

### 栈帧 frame

是用来存储数据和部分过程结果的数据结构。同时也用来处理动态链接dynamic linking 方法返回值和异常分派 dispatch exception

栈帧随着方法的调用而创建，方法介绍销毁。存储空间由线程分配在java虚拟机栈，拥有自己的本地变量表 local variable，操作数栈operand stack和指向当前方法所属的类的运行时常量池的引用。

在某条线程执行过程中的某个时间点上，只有目前正在执行的那个方法的栈帧是活动的，称为`当前栈帧` current frame ，这个栈帧对应的方法称为`当前方法` current method，定义这个方法的类称作`当前类` current class。方法返回之际，当前栈帧会传回此方法的执行结果给前一个栈帧，然后虚拟机会抛弃当前栈帧，使得前一个栈帧重新成为当前栈帧。（想象一下递归，前置函数、后置函数和递归体）

#### -局部变量表

每个栈帧内部都包含一组称为局部变量表的变量列表，长度由编译期决定，并且存储于类或者接口的二进制表示之中，即通过方法的code属性（4.7.3）保存及提供给栈帧使用。一个局部变量可以保存一个类型为boolean、byte、char、short、int、flaot、reference或returnAddress的数据，两个连续的局部变量可以保存一个类型为long或double的数据。局部变量使用索引来定位，0到n-1。long或double的数据采用较小的索引来定位。java虚拟机不要求double和long类型数据采用64位对齐的方式连续地存储在局部变量表中。java虚拟机使用局部变量表来完成方法调用时的参数传递。

#### -操作数栈

每个栈帧内部都包含一个称为操作数栈的后进先出栈。最大深度由编译期决定，并通过方法的code属性保存及提供给栈帧使用。栈帧刚刚创建时，操作数栈是空的，java虚拟机提供一些字节码指令来从局部变量表或者对象实例的字段中复制常量或变量值到操作数栈中，也提供了一些指令用于从操作数栈取走数据、操作数据以及把操作结果重新入栈。

#### -动态链接

每个栈帧内部都包含一个指向当前方法所在类型的运行时常量池的引用。在class文件里，一个方法若要调用其他方法，或者访问成员变量，则需要通过符号引用symbolic reference来表示，动态链接的作用就是将这些以符号引用所表示的方法转换为对实际方法的直接引用。类加载的过程中将要解析尚未被解析的符号引用，并且将对变量的访问转化为变量在程度运行时，位于存储结构中的正确偏移量。由于对其他类中的方法和变量进行了晚期绑定late binding ，所以即使这些类发生变化，也不会影响调用它们的方法。

早期（或静态）绑定是指编译时绑定，后期（或动态）绑定是指运行时绑定（例如，当你使用反射时）。

方法调用正常完成：栈帧承担着恢复调用者状态的责任，包括恢复调用者的局部变量表和操作数栈，以及正确递增程序计数器，以跳过刚才执行的方法调用指令等。调用者的代码在被调用方法的返回值压入调用者栈帧的操作数栈后，会继续正常执行。

### 特殊方法

### 异常

[jvm如何处理异常](https://www.cnblogs.com/qdhxhz/p/10765839.html)

java虚拟机里面的异常使用throwable或其子类的实例来表示，抛异常的本质实际上是程序控制权的一种即时的、非局部的转换，从异常抛出的地方转换至处理异常的地方。

java虚拟机规范允许在异步异常抛出之前额外执行一小段有限的代码，使得代码优化器能够在不违反java语言语义的前提下检测并把这些异常在可处理他们的地方抛出。简单的java虚拟机实现，可以在程序执行控制权转移指令时，处理异步异常。由java虚拟机执行的每个方法都会配有零至多个异常处理器exception handler。搜索异常处理器时的搜索顺序是很关键的，在class文件里面，每个方法的异常处理器都存储在一个表中，运行时如果有异常抛出，java虚拟机就按照class文件中的异常处理器表所描述的异常处理器的先后顺序，从前至后进行搜索。

### 字节码指令集简介

java虚拟机的指令由一个字节长度的、代表着某种特定操作含义的操作码opcode以及跟随其后的零至多个代表此操作所需参数的操作数operand所构成。如果忽略异常处理，那么java虚拟机的解释器通过下面这个伪代码的循环即可有效工作：

```java
do{
	自动计算pc寄存器以及从pc寄存器的位置取出操作码;
	if(存在操作数)取出操作数;
	执行操作码所定义的操作;
}while(处理下一次循环);
```

#### 数据结构与java虚拟机

在java虚拟机的指令集中，大多数指令都包含了其所操作的数据类型信息。

#### 加载与存储指令

加载和存储指令用于将数据从栈帧的本地变量表和操作数栈之间来回传递。

<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200828175509207.png" alt="image-20200828175509207" style="zoom:50%;" />

#### 对象的创建与操作

虽然类实例和数组都是对象，但java虚拟机对类实例和数组的创建和操作使用了不同的字节码指令：<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200831111834701.png" alt="image-20200831111834701" style="zoom:50%;" />

#### 操作数栈管理指令

java虚拟机提供了一些用于直接控制操作数栈的指令，包括：pop、pop2、dup、dup2、dup_x1、dup2_x1、dup_x2、dup2_x2和swap。

#### 控制转移指令

<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200831114338571.png" alt="image-20200831114338571" style="zoom:50%;" />

#### 方法调用和返回指令

invokevirtual：用于调用对象的实例方法

invokeinterface：用于调用接口方法

invokespecial：用于调用一些需要特殊处理的实例方法，比如初始化方法、私有方法和父类方法

invokestatic：用于调用命名类中的类方法（static方法）

invokedynamic：用于调用以绑定了invokedynamic指令的调用点对象作为目标的方法。

方法返回指令根据返回值的类型进行区分，还有一条return指令供声明为void的方法、实例初始化方法、类和接口的类初始化方法使用。

#### 抛出异常

在程序中显式抛出异常的操作由athrow指令实现，除了这种情况，还有别的异常会在其他java虚拟机指令检测到异常情况时虚拟机自动抛出

#### 同步

java虚拟机可以支持方法级的同步和方法内部一段指令序列的同步，这两种同步结构都是使用同步锁monitor来支持的。

方法级的同步是隐式的，即无需通过字节码指令来控制，它实现在方法调用和返回操作中。虚拟机可以从方法常量池中对的方法表结构method-info structure中的ACC_SYNCHRONIZED访问标志区分一个方法是否是同步方法。

指令序列的同步通常用来表示java语言中的synchronized块，java虚拟机的指令集中有monitorenter和mintorexit两个指令来支持这种synchronized关键字的语义。正确实现synchronized关键字需要编译器与java虚拟机两者协作支持（3.14）

## java虚拟机编译器

[jvm指令集](https://juejin.im/entry/6844903461339807757)

istore_1指令的作用是从操作数栈中弹出一个int类型的值，并保存在第一个局部变量中。

iload_1指令的作用是将第一个局部变量的值压入操作数栈。

由于有了专门定制的load和store指令，所以编译器的开发者应尽可能多地重用局部变量，这样会使得代码更高效、更简洁，占用的内存(当前栈帧的空间)更少。

在java虚拟机中，因缺乏对byte、char和short类型数据的直接操作指令而带来的问题并不大，因为这些类型的值会自动提升为int类型。唯一额外的代价是要将操作结果截短到它们的有效范围内。

#### 算数运算

java虚拟机通常基于操作数栈进行算术运算，只有iinc指令例外，它直接对局部变量进行自增操作。

#### 访问运行时常量池

很多数值常量，以及对象、字段和方法，都是通过当前类的运行时常量池进行访问的。

ldc和ldc_w指令用于访问运行时常量池中的值，这包括类string的实例。ldc2_w指令用于访问类型为double和long的运行时常量池项。

#### 接受参数

如果传递了n个参数给某个实例方法，则当前栈帧会按照约定，一参数传递顺序来接收这些参数，将它们保存到方法的第1个至第n个局部变量之中。按照约定，需要给实例方法传递一个指向该实例的引用作为方法的第0个局部变量，在java语言中，自身实例可以通过this关键字来访问。由于static类方法不需要传递实例引用，所以不需要使用第0个局部变量来保存this关键字，而是会用它来保存方法的首个参数。

#### 方法调用

普通实例方法调用是在运行时根据对象类型进行分派的。这类方法调用通过invokevirtual指令实现，invokevirtual指令都会带有一个表示索引的参数，运行时常量池在该索引处的项为某个方法的符号引用，这个符号引用可以提供方法所在对象的类型的内部二进制名称、方法名称和方法描述符。

#### 数组

在java虚拟机中，数组也使用对象来表示，由专门的指令集来创建和操作。newarray指令用于创建元素类型为数值类型的数组。anewarray指令用于创建元素为对象引用的一维数组。也可以使用multianewarray指令一次性创建多维数组。

#### 编译switch语句

编译器会使用tableswitch和lookupswitch指令来生成switch语句的变异代码。tableswitch指令用于表示switch结构中case语句块，它可以高效的从索引表中确定case语句块的分支偏移量。

#### 编译finally语句块

把控制权转移到try结构外围之前，首先要执行finally里的内容，无论try部分是整成执行完毕还是在执行过程中抛出异常，都要如此。

#### 同步

java虚拟机中的同步synchronization是用monitor的进入和退出来实现的。无论显式同步(有明确的monitorenter和monitorexit指令)，还是隐式同步(依赖方法调用和返回指令实现)都是如此。在java语言中，同步用得最多的地方可能是经synchronized所修饰的同步方法。同步方法并不是用monitorenter和monitorexit指令来实现的，而是由方法调用指令读取运行时常量池中方法的ACC_SYNCHRONIZED标志来隐式实现的。

编译器必须确保无论方法以何种方式完成，方法中调用过的每条monitorenter指令都必须有对应的monitorexit指令得到执行。为了确保正确配对执行，编译器会自动产生一个异常处理器，这个异常处理器宣称自己可以处理所有异常，它的代码用来执行monitorexit指令。

## 4.class文件格式

> 本章将描述java虚拟机中定义的class文件格式。每一个class文件都对应着唯一一个类或者接口的定义信息。但是类或接口并不一定都必须定义在文件里(类或接口也可以通过类加载器直接生成)
>
> 每个class文件都由字节流组成，每个字节含有8个二进制位。所有16位，32位和64位长度的数据将通过构造成2个，4个和8个连续的8位字节来表示。多字节数据项总是按照big-endian大端在前的顺序进行存储。在class文件爱你中国呢，各项按照严格顺序连续存放的，它们之间没有用任何填充或对齐作为各项间的分隔符号。

![image-20200903183642651](/Users/cty/Library/Application Support/typora-user-images/image-20200903183642651.png)

### 描述符descriptor

是一个描述字段或方法的类型的字符串。

### 常量池

java虚拟机指令不依赖于类、接口、类实例或数组的运行时布局，而是依赖常量池表中的符号信息。<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200903195725201.png" alt="image-20200903195725201" style="zoom:50%;" />

### 字段field

每个字段都由field_info结构所定义

<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200903195537722.png" alt="image-20200903195537722" style="zoom:50%;" />

### 方法method

所有方法，包括实例初始化方法以及类或接口初始化方法在哪，都由method_info结构来定义。<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200903200014357.png" alt="image-20200903200014357" style="zoom:50%;" />

### 属性attribute

属性在class文件格式中的classfile结构、field_info结构、method_info结构和code_attribute结构都由使用。<img src="/Users/cty/Library/Application Support/typora-user-images/image-20200903200426922.png" alt="image-20200903200426922" style="zoom:50%;" />

###  格式检查

如果java虚拟机准备加载某个class文件，那么它首先应保证这个文件符合class文件的基本格式。

1. 前四个字节必须是正确的魔数 cafebabe
2. 能够辨识出来的所有属性都必须具备合适的长度
3. class文件的内容不能缺失，尾部也不能有多余字节
4. 常量池必须符合所规定的各项约定
5. 常量池中的所有字段引用及方法引用，都必须具备有效的名称、有效的类及有效的描述符

### 文件校验

java虚拟机需要为自己来校验准备载入的class文件能否满足规定的约束条件，保证解释器在运行期可以假定这些校验都做过了。

1. 操作数栈不会发生上限或下限溢出
2. 所有局部变量的使用和存储都是有效的
3. 所有java虚拟机指令都拥有正确的参数类型
4. code属性的code数组能够符合静态约束和结构化约束
5. 确保final类没有子类，确保final方法没有为其他方法所覆写，确保除Object类之外的其他类都有直接超类

## 5.加载、链接与初始化

> 加载时根据特定名称查找类或接口类型的二进制表示，并由此二进制表示
>
> 来创建类或接口的过程。
>
> 链接是为了让类或接口可以被java虚拟机执行，而将类或接口并入虚拟机运行时状态的过程。
>
> 类或接口的初始化是指执行类或接口的初始化方法<clinit>

### 5.1运行时常量池

>java虚拟机如何从类或接口的二进制表示中得到符号引用

当类或接口创建时，它的二进制表示中的常量池表被用来构造运行时常量池，运行时常量池中的所有引用最初都是符号引用。

### 5.2虚拟机启动

java虚拟机的启动时通过引导类加载器 bootstrap class loader 创建一个初始类 initial class来完成的(带有main方法的类)。虚拟机实现也可以令初始类设定类加载器，并且用这个加载器依次加载整个应用。

### 5.3创建和加载

如果要创建标记为N的类或接口C，就需要现在java虚拟机方法区上为C创建与虚拟机实现相匹配的内部表示。如果C不是数组类，那么它可以通过类加载器加载C的二进制表示来创建。

java虚拟机支持两种类加载器：java虚拟机提供的引导类加载器和用户自定义的类加载器。每个用户自定义的类加载器应该是抽象类ClassLoader的某个子类的实例。应用程序使用用户自定义类加载器时为了便于扩展java虚拟机的功能，以支持动态加载并创建类。

在java虚拟机运行时，类或接口不仅仅是由它的并称来确定，而是由一个值对：二进制名称和它的定义类加载器共同确定的。每个这样的类或接口都只属于一个运行时包结构runtime package。

