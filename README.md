# 对象论

## 什么是对象

知道世界上什么人最幸福吗？当然就是程序猿了，因为他们从不缺对象。

言归正传，到底什么是对象。

在面向对象编程中，对象具有状态，可以执行操作。和现实中的对象对比，具有特性和行为。

1. **万物皆对象**。将对象看成一种特殊的变量，可以存储数据，可以在自身上执行操作，也可以抽取待解决问题中概念化的构件，将其表示为程序中的对象。

2. **程序是对象的集合**。它们通过发送消息来彼此交流，可以把消息看成是对某个对象的方法的调用请求。

3. **对象即是道**。道生一，一生二，二生三，三生万物。创建一个对象，可以通过创建的对象来创建其他类型的对象。（老子在现代一定是个很牛逼的程序猿大佬）

4. **每个对象都具有类型**。每个对象都是某个类(class)的的一个实例。类即是类型的意思。

5. **某种特定类型的对象可以接收相同的消息**。例如类型“轿车”的对象也是是类型“汽车”的对象，所以“轿车”类型的对象也可以接受“汽车”类型的消息。

更简单一点来理解就是，对象具有状态、行为和标识。状态表明，对象自身具有的内部结构来表明自己的状态；行为表名，对象具有自己的方法来执行某些操作的行为；标识表明，每一个对象在内存中都有自己的地址，可以和唯一和其他对象区分开。

## 怎么获取对象

每个对象都具有类型。就像是鸟，鸟是类型，麻雀就是鸟类型的对象，是鸟类的实例。所以想要有对象，必须先要有其类型，要么是一个类型的新实例，要么创建一个新类型，创建新类型下的对象。就像是生物，必有其科、属、种，如果是新发现的不归属现有的分类的新物种，就可能需要新定义一类了。

可知，要有对象必须先有类型（类）。

> 创建抽象数据类型（类）是面向对象程序设计的概念之一。

基本在所有的面向对象编程语言中，都是用`class`关键字来表示类型（类）。类型就是类，类就是类型。

> 类描述了具有相同特性（数据元素）和行为（功能）的对象集合。所以类就是一个数据类型。

类被创建了，接着就可以创建对象了，但是需要怎么来获取有用的对象呢？肯定有某些方式产生某些请求，使对象完成对应的操作，比如完成一笔交易、在屏幕上画画、打开开关等。每个对象都需要满足这些请求，而这些请求都是由接口定义的，而决定接口的就是类型。例如下图的电灯。

![](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200908160712982.png)

```java
Light lt = new Light();
lt.off();
lt.on();
```

接口确定了某一特定对象所能发出的请求。但是，程序中必须有满足这些请求的代码。这些代码合隐藏的数据一起构成了实现。在类中，每一个请求都有一个方法与之关联，当向对象发送请求时，与之关联的方法就会被调用。这个过程概括为：向某个对象“发送消息”（产生请求），这个对象便知道次消息的目的，然后执行对应的代码。

上例中类的名称是`Light`，类的对象是 `lt` ，可以向这个对象发送的请求时打开、关闭、调亮、调暗。

创建类`Light`对象的方式：定义对象的应用 `lt` ,然后通过`new`方法方法来创建该类型的对象。

# 一切都是对象

在Java中一切都是对象。

## 引用操纵对象

在Java中一切都是对象，所以操纵对象就有固定的语法来实现。尽管一切都看作是对象，但操纵的标识符实际上是对象的一个“引用”。

```Java
String s;
```

这里创建的只是引用，不是对象。如果向s发送一条消息，就会返回一个运行时异常。因为此时s并没有和任何事物相关联，有一个安全的做法就是，创建引用的同时进行初始化

```Java
String s = "adsad";
```

## 必须由你创建所有的对象

一旦创建了引用，就需要创建对象与之关联。用关键词new来实现，new的意思就是，给我一个新对象，前面的例子可以看成：

```java
String s = new String("adsad");
```

### 创建的对象存储在什么地方

在程序运行的时候，对象是存储在什么地方？内存是怎么样分配的。有五个地方可以存储数据：

**（1）寄存器。**

这是最快的存储区，是CPU内部用来存放数据的存储区域。

![image-20200915163930583](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200915163930583.png)

**（2）堆栈。**

堆栈是一种数据结构。位于RAM中，是存取速度仅次于寄存器。但缺点是，存在栈中的数据大小与生存期必须是确定的，缺乏灵活性。

Java**对象的引用**都是存放在栈中。

栈有一个很重要的**特性**，存在**栈中的数据可以共享**。

```java
int i = 3;
int j = 3;
```

Java编译器会先在栈中创建一个变量为 i 的内存空间，查找有没有值3的地址，没有找到就会开辟一个存放值为3的地址。`int j = 3` Java 编译器创建完`j` 的内存空间后，在栈中找到了值为3的地址，直接指向该地址。

**思考：**

```java
int i = 3;
int j = 3;
i = 8;
System.out.println("j =" + j)
```

这样的话 j 会输出多少？

当然输出`j =  8`。i 和 j 同时指向了3。如果再令 i = 8 ，会重新在占中查找 8 ，找到的话，i会重新指向该地址，并不会影响 j 。

**（3）堆**

一种通用的内存池位于RAM，用于存放所有的Java对象。和栈相比较，堆刻意动态的分配内存大小，也不用在意存放数据的生命周期，当需要一个对象时，只需要关键字 new ，就会堆中自动分配内存，创建对象。

```java
String a = new String("123");
```

当然这种方式也会有弊端，堆中进行内存的分配和数据的清理，会花费更多的时间。

**（4）常量存储**

常量值通常直接存放在程序代码中，这样他们就永远不会被改变。

**（5）非RAM存储**

数据完全在程序之外，不受程序的控制，例如流对象和持久化对象。流对象中，对象转换成字节流，通常发送到另一台机器中；持久化对象，对象一般存放在磁盘中，这样的话在需要时就可以恢复成常规的、基于RAM对象。Java中提供了例如JDBC这样的机制，实现对数据库存取和读取。

### 基本类型

Java中有常常用来一些类型，它们不用new来创建变量，而是创建一个非引用的自动变量，这个变量直接在栈中存储值，所以也更高效。

在java中每种基本数据类型所占存储空间的大小是固定不变的，并不会随着机器硬件架构的变化而变化。

| 基本类型 | 大小    | 最小值     | 最大值    | 包装类类型 |
| -------- | ------- | ---------- | --------- | ---------- |
| boolean  | -       | -          | -         | Boolean    |
| char     | 16 bit  | 0          | 65535     | Character  |
| byte     | 8 bits  | -128       | +127      | Byte       |
| short    | 16 bits | -2^15      | +2^15-1   | Short      |
| int      | 32 bits | -2^31      | +2^31-1   | Integer    |
| long     | 64 bits | -2^63      | +2^63-1   | Long       |
| float    | 32 bits | -3.403E38  | 3.403E38  | Float      |
| double   | 64 bits | -1.798E308 | 1.798E308 | Double     |

 boolean 类型在内存空间中所占大小没有明确的规定，数值只有 true 和 false

### 引用数据类型

数组、类、接口、枚举、标注

引用数据类型，声明是在栈区申请一片区域用于存放指向堆区的地址。引用数据类型的数值存放在堆区。

## 永远不需要销毁对象

对象的生命周期，什么时候需要销毁对象，这种顾虑永远不会出现在Java中，Java有自己的垃圾回收机制来帮我们去清理。

### 作用域

作用域决定了在其内定义的变量的可见性和生命周期。

在Java中，作用域由花括号的位置决定：

```java
{
    int i = 10;
    //只可以使用 i
    {
        int j = 12;
        // i j 都可以使用
    }
    
    //只可以使用 i
}
```

### 对象的作用域

当**new** 创建一个Java对象时，它可以存活在作用域之外。

```java
{
    String str = new String("is a Object");
}
```

对象引用`str`在作用域结束后就消失了，但是`str`指向的**String**对象仍存在并占据着内存空间。我们无法在作用域之外继续访问这个对象，因为它的引用超出了作用域范围。（后续可以看到，传递和复制对象引用）

思考：对象一直存在，那Java的内存空间肯定会爆炸，那要怎样防止对象填满内存空间。

Java最神奇的地方就是，Java有个**垃圾回收机制**，用来监听所有`new`创建的的对象，并辨别那些不在被引用的的对象，随后在释放这些对象的内存空间。

## 创建新的数据类型：类

- 对象主要指现实生活中客观存在的实体，在Java语言中对象体现为内存空间中堆一块存储区域。
- 类简单来就是“类型”，是对具有相同特征和行为的多个对象共性的抽象描述，在Java语言中体现为一种引用数据类型，里面包含了描述特征/属性的成员变量以及描述行为的成员方法。
- 类是用于构建对象的模板，对象的数据结构由定义它的类来决定。

**class**关键字后跟着就是类的名称

```java
class 类名{
    
}

class Person {
    
}
```

然后就可以使用new 关键字创建对象了

```java
Person person = new Person();
```

### 成员变量

一旦定义了一个类，就可以在类中设置两种类型的元素：成员变量和方法

成员变量可以是任何类型的对象（基本数据类型和引用数据类型），通过其引用进行通信。

成员变量如果是引用数据类型，就必须初始化该引用，使其和具体的对象关联（如使用new实现）。

```java
class Data{
    int i;
    double j;
    Person person = new Person();
}
```

#### 基本成员默认值

如果类的成员变量时基本数据类型，即使没有初始化，Java也会给它一个默认值。

| 基本数据类型 | 默认值 |
| ------------ | ------ |
| boolean      | false  |
| char         | null   |
| byte         | 0      |
| short        | 0      |
| int          | 0      |
| long         | 0      |
| float        | 0      |
| double       | 0      |

初始值对于程序来说可能是不正确的，不合法的，所以最好明确对变量进行初始化。

但是这种默认值初始化，在局部变量中不适用，Java编译不通过，必须进行明确的初始化。

## 方法

Java的方法决定一个对象能接受什么样的消息。方法的基本组成包括：名称、参数、返回值和方法体。

```java
class 类名{
    返回值类型 方法名（参数列表）{
        方法体;
    }
}

class Person{
    void show(){
        
    }
}
```

### 返回类型

返回类型主要描述在调用方法后，从方法返回值的类型。

- 返回值主要指从方法体内返回到方法体外的数据内容。
- 返回值类型主要指返回值的数据类型，可以是基本数据类型，也可以是引用数据类型。
- 当返回的数据内容是66时，则返回值类型写int即可
- 在方法体中使用return关键字可以返回具体的数据内容并结束当前方法。
- 当返回的数据内容是66时，则方法体中写return 66;即可
- 当该方法不需要返回任何数据内容时，则返回值类型写void即可。

### 参数列表

方法的参数列表给出了要传给方法的信息的类型和名称。

- 形式参数主要用于将方法体外的数据内容带入到方法体内部。
- 形式参数列表主要指多个形式参数组成的列表，语法格式如下：数据类型 形参变量名1,数据类型 形参变量名2, ...
- 当带入的数据内容是"hello"时，则形参列表写String s即可
- 当带入的数据内容是66和"hello"时，则形参列表写int i, String s即可
- 若该方法不需要带入任何数据内容时，则形参列表位置啥也不写即可

```java
objectName.methodName(arg1,arg2,arg3);
```

#### 可变长参数

- 返回值类型 方法名(参数的类型...参数名)
- 方法参数部分指定类型的参数个数是可以改变的，也就是0~n个 。
- 一个方法的形参列表中最多只能声明一个可变长形参，并且需要放到参数列表的末尾。



















