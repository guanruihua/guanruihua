---
title: java-basis
date: 2020-09-08 21:28:23
tags: 
	- java
---

# java

> [文档](http://c.biancheng.net/view/6262.html)
>
> cmd界面运行java文件
>
> 1. 创建.java文件
> 2. cmd界面到该文件路径下,  运行`javac 文件名.java`
> 3. 运行成功会生成一个.class文件
> 4. 再运行`java 文件名`就可以看到结果

## 基础



### 标识符

> - 标识符由数字（0\~9）和字母（A~Z 和 a~z）、美元符号（$）、下划线（_）以及 Unicode 字符集中符号大于 0xC0 的所有符号组合构成（各符号之间没有空格）。
> - 标识符的第一个符号为字母、下划线和美元符号，后面可以是任何字母、数字、美元符号或下划线。

### 关键字

> 1. 数据类型：boolean、int、long、short、byte、float、double、char、class、interface。
> 2. 流程控制：if、else、do、while、for、switch、case、default、break、continue、return、try、catch、finally。
> 3. 修饰符：public、protected、private、final、void、static、strict、abstract、transient、synchronized、volatile、native。
> 4. 动作：package、import、throw、throws、extends、implements、this、supper、instanceof、new。
> 5. 保留字：true、false、null、goto、const。



### 注释

```java
单行注释
//注释

多行注释
/* 
	注释 
*/
文档注释
/**
 * 文档注释
*/
```







### 基本数据类型

> null： 基本数据类型直接量只有一个值（null), 可以给任何数据类型赋值

| 名称    | 包装类    | 字节数  | 栗子        | 直接量                    |
| ------- | --------- | ------- | ----------- | ------------------------- |
| byte    | Byte      | 1       | 1           | 012(八进制)、0x12(16进制) |
| short   | Short     | 2       | 2           |                           |
| int     | Interger  | 4       | 123         |                           |
| long    | Long      | 8       | 123         |                           |
| float   | Float     | 4       | 3.14f       | 5.34F、3.14E5f            |
| double  | Double    | 8       | 1.1         | 5.34、3.14E5              |
| char    | Character | 2       | "关"        | ‘a’，‘\n’和‘\u0061’       |
| String  | /         | /       | "华"        |                           |
| boolean | Boolean   | 一般是4 | true\|flase |                           |

### 包装类和基本数据类型

> 包装类 : 能够直接将简单类型的变量表示为一个类，在执行变量类型的相互转换时，我们会大量使用这些包装类

#### 包装类的用途

> 1. 集合中不可以存放基本数据类型, 故常用包装类
> 2. 包含了每种基本类型的相关属性, 如最大值, 最小值, 所占位符数
> 3. 作为基本数据类型对应的类类型, 提供了一系列的对象操作, 如类型转换, 进制转换等

#### 基本类型与包装类型的区别

>  1、在Java中，一切皆对象，但八大基本类型却不是对象。
>
> 2、声明方式的不同，基本类型无需通过new关键字来创建，而封装类型需new关键字。
> 3、存储方式及位置的不同，基本类型是直接存储变量的值保存在堆栈中能高效的存取，封装类型需要通过引用指向实例，具体的实例保存在堆中。
> 4、初始值的不同，封装类型的初始值为null，基本类型的的初始值视具体的类型而定，比如int类型的初始值为0，boolean类型为false；
> 5、使用方式的不同，比如与集合类合作使用时只能使用包装类型。
> 6、什么时候该用包装类，什么时候用基本类型，看基本的业务来定：这个字段允许null值，就需要使用包装类型，如果不允许null值，使用基本类型就可以了，用到比如泛型和反射调用函数，就需要用包装类！
>
> 7、Ingeter是int的包装类,初始值null, int 初始值为0

### 引用数据类型

> - 类 class
> - 接口 interface
> - 数组 []

#### 数组

常用定义方法

```java
//一维数组
int[] str;
int str = new int[]{1, 2, 3, 4 };
int str = {1, 2, 3, 4, 5};
int[] str = new String[5];
//二维数组
int[][] str = new int[][]{{1, 2, 3, 4}, {1, 2, 3, 4} }
int[][] str = new int[2][3];
int[][] str = new int[2][];
```



### 类型转换

#### 隐式转换(自动类型转换)

- 数值型数据的转换：byte→short→int→long→float→double。
- 字符型转换为整型：char→int。

#### 显示转换(强制类型转换)

> 当转换的两种类型不兼容, 或目标类型的取值范围小于源数据类型时, 自动转换键无法进行, 这时需要进行强制类型转换

```java
(type)variableName
```

### java运算符

#### 一元运算符

| 运 算 符 | 名 称    | 说 明                        | 例 子      |
| -------- | -------- | ---------------------------- | ---------- |
| -        | 取反符号 | 取反运算                     | b=-a       |
| ++       | 自加一   | 先取值再加一，或先加一再取值 | a++ 或 ++a |
| --       | 自减一   | 先取值再减一，或先减一再取值 | a-- 或 --a |

> ++/ - - : 在前要先进行自加(减) 

#### 二元运算符

| 运 算 符 | 名 称 | 说 明                                                    | 例 子 |
| -------- | ----- | -------------------------------------------------------- | ----- |
| +        | 加    | 求 a 加 b 的和，还可用于 String 类型，进行字符串连接操作 | a + b |
| -        | 减    | 求 a 减 b 的差                                           | a - b |
| *        | 乘    | 求 a 乘以 b 的积                                         | a * b |
| /        | 除    | 求 a 除以 b 的商                                         | a / b |
| %        | 取余  | 求 a 除以 b 的余数                                       | a % b |

#### 算术赋值运算符

| 运 算 符 | 名 称    | 例 子            |
| -------- | -------- | ---------------- |
| +=       | 加赋值   | a += b、a += b+3 |
| -=       | 减赋值   | a -= b           |
| *=       | 乘赋值   | a *= b           |
| /=       | 除赋值   | a /= b           |
| %=       | 取余赋值 | a %= b           |

#### 逻辑运算符

| 运算符 | 用法   | 含义   | 说明                                               | 实例       | 结果  |
| :----- | :----- | :----- | :------------------------------------------------- | :--------- | :---- |
| &&     | a&&b   | 短路与 | ab 全为 true 时，计算结果为 true，否则为 false。   | 2>1&&3<4   | true  |
| \|\|   | a\|\|b | 短路或 | ab 全为 false 时，计算结果为 false，否则为 true。  | 2<1\|\|3>4 | false |
| !      | !a     | 逻辑非 | a 为 true 时，值为 false，a 为 false 时，值为 true | !(2>4)     | true  |
| \|     | a\|b   | 逻辑或 | ab 全为 false 时，计算结果为 false，否则为 true    | 1>2\|3>5   | false |
| &      | a&b    | 逻辑与 | ab 全为 true 时，计算结果为 true，否则为 false     | 1<2&3<5    | true  |

#### 关系运算符

| 运算符 | 含义       | 说明                                                         | 实例                            | 结果                 |
| ------ | ---------- | ------------------------------------------------------------ | ------------------------------- | -------------------- |
| >      | 大于       | 只支持左右两边操作数是数值类型。如果前面变量的值大于后面变量的值， 则返回 true。 | 2>3                             | false                |
| >=     | 大于或等于 | 只支持左右两边操作数是数值类型。如果前面变量的值大于等于后面变量的值， 则返回 true。 | 4>=2                            | true                 |
| <      | 小于       | 只支持左右两边操作数是数值类型。如果前面变量的值小于后面变量的值，则返回 true。 | 2<3                             | true                 |
| <=     | 小于或等于 | 只支持左右两边操作数是数值类型。如果前面变量的值小于等于后面变量的值， 则返回 true。 | 4<=2                            | false                |
| ==     | 相等       | 如果进行比较的两个操作数都是数值类型，无论它们的数据类型是否相同，只要它们的值相等，也都将返回 true。 如果两个操作数都是引用类型，只有当两个引用变量的类型具有父子关系时才可以比较，只要两个引用指向的不是同一个对象就会返回 true。 [Java](http://c.biancheng.net/java/) 也支持两个 boolean 类型的值进行比较。 | 4==4 97=='a' 5.0==5 true==false | true true true false |
| !=     | 不相等     | 如果进行比较的两个操作数都是数值类型，**无论它们的数据类型是否相同**，只要它们的**值不相等**，也都将返回 true。<br> 如果两个操作数都是引用类型，只有当两个引用变量的类型具有父子关系时才可以比较，只要两个引用指向的不是同一个对象就会返回 true。 | 4!=2                            | true                 |

#### 位运算符

| 运算符 | 含义                    | 实例   | 结果 |
| ------ | ----------------------- | ------ | ---- |
| &      | 按位进行与运算（AND）   | 4 & 5  | 4    |
| \|     | 按位进行或运算（OR）    | 4 \| 5 | 5    |
| ^      | 按位进行异或运算（XOR） | 4 ^ 5  | 1    |
| ~      | 按位进行取反运算（NOT） | ~ 4    | -5   |

#### 三目运算符

> `result = <expression> ? <statement1> : <statement3>;`

#### 运算符优先级

| 优先级 | 运算符                                           | 结合性   |
| ------ | ------------------------------------------------ | -------- |
| 1      | ()、[]、{}                                       | 从左向右 |
| 2      | !、+、-、~、++、--                               | 从右向左 |
| 3      | *、/、%                                          | 从左向右 |
| 4      | +、-                                             | 从左向右 |
| 5      | «、»、>>>                                        | 从左向右 |
| 6      | <、<=、>、>=、instanceof                         | 从左向右 |
| 7      | ==、!=                                           | 从左向右 |
| 8      | &                                                | 从左向右 |
| 9      | ^                                                | 从左向右 |
| 10     | \|                                               | 从左向右 |
| 11     | &&                                               | 从左向右 |
| 12     | \|\|                                             | 从左向右 |
| 13     | ?:                                               | 从右向左 |
| 14     | =、+=、-=、*=、/=、&=、\|=、^=、~=、«=、»=、>>>= | 从右向左 |



### 流程控制语句

> if else
>
> switch case
>
> while
>
> do…while
>
> for
>
> foreach
>
> ```java
> for(类型 变量名:集合) {
>     语句块;
> }
> ```
>
> break
>
> continue

### 字符串处理

> 不可变的好处
>
> - String的hash值经常被使用, 例如String 用做HashMap的key,不可以变的特性可以使得hash值也不可以变, 因此只需要进行一次计算
> - String Pool的需要
>   - 如果一个String对象已经被创建过了, 那么就会从String Pool中去的应用,只有String是不可以变的, 才可能使用String Pool



#### 直接定义字符串

```java
String str = "hello";
```

#### 使用String类定义

```java
// String()
new String(); // 空字符序列

// String(String original)
String str1 = new String("Hello Java"); // 新建字符串序列
String str2 = new String(str1); // 这可以说是str1的一个副本

// String(char[] value)
char a[] = {'H','e','l','l','0'};
String sChar = new String(a);
a[1] = 's'; // 不会影响sChar的值

// String(char[] value, int offset, int count)
char a[]={'H','e','l','l','o'};
String sChar=new String(a,1,4); // ello
a[1]='s';  // 不会影响sChar的值
```



#### String转换为int

> - Integer.parseInt(str)
> - Integer.valueOf(str).intValue()



#### int转换为String

> - String s = String.valueOf(i);
> - String s = Integer.toString(i);
> - String s = "" + i;



#### String, StringBuffer 和 StringBuilder类区别

> - 可变性
>   - String 不可变
>   - StringBuffer和StringBuilder可变
> - 线程安全
>   - String不可变, so 线程安全
>   - StringBuilder不是线程安全的
>   - StringBuffer 是线程安全, 内部使用synchronized 进行同步



![](https://images.gitee.com/uploads/images/2020/1102/010927_dfdaec78_7984151.png)

### 数字和日期处理

### 内置安装包

### 数组处理

### 类和对象

### 继承和多态

### 异常处理

### 集合、泛型和枚举

### 反射机制

### 输入输出流

### 注解

## 队列

> 队列: 先进先出(First IN First Out, FIFO)
>
> 常用方法: 
>
> - 去队(enqueque)
> - 出队(dequeue)
>
> 入口端口叫队尾(rear)
>
> 出口端口叫队首(front)
>
> 常见使用场景
>
> - 存储多线程中等待排队执行的任务
> - 存储多线程公平说中等待执行任务的线程
> - 常见消息中间件的任务队列等















### 自定义队列

```java
public class MyQueue<E> {

    private Object[] queue; // 存储容器
    private int head; // 头部指针
    private int tail; // 尾部指针
    private int size; // 队列实际存储长度
    private int maxSize; // 最大容量

    public MyQueue() {
        // 无参构造函数，设置默认参数
        this.maxSize = 10;
        this.head = 0;
        this.tail = -1;
        this.size = 0;
        this.queue = new Object[this.maxSize];
    }

    public MyQueue(int initSize) {
        // 有参构造函数，设置参数
        this.maxSize = initSize;
        this.head = 0;
        this.tail = -1;
        this.size = 0;
        this.queue = new Object[this.maxSize];
    }

    /**
     * 查询队头元素
     */
    public E peek() throws Exception {
        if (size == 0) {
            throw new Exception("队列中暂无数据");
        }
        return (E) this.queue[this.head];
    }

    /**
     * 入列
     */
    public boolean offer(E e) throws Exception {
        if (tail >= (maxSize - 1)) {
            throw new Exception("添加失败，队列已满");
        }
        this.queue[++tail] = e;
        size++;
        return true;
    }

    /**
     * 出列
     */
    public E poll() throws Exception {
        if (size == 0) {
            throw new Exception("删除失败，队列为空");
        }
        size--;
        return (E) this.queue[head++];
    }

    /**
     * 代码测试
     */
    public static void main(String[] args) throws Exception {
        MyQueue queue = new MyQueue();
        queue.offer("Hello");
        queue.offer("Java");
        System.out.println(queue.peek());
        queue.poll();
        System.out.println(queue.poll());
    }
}

// 运行结果
Hello
Java
```



自定义队列 - 链表

![](https://images.gitee.com/uploads/images/2020/1023/212153_7c979d68_7984151.png)

```java
public class QueueByLinked {

    /**
     * 声明链表节点
     */
    static class Node<E> {
        E item; // 当前的值
        Node<E> next; // 下一个节点
        Node(E e) {
            this.item = e;
        }
    }

    private Node firstNode; // 队头元素
    private Node lastNode; // 队尾元素
    private int size; // 队列实际存储数量
    private int maxSize; // 队列最大容量

    public QueueByLinked(int maxSize) {
        if (maxSize <= 0) throw new RuntimeException("队列最大容量不能为空");
        // 默认初始化函数
        firstNode = lastNode = new Node(null);
        this.size = 0;
        this.maxSize = maxSize;
    }

    /**
     * 判断队列是否为空
     */
    public boolean isEmpty() {
        return size == 0;
    }

    /**
     * 入列
     */
    public void offer(Object e) {
        // 最大值效验
        if (maxSize <= size) throw new RuntimeException("队列已满");
        Node node = new Node(e);
        lastNode = lastNode.next = node; // 设置最后一个节点和倒数第二个节点的 next
        size++; // 队列数量 +1
    }

    /**
     * 出列
     */
    public Node poll() {
        if (isEmpty()) throw new RuntimeException("队列为空");
        size--; // 队列数量 -1
        return firstNode = firstNode.next; // 设置并返回队头元素（第一个节点是 null，当前元素则为 Node.next）
    }
    
    /**
     * 查询队头元素
     */
    public Node peek() {
        if (isEmpty()) throw new RuntimeException("队列为空");
        return firstNode.next;  // 返回队头元素（第一个节点是 null，当前元素则为 Node.next）
    }

    /**
     * 代码测试
     */
    public static void main(String[] args) {
        QueueByLinked queue = new QueueByLinked(10);
        queue.offer("Hello");
        queue.offer("JDK");
        queue.offer("Java");
        System.out.println(queue.poll().item);
        System.out.println(queue.poll().item);
        System.out.println(queue.poll().item);
    }
}

// 执行结果
Hello
JDK
Java
```



使用List实现自定义队列

> 不建议使用

```java
import java.util.ArrayList;
import java.util.List;

/**
 * 自定义队列（List方式）
 */
public class QueueByList<E> {

    private List value; // 队列存储容器

    public QueueByList() {
        // 初始化
        value = new ArrayList();
    }

    /**
     * 判断队列是否为空
     */
    public boolean isEmpty() {
        return value.size() == 0;
    }

    /**
     * 入列
     */
    public void offer(Object e) {
        value.add(e);
    }

    /**
     * 出列
     */
    public E poll() {
        if (isEmpty()) throw new RuntimeException("队列为空");
        E item = (E) value.get(0);
        value.remove(0);
        return item;
    }

    /**
     * 查询队头元素
     */
    public E peek() {
        if (isEmpty()) throw new RuntimeException("队列为空");
        return (E) value.get(0);
    }

    /**
     * 代码测试
     */
    public static void main(String[] args) {
        QueueByList queue = new QueueByList();
        queue.offer("Hello");
        queue.offer("JDK");
        queue.offer("Java");
        System.out.println(queue.poll());
        System.out.println(queue.poll());
        System.out.println(queue.poll());
    }
}
// 执行结果
Hello
JDK
Java
```



<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f54994efac0d43de9c8a93f0b8ef9f5a~tplv-k3u1fbpfcp-zoom-1.image" alt="image.png" style="zoom:50%;" />

### 阻塞队列和非阻塞队列

阻塞队列（Blocking Queue）提供了可阻塞的 `put` 和 `take` 方法，它们与可定时的 `offer` 和 `poll` 是等价的。如果队列满了 `put` 方法会被阻塞等到有空间可用再将元素插入；如果队列是空的，那么 `take` 方法也会阻塞，直到有元素可用。当队列永远不会被充满时，`put` 方法和 `take` 方法就永远不会阻塞。

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cdec5b9180a446a7b2277cef0944d30d~tplv-k3u1fbpfcp-zoom-1.image)

我们可以从队列的名称中知道此队列是否为阻塞队列，阻塞队列中包含 `BlockingQueue` 关键字，比如以下这些：

- ArrayBlockingQueue
- LinkedBlockingQueue
- PriorityBlockingQueue
- .......

#### 阻塞队列功能演示

接下来我们来演示一下当阻塞队列的容量满了之后会怎样，示例代码如下：

```java
import java.util.Date;
import java.util.concurrent.ArrayBlockingQueue;

public class BlockingTest {
    public static void main(String[] args) throws InterruptedException {
        // 创建一个长度为 5 的阻塞队列
        ArrayBlockingQueue q1 = new ArrayBlockingQueue(5);
        
        // 新创建一个线程执行入列
        new Thread(() -> {
            // 循环 10 次
            for (int i = 0; i < 10; i++) {
                try {
                    q1.put(i);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(new Date() + " | ArrayBlockingQueue Size:" + q1.size());
            }
            System.out.println(new Date() + " | For End.");
        }).start();

        // 新创建一个线程执行出列
        new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                try {
                    // 休眠 1S
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                if (!q1.isEmpty()) {
                    try {
                        q1.take(); // 出列
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }).start();
    }
}
```

以上代码的执行结果如下：

> Mon Oct 19 20:16:12 CST 2020 | ArrayBlockingQueue Size:1
>
> Mon Oct 19 20:16:12 CST 2020 | ArrayBlockingQueue Size:2
>
> Mon Oct 19 20:16:12 CST 2020 | ArrayBlockingQueue Size:3
>
> Mon Oct 19 20:16:12 CST 2020 | ArrayBlockingQueue Size:4
>
> Mon Oct 19 20:16:12 CST 2020 | ArrayBlockingQueue Size:5
>
> Mon Oct 19 20:16:13 CST 2020 | ArrayBlockingQueue Size:5
>
> Mon Oct 19 20:16:14 CST 2020 | ArrayBlockingQueue Size:5
>
> Mon Oct 19 20:16:15 CST 2020 | ArrayBlockingQueue Size:5
>
> Mon Oct 19 20:16:16 CST 2020 | ArrayBlockingQueue Size:5
>
> Mon Oct 19 20:16:17 CST 2020 | ArrayBlockingQueue Size:5
>
> Mon Oct 19 20:16:17 CST 2020 | For End.

从上述结果可以看出，当 `ArrayBlockingQueue` 队列满了之后就会进入阻塞，当过了 1 秒有元素从队列中移除之后，才会将新的元素入列。

#### 非阻塞队列

非阻塞队列也就是普通队列，它的名字中不会包含 `BlockingQueue` 关键字，并且它不会包含 `put` 和 `take` 方法，当队列满之后如果还有新元素入列会直接返回错误，并不会阻塞的等待着添加元素，如下图所示： ![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/20117a9dd30247e8806d56cd60f17307~tplv-k3u1fbpfcp-zoom-1.image) 非阻塞队列的典型代表是 `ConcurrentLinkedQueue` 和 `PriorityQueue`。

### 有界队列和无界队列

**有界队列**：是指有固定大小的队列，比如设定了固定大小的 `ArrayBlockingQueue`，又或者大小为 0 的 `SynchronousQueue`。 ![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2d413d77bccd42db8e130cca8b31114d~tplv-k3u1fbpfcp-zoom-1.image)

**无界队列**：指的是没有设置固定大小的队列，但其实如果没有设置固定大小也是有默认值的，只不过默认值是 Integer.MAX_VALUE，当然实际的使用中不会有这么大的容量（超过 Integer.MAX_VALUE），所以从使用者的角度来看相当于 “无界”的。

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bcc957d5edd54885909031dc2c72bc22~tplv-k3u1fbpfcp-zoom-1.image)

### 按功能分类

接下来就是本文的重点了，我们以功能来划分一下队列，它可以被分为：普通队列、优先队列、双端队列、延迟队列、其他队列等，接下来我们分别来看。

#### 1.普通队列

普通队列（Queue）是指实现了先进先出的基本队列，例如 `ArrayBlockingQueue` 和 `LinkedBlockingQueue`，其中 `ArrayBlockingQueue` 是用数组实现的普通队列，如下图所示：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5f5c609371584b6bbc38ad704b98fadd~tplv-k3u1fbpfcp-zoom-1.image) 而 `LinkedBlockingQueue` 是使用链表实现的普通队列，如下图所示：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6bd0d697eaf741858afe1f2f358c71d4~tplv-k3u1fbpfcp-zoom-1.image)

##### 常用方法

普通队列中的常用方法有以下这些：

- offer()：添加元素，如果队列已满直接返回 false，队列未满则直接插入并返回 true；
- poll()：删除并返回队头元素，当队列为空返回 null；
- add()：添加元素，此方法是对 offer 方法的简单封装，如果队列已满，抛出 IllegalStateException 异常；
- remove()：直接删除队头元素；
- put()：添加元素，如果队列已经满，则会阻塞等待插入；
- take()：删除并返回队头元素，当队列为空，则会阻塞等待；
- peek()：查询队头元素，但不会进行删除；
- element()：对 peek 方法进行简单封装，如果队头元素存在则取出并不删除，如果不存在抛出 NoSuchElementException 异常。

注意：**一般情况下 offer() 和 poll() 方法配合使用，put() 和 take() 阻塞方法配合使用，add() 和 remove() 方法会配合使用，程序中常用的是 offer() 和 poll() 方法，因此这两个方法比较友好，不会报错**。

接下来我们以 `LinkedBlockingQueue` 为例，演示一下普通队列的使用：

```java
import java.util.concurrent.LinkedBlockingQueue;

static class LinkedBlockingQueueTest {
    public static void main(String[] args) {
        LinkedBlockingQueue queue = new LinkedBlockingQueue();
        queue.offer("Hello");
        queue.offer("Java");
        queue.offer("中文社群");
        while (!queue.isEmpty()) {
            System.out.println(queue.poll());
        }
    }
}
复制代码
```

以上代码的执行结果如下：

> Hello
>
> Java
>
> 中文社群

#### 2.双端队列

双端队列（Deque）是指队列的头部和尾部都可以同时入队和出队的数据结构，如下图所示： ![image.png](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="800" height="600"></svg>) 接下来我们来演示一下双端队列 `LinkedBlockingDeque` 的使用：

```java
import java.util.concurrent.LinkedBlockingDeque;

/**
  * 双端队列示例
  */
static class LinkedBlockingDequeTest {
    public static void main(String[] args) {
        // 创建一个双端队列
        LinkedBlockingDeque deque = new LinkedBlockingDeque();
        deque.offer("offer"); // 插入首个元素
        deque.offerFirst("offerFirst"); // 队头插入元素
        deque.offerLast("offerLast"); // 队尾插入元素
        while (!deque.isEmpty()) {
            // 从头遍历打印
            System.out.println(deque.poll());
        }
    }
}
复制代码
```

以上代码的执行结果如下：

> offerFirst
>
> offer
>
> offerLast

#### 3.优先队列

优先队列（PriorityQueue）是一种特殊的队列，它并不是先进先出的，而是优先级高的元素先出队。

优先队列是根据二叉堆实现的，二叉堆的数据结构如下图所示： ![image.png](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="800" height="600"></svg>) **二叉堆分为两种类型：一种是最大堆一种是最小堆。**以上展示的是最大堆，**在最大堆中，任意一个父节点的值都大于等于它左右子节点的值。**

> 因为优先队列是基于二叉堆实现的，因此它可以将优先级最好的元素先出队。

接下来我们来演示一下优先队列的使用：

```java
import java.util.PriorityQueue;

public class PriorityQueueTest {
    // 自定义的实体类
    static class Viper {
        private int id; // id
        private String name; // 名称
        private int level; // 等级

        public Viper(int id, String name, int level) {
            this.id = id;
            this.name = name;
            this.level = level;
        }

        public int getId() {
            return id;
        }

        public void setId(int id) {
            this.id = id;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public int getLevel() {
            return level;
        }

        public void setLevel(int level) {
            this.level = level;
        }
    }
    public static void main(String[] args) {
        PriorityQueue queue = new PriorityQueue(10, new Comparator<Viper>() {
            @Override
            public int compare(Viper v1, Viper v2) {
                // 设置优先级规则（倒序，等级越高权限越大）
                return v2.getLevel() - v1.getLevel();
            }
        });
        // 构建实体类
        Viper v1 = new Viper(1, "Java", 1);
        Viper v2 = new Viper(2, "MySQL", 5);
        Viper v3 = new Viper(3, "Redis", 3);
        // 入列
        queue.offer(v1);
        queue.offer(v2);
        queue.offer(v3);
        while (!queue.isEmpty()) {
            // 遍历名称
            Viper item = (Viper) queue.poll();
            System.out.println("Name：" + item.getName() +
                               " Level：" + item.getLevel());
        }
    }
}
复制代码
```

以上代码的执行结果如下：

> Name：MySQL Level：5
>
> Name：Redis Level：3
>
> Name：Java Level：1

从上述结果可以看出，**优先队列的出队是不考虑入队顺序的，它始终遵循的是优先级高的元素先出队**。

#### 4.延迟队列

延迟队列（DelayQueue）是基于优先队列 `PriorityQueue` 实现的，它可以看作是一种以时间为度量单位的优先的队列，当入队的元素到达指定的延迟时间之后方可出队。

![image.png](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="800" height="600"></svg>)

我们来演示一下延迟队列的使用：

```java
import lombok.Getter;
import lombok.Setter;
import java.text.DateFormat;
import java.util.Date;
import java.util.concurrent.DelayQueue;
import java.util.concurrent.Delayed;
import java.util.concurrent.TimeUnit;

public class CustomDelayQueue {
    // 延迟消息队列
    private static DelayQueue delayQueue = new DelayQueue();
    public static void main(String[] args) throws InterruptedException {
        producer(); // 调用生产者
        consumer(); // 调用消费者
    }

    // 生产者
    public static void producer() {
        // 添加消息
        delayQueue.put(new MyDelay(1000, "消息1"));
        delayQueue.put(new MyDelay(3000, "消息2"));
    }

    // 消费者
    public static void consumer() throws InterruptedException {
        System.out.println("开始执行时间：" +
                DateFormat.getDateTimeInstance().format(new Date()));
        while (!delayQueue.isEmpty()) {
            System.out.println(delayQueue.take());
        }
        System.out.println("结束执行时间：" +
                DateFormat.getDateTimeInstance().format(new Date()));
    }

    static class MyDelay implements Delayed {
        // 延迟截止时间（单位：毫秒）
        long delayTime = System.currentTimeMillis();
        // 借助 lombok 实现
        @Getter
        @Setter
        private String msg;

        /**
         * 初始化
         * @param delayTime 设置延迟执行时间
         * @param msg       执行的消息
         */
        public MyDelay(long delayTime, String msg) {
            this.delayTime = (this.delayTime + delayTime);
            this.msg = msg;
        }

        // 获取剩余时间
        @Override
        public long getDelay(TimeUnit unit) {
            return unit.convert(delayTime - System.currentTimeMillis(), TimeUnit.MILLISECONDS);
        }

        // 队列里元素的排序依据
        @Override
        public int compareTo(Delayed o) {
            if (this.getDelay(TimeUnit.MILLISECONDS) > o.getDelay(TimeUnit.MILLISECONDS)) {
                return 1;
            } else if (this.getDelay(TimeUnit.MILLISECONDS) < o.getDelay(TimeUnit.MILLISECONDS)) {
                return -1;
            } else {
                return 0;
            }
        }
        @Override
        public String toString() {
            return this.msg;
        }
    }
}
复制代码
```

以上代码的执行结果如下：

> 开始执行时间：2020-10-20 20:17:28
>
> 消息1
>
> 消息2
>
> 结束执行时间：2020-10-20 20:17:31

从上述结束执行时间和开始执行时间可以看出，消息 1 和消息 2 都正常实现了延迟执行的功能。

#### 5.其他队列

在 Java 的队列中有一个比较特殊的队列 `SynchronousQueue`，它的特别之处在于它内部没有容器，每次进行 `put()` 数据后（添加数据），必须等待另一个线程拿走数据后才可以再次添加数据，它的使用示例如下：

```java
import java.util.concurrent.SynchronousQueue;

public class SynchronousQueueTest {

    public static void main(String[] args) {
        SynchronousQueue queue = new SynchronousQueue();

        // 入队
        new Thread(() -> {
            for (int i = 0; i < 3; i++) {
                try {
                    System.out.println(new Date() + "，元素入队");
                    queue.put("Data " + i);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

            }
        }).start();

        // 出队
        new Thread(() -> {
            while (true) {
                try {
                    Thread.sleep(1000);
                    System.out.println(new Date() + "，元素出队：" + queue.take());
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }
}
复制代码
```

以上代码的执行结果如下：

> Mon Oct 19 21:00:21 CST 2020，元素入队
>
> Mon Oct 19 21:00:22 CST 2020，元素出队：Data 0
>
> Mon Oct 19 21:00:22 CST 2020，元素入队
>
> Mon Oct 19 21:00:23 CST 2020，元素出队：Data 1
>
> Mon Oct 19 21:00:23 CST 2020，元素入队
>
> Mon Oct 19 21:00:24 CST 2020，元素出队：Data 2

从上述结果可以看出，当有一个元素入队之后，只有等到另一个线程将元素出队之后，新的元素才能再次入队。

### 总结

本文讲了 Java 中的 5 种队列：普通队列、双端队列、优先队列、延迟队列、其他队列。其中普通队列的典型代表为 `ArrayBlockingQueue` 和 `LinkedBlockingQueue`，双端队列的代表为 `LinkedBlockingDeque`，优先队列的代表为 `PriorityQueue`，延迟队列的代表为 `DelayQueue`，最后还讲了内部没有容器的其他队列 `SynchronousQueue`。

> 文末福利：搜索公众号「Java中文社群」发送“面试”，领取最新的面试资料。



