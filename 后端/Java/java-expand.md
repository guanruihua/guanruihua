---
title: java-expand
date: 2020-11-01 16:29:04
tags:
- java
- expand
---

# java-expand

## JVM 虚拟内存分布

> 借鉴https://www.cnblogs.com/syp172654682/p/8082625.html

<img src="https://images.gitee.com/uploads/images/2020/1101/171425_ce5ca706_7984151.png" style="zoom:50%;align:center;" />

> - **程序计数器**是jvm执行程序的流水线，存放一些跳转指令。
>
> - **本地方法栈**是jvm调用操作系统方法所使用的栈。
>
> - **虚拟机栈**是jvm执行java代码所使用的栈。
>
> - **方法区**存放了一些常量、静态变量、类信息等，可以理解成class文件在内存中的存放位置。
>
> - **虚拟机堆**是jvm执行java代码所使用的堆。





### 常量池

> 分为 : **静态常量池** 和 **运行时常量池**
>
> 可以通过jvm参数`-XX:PermSize、-XX:MaxPermSize来设置方法区大小`, 从而间接显示常量池的大小

#### 静态常量池

> - `*.class`文件中的常量池
>   - `*.class`文件中的常量池不仅仅包含字符串(数字) 字面量, 还包含类, 方法的信息, 常用class文件绝大部分空间, 改常量池主要涌入存放两大类常量: `字面量(Literal) `和` 符号引用量( Symbolic References )`
>     - 字面量: 相当于Java语言层面的概念, 入文本字符串, 声明为final的常量值等
>     - 符号引用量: 属于编译原理方面的概念,包含了如下三种类型的常量:
>       - 类和接口的全限定名
>       - 字段名称和描述符
>       - 方法名称和描述符



#### 运行常量池

> - 这是JVM虚拟机在完成类装载操作后, 将class文件中的常量池载入到内存中, 并保存到方法区中, 我们常说的常量池, 就是指方法去中的运行时常量池
> - 运行时常量池相对于CLass文件常量池的另外一个重要特征是**具备动态性**，Java语言并不要求常量一定只有编译期才能产生，也就是并非预置入CLass文件中常量池的内容才能进入方法区运行时常量池，运行期间也可能将新的常量放入池中，这种特性被开发人员利用比较多的就是**String类的intern()**方法。
> - String的`intern()`方法会查找在常量池中是否存在一份equal相等的字符串,如果有则返回该字符串的引用,如果没有则添加自己的字符串进入常量池

##### 常量池的好处

> - 避免频繁的创建和销毁对象而影响系统性能, 其实现了对象的共享
> - 节约内存空间: 常量池中所有相同的字符串长阿林被合并, 只占用一个空间
> - 节约运行时间: 比较字符串是, == 比 equals() 快, 对于两个引用变量, 只用 == 判断引用是否相等, 也就是判断实际值是否相等

```java

		String s1 = "hello";
		String s2 = "hello";
		String s3 = "hel" + "lo";
		String s4 = "hel" + new String("lo");
		String s5 = new String("hello");
		String s6 = s5.intern();
		String s7 = "H";
		String s8 = "ello";
		String s9 = s7 + s8;
		
		System.out.println( s1 == s2 ); // true
		/**
		 s1 和  s2 在赋值的时候, 均使用字符串字面量,
		  在编译期间, 这种字面量会直接放入class文件的常量池中, 而实现复用
		  载入运行时常量池后, s1, s2 指向同一个内存地址, 所以相等
		 */
		System.out.println( s1 == s3 ); // true
		/*
		 s3 虽然是动态拼接出来的字符串, 但是所有参与拼接的部分都是已知的字面量, 
		 在编译期间, 这种拼接会被优化, 编译器直接帮你拼接好
		 
		 只有使用引号包括的文本的方式创建的String对象之间使用"+"连接产生新的对象才会被加入字符串池中
		 */
		System.out.println( s1 == s4 ); // false
		/*
		 s4有一部分不是已知字面量, 编译器不会优化, 必须等到运行时才可以确定结果, 
		 给字符串不变定理, 不确定s4被分配到哪里, 所以地址不相同
		 
		 对于所有包含new方式新建对象(包括null) 的"+ " 连接表达式, 他所产生的新对象都不会被加入字符串池中
		 */
		System.out.println( s1 == s9 ); // false
		/**
		 	s7, s8 作为两个变量, 都是不可以预料的, 都是不可以预料的, 
		 	在编译期被确认, 所以不做优化, 只能等到运行时, 在堆中创建
		 */
		System.out.println( s4 == s5 ); // false
		//  两个都在堆中, 地址不相同
		System.out.println( s1 == s6 ); // true
		//  intern() 会尝试将Hello字符串添加到常量池中，并返回其在常量池中的地址，
		// 因为常量池中已经有了Hello字符串，所以intern方法直接返回地址； 故地址相同
		
		
```

特例1

```java
public static final String A = "ab"; // 常量A
public static final String B = "cd"; // 常量B
public static void main(String[] args) {
     String s = A + B;  // 将两个常量用+连接对s进行初始化 
     String t = "abcd";   
   System.out.println(s == t);//true 
 } 
// A 和 B 都是常量, 值是固定的, 因此s的值也是固定的, 他在编译时就已确认, 也就是String s = A + B; 等同于 String s = "ab" + "cd";
```

特例2

```java
public static final String A; // 常量A
public static final String B;    // 常量B
static {   
     A = "ab";   
     B = "cd";   
 }   
 public static void main(String[] args) {   
    // 将两个常量用+连接对s进行初始化   
     String s = A + B;   
     String t = "abcd";   
     System.out.println(s == t);//false
 } 
```

> A 和 B 虽然被定义为常量,  但是它们都没有被马上复制, 在运算上s的值之前, 它们合适被赋值, 以及被赋予什么样的值, 都是变数, 因此A和B在被赋值之前, 性质类型于与变量, 那么s就不能在编译期被确认, 只能在运行时被创建

#### 缓存池

> - new Integer(123) 每次都会新建一个对象；
> - Integer.valueOf(123) 会使用缓存池中的对象，多次调用会取得同一个对象的引用。
> - integer缓存池大小默认为-128~127

```java
Integer x = new Integer(123);
Integer y = new Integer(123);
System.out.println(x == y);    // false
Integer z = Integer.valueOf(123);
Integer k = Integer.valueOf(123);
System.out.println(z == k);   // true
```











## 包装类和基本类型

```java
int i = 128;
Integer i2 = 128;
Integer i3 = new Integer(128);
System.out.println(i == i2); //Integer会自动拆箱为int，所以为true
System.out.println(i == i3); //true，理由同上

Integer i4 = 127;//编译时被翻译成：Integer i4 = Integer.valueOf(127);
Integer i5 = 127;

System.out.println(i4 == i5);

Integer i6 = 128;
Integer i7 = 128;
System.out.println(i6 == i7);

Integer i8 = new Integer(127);
System.out.println(i5 == i8); //false

Integer i9 = new Integer(128);
Integer i10 = new Integer(123);
System.out.println(i9 == i10);  //false
			
int i11 = 11;
int i12 = 11;
System.out.println(i11 == i12); // true
```

> i4 和 i5 为true 的原因: 
>
> - -128~127之间的数, 会进行缓存,
> - Interger i4 =127 时, 会将127进行缓存
> - Interger i5 = 127 时, 会直接在缓存中去, 就不会再new一个对象
> - 所以i4和i5是true, i6和i7为false
>
> 总结: 
>
> - 两个都是new出来的, 都是false,地址不同
> - int和Interger(无论new否)比, 都是true, 因为Interger自动猜想为int再去比