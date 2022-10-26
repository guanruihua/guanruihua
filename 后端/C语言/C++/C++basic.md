---
title: C++basic
date: 2020-11-11 22:52:28
tags: 
- C++
- basic
---

# C++basic

## 前言

> - C++ 是一种中级语言，它是由 Bjarne Stroustrup 于 1979 年在贝尔实验室开始设计开发的。
> - C++ 进一步扩充和完善了 C 语言，是一种面向对象的程序设计语言。
>- C++ 可运行于多种平台上，如 Windows、MAC 操作系统以及 UNIX 的各种版本。
> - 源码文件通用使用拓展名.cpp, .cp 或 .c

### 四大特性 

>   - 封装
>   - 抽象
>   - 继承
>   - 多态

### 标准库



> - 三个重要部分组成
>   - 核心语言，提供了所有构件块，包括变量、数据类型和常量，等等。
>   - C++ 标准库，提供了大量的函数，用于操作文件、字符串等。
>   - 标准模板库（STL），提供了大量的方法，用于操作数据结构等。



## 基础

> - **对象 -** 对象具有状态和行为。例如：一只狗的状态 - 颜色、名称、品种，行为 - 摇动、叫唤、吃。对象是类的实例。
> - **类 -** 类可以定义为描述对象行为/状态的模板/蓝图。
> - **方法 -** 从基本上说，一个方法表示一种行为。一个类可以包含多个方法。可以在方法中写入逻辑、操作数据以及执行所有的动作。
> - **即时变量 -** 每个对象都有其独特的即时变量。对象的状态是由这些即时变量的值创建的。

```c++
#include <iostream> // 定义头文件
using namespace std; // 告诉编译器使用std命名空间
 
// main() 是程序开始执行的地方
 
int main()
{
   cout << "Hello World"; // 输出 Hello World
   return 0;
}
```

### 关键词

| asm          | else      | new              | this     |
| ------------ | --------- | ---------------- | -------- |
| auto         | enum      | operator         | throw    |
| bool         | explicit  | private          | true     |
| break        | export    | protected        | try      |
| case         | extern    | public           | typedef  |
| catch        | false     | register         | typeid   |
| char         | float     | reinterpret_cast | typename |
| class        | for       | return           | union    |
| const        | friend    | short            | unsigned |
| const_cast   | goto      | signed           | using    |
| continue     | if        | sizeof           | virtual  |
| default      | inline    | static           | void     |
| delete       | int       | static_cast      | volatile |
| do           | long      | struct           | wchar_t  |
| double       | mutable   | switch           | while    |
| dynamic_cast | namespace | template         |          |



### 数据结构

| 类型     | 关键字                                |
| :------- | :------------------------------------ |
| 布尔型   | bool                                  |
| 字符型   | char                                  |
| 整型     | int                                   |
| 浮点型   | float                                 |
| 双浮点型 | double                                |
| 无类型   | void                                  |
| 宽字符型 | wchar_t(`typedef short int wchar_t;`) |

| 类型               | 位            | 范围                                                         |
| :----------------- | :------------ | :----------------------------------------------------------- |
| char               | 1 个字节      | -128 到 127 或者 0 到 255                                    |
| unsigned char      | 1 个字节      | 0 到 255                                                     |
| signed char        | 1 个字节      | -128 到 127                                                  |
| int                | 4 个字节      | -2147483648 到 2147483647                                    |
| unsigned int       | 4 个字节      | 0 到 4294967295                                              |
| signed int         | 4 个字节      | -2147483648 到 2147483647                                    |
| short int          | 2 个字节      | -32768 到 32767                                              |
| unsigned short int | 2 个字节      | 0 到 65,535                                                  |
| signed short int   | 2 个字节      | -32768 到 32767                                              |
| long int           | 8 个字节      | -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807      |
| signed long int    | 8 个字节      | -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807      |
| unsigned long int  | 8 个字节      | 0 到 18,446,744,073,709,551,615                              |
| float              | 4 个字节      | 精度型占4个字节（32位）内存空间，+/- 3.4e +/- 38 (~7 个数字) |
| double             | 8 个字节      | 双精度型占8 个字节（64位）内存空间，+/- 1.7e +/- 308 (~15 个数字) |
| long double        | 16 个字节     | 长双精度型 16 个字节（128位）内存空间，可提供18-19位有效数字。 |
| wchar_t            | 2 或 4 个字节 | 1 个宽字符                                                   |

#### typedef声明



> 给已有的类型取一个新的名字

```c++
#include <iostream>
using namespace std;
typedef int newname;
int main()
{
   	newname a = 1;
    cout << a << endl;  
    return 0;  
}

// 1 
```



#### 枚举类型



```c++
enum 枚举名{ 
     标识符[=整型常数], 
     标识符[=整型常数], 
... 
    标识符[=整型常数]
} 枚举变量;
```



eg: 

```c++
enum color { red, green, blue } c;
c = blue;// 2

// 可以自己赋值
enum color { red, green=5, blue };
```



##### 初始值

```c++
enum week{ Mon = 1, Tues, Wed, Thurs = 6 , Fri, Sat, Sun } day;
day = Wed ; // 3
day = Fri ; // 7
```

> - 默认值(但是前面的值没有赋值的情况下, 有则是往后加一)
>   - 第一个名称为0
>   - 第二个名称为1
>   - 以此类推

https://www.runoob.com/cplusplus/cpp-variable-types.html

http://c.biancheng.net/view/1367.html