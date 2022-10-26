---
title: lambda
date: 2021-02-15 00:01:25
tags: 
- lambda
- java
---

# Lambda

> - `Lambda` 表达式，也可称为`闭包`，是一个`匿名函数`。
>
> - 可以把 Lambda 表达式理解为是`一段可传递的代码`（像数据一样传递）。即 Lambda 允许把`函数`作为一个方法的`实参参数`（函数当作参数传递到方法中）。
> - `Java8`的其中一个很重要的新特性。

```java
public class LambdaDemo {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("A", "B", "C");
        //x -> System.out.println(x)，就是一个匿名函数,即Lambda表达式，作为实参传给dealList方法
        dealList(list, x -> System.out.println(x));
    }
	
    public static void dealList(List<String> list, Consumer<String> consumer) {
    	// 遍历list中的每一个元素，传给consumer对象的accept函数，进行调用
        for (String x : list) {
            consumer.accept(x);
        }
    }
}
```



> - Lambda 表达式是对某些接口的简单实现，但不是所有接口都可以使用 Lambda 表达式来实现的，Lambda 规定能被 Lambda 表达式实现的接口中，它`只能只有一个需要被实现的方法（函数）`，但`不要求接口中只能只有一个方法。`
> - 因为Java8中有另外一个新特性，即 `default` 关键字修饰的接口方法有默认实现，这个默认的方法是可以不需要子类实现的，可使用`@FunctionalInterface`注解来强制使接口只能有一个需要被实现的方法。

```java
// 此注解表明此接口为函数接口，即只能有一个抽象方法
@FunctionalInterface
public interface Human {

    // 抽象方法，需要被实现
    void eat(String name);

    // default修饰的默认方法，不需要被子类实现
    default void run() {
        System.out.println("I can run...");
    }
}

public static void main(String[] args) {
    Human human = x -> System.out.println(x + " is eat");
    human.eat("Mr.nobody");
    human.run();
}
// 输出结果
Mr.nobody is eat
I can run...
```



## 案例一



```java
// 接口
public interface Human {
	// 抽象方法，需要被实现
    void speak();
}
```





### 通过类来实现接口

> 并且实现里面的抽象方法

```java
// 实现类，并且实现抽象方法
public class Man implements Human {
    @Override
    public void speak() {
        System.out.println("I am man!");
    }
}

public class Main {
    public static void main(String[] args) {
        // 方式一 直接编写实现类
        Human human = new Man();
        human.speak();
    }
}

```

### 匿名内部类

```java
public class Main {
    public static void main(String[] args) {
        // 方式一 直接编写实现类
        Human human = new Man();
        human.speak();

        // 方式二 匿名内部类
        Human human1 = new Human() {
            @Override
            public void speak() {
                System.out.println("I am woman!");
            }
        };
        human1.speak();
    }
}

```

> 但方式二中，有用的就只有`System.out.println("I am woman!");`这一行，所以有了Lambda表达式，可以这样写，如方式三：

```java
public class Main {
    public static void main(String[] args) {
        // 方式一 直接编写实现类
        Human human = new Man();
        human.speak();
        
        // 方式二 匿名内部类
        Human human1 = new Human() {
            @Override
            public void speak() {
                System.out.println("I am woman!");
            }
        };
        human1.speak();
        
        // 方式三 Lambda表达式
        Human human2 = () -> System.out.println("I am woman!");
        human2.speak();
    }
}

```

## 案例二

> 再假如我们要对一个Student类的数组按指定条件进行过滤，如下：

```java
public class Student {
    private String name;
    private int age;
    private double score;
}

public class Main {

    public static void main(String[] args) {

        List<Student> students = Arrays.asList(new Student("张三", 18, 89.5),
                new Student("李四", 20, 60), new Student("王五", 19, 100), new Student("赵六", 22, 89));

        // 过滤出年龄大于等于20的学生
        List<Student> stus1 = filterStudentByAge(students);
        System.out.println(stus1);

        // 过滤出成绩大于80的学生
        List<Student> stus2 = filterStudentByScore(students);
        System.out.println(stus2);
    }

    // 过滤出年龄大于等于20的学生
    private static List<Student> filterStudentByAge(List<Student> students) {
        List<Student> stus = new ArrayList<>();
        for (Student stu : students) {
            if (stu.getAge() >= 20) {
                stus.add(stu);
            }
        }
        return stus;
    }

    // 过滤出成绩大于80的学生
    private static List<Student> filterStudentByScore(List<Student> students) {
        List<Student> stus = new ArrayList<>();
        for (Student stu : students) {
            if (stu.getScore() > 80) {
                stus.add(stu);
            }
        }
        return stus;
    }
}

```

> 按上面的方式，如果要按另外一个条件过滤呢，又要写一个方法。那可以用策略模式处理，编写一个抽象策略接口，然后编写多个不同策略类实现它。

```java
// 策略接口
public interface MyPredicate<T> {
    boolean test(T t);
}

// 过滤出年龄大于等于20的学生
public class filterStudentByAge implements MyPredicate<Student> {
    @Override
    public boolean test(Student t) {
        return t.getAge() >= 20;
    }
}

// 过滤出成绩大于80的学生
public class filterStudentByScore implements MyPredicate<Student> {
    @Override
    public boolean test(Student t) {
        return t.getScore() > 80;
    }
}

public class Main {

    public static void main(String[] args) {

        List<Student> students = Arrays.asList(new Student("张三", 18, 89.5),
                new Student("李四", 20, 60), new Student("王五", 19, 100), new Student("赵六", 22, 89));

        // 过滤出年龄大于等于20的学生
        List<Student> stus1 = filterStudent(students, new FilterStudentByAge());
        System.out.println(stus1);

        // 过滤出成绩大于80的学生
        List<Student> stus2 = filterStudent(students, new FilterStudentByScore());
        System.out.println(stus2);
    }

    // 按myPredicate策略过滤出满足条件的学生
    private static List<Student> filterStudent(List<Student> students,
            MyPredicate<Student> myPredicate) {
        List<Student> stus = new ArrayList<>();
        for (Student stu : students) {
            if (myPredicate.test(stu)) {
                stus.add(stu);
            }
        }
        return stus;
    }
}

```

> 但是以上方式，每增加一个过滤条件，就要编写一个策略类，太麻烦。所以我们就用匿名内部类方式。

```java
// 匿名内部类形式 过滤出年龄大于等于18的学生
List<Student> stus3 = filterStudent(students, new MyPredicate<Student>() {
    @Override
    public boolean test(Student t) {
        return t.getAge() > 18;
    }
});
System.out.println(stus3);

```

> 但我们会觉得匿名内部类还是太麻烦，无用代码太多，有用的代码其实就只有`return t.getAge() > 18;`，于是 Lambda 表达式发挥的作用就来了：

```java
// Lambda形式 过滤出年龄大于等于18的学生
List<Student> stus4 = filterStudent(students, t -> t.getAge() > 18);
System.out.println(stus4);

```

> 这时还是有人会问，那我们定义的`接口MyPredicate`和`方法filterStudent()`，好像没什么作用呀。然而官方已经想到这一点，它内置了一些通用接口，我们可以使用它。例如断言的接口 `Predicate`，那我们就用如下方式，完全不用写`接口MyPredicate`和`方法filterStudent()`，如下：

```java
// 按myPredicate策略过滤出满足条件的学生
private static List<Student> filterStudent(List<Student> students,
        Predicate<Student> predicate) {
    List<Student> stus = new ArrayList<>();
    for (Student stu : students) {
        if (predicate.test(stu)) {
            stus.add(stu);
        }
    }
    return stus;
}

```

> 当然，如果你会使用Stream（可以看我另外一篇文章），只需要写下面的代码，如下：

```java
public class Main {

    public static void main(String[] args) {

        List<Student> students = Arrays.asList(new Student("张三", 18, 89.5),
                new Student("李四", 20, 60), new Student("王五", 19, 100), new Student("赵六", 22, 89));

        // 过滤出年龄大于等于20的学生
        students.stream().filter(t -> t.getAge() >= 20).forEach(System.out::println);
        
        System.out.println("-------------------------------------");

        // 过滤出成绩大于80的学生
        students.stream().filter(t -> t.getScore() > 80).forEach(System.out::println);
    }
}

```

## Lambda 语法

> 语法：() -> {} ()：Lambda的形参列表，也就是接口里面那个抽象方法的形参列表。 ->：Lambda的操作符，可以理解为参数和Lambda体的分隔符。 {}：实现了接口中的抽象方法的方法体。

> 我们还是以一个简单的例子，由浅到深学习 Lambda 语法。按照语法，我们可以写出如下 Lambda 表达式，`(String name, int age)` 是参数列表，`->` 是分隔符，`{}` 中的代码是方法体。

```java
// 函数接口
@FunctionalInterface
public interface Human {
    // 抽象方法，需要被实现
    String speak(String name, int age);
}

public class LambdaDemo {
    public static void main(String[] args) {
        Human human = (String name, int age) -> {
            System.out.println("My name is " + name + " ,I am " + age + " years old.");
            return name;
        };
        human.speak("Mr.nobody", 18);
    }
}

```

> 当然，`()` 括号内的`参数类型还能省略`（推荐）。

```java
public class LambdaDemo {
    public static void main(String[] args) {
        Human human = (name, age) -> {
            System.out.println("My name is " + name + " ,I am " + age + " years old.");
            return name;
        };
        human.speak("Mr.nobody", 18);
    }
}

```

> 如果是`只有一个参数，() 也能省略`。

```java
@FunctionalInterface
public interface Human {
    // 抽象方法，需要被实现
    String speak(String name);
}

public class LambdaDemo {
    public static void main(String[] args) {
        Human human = name -> {
            System.out.println("My name is " + name + ".");
            return name;
        };
        human.speak("Mr.nobody");
    }
}

```

> 如果，方法体 `{}` 中，只有一行语句，{} 也能省略（推荐）。

```java
@FunctionalInterface
public interface Human {
    // 抽象方法，需要被实现
    void speak(String name, int age);
}

public class LambdaDemo {
    public static void main(String[] args) {
        Human human = (name, age) -> System.out
                .println("My name is " + name + " ,I am " + age + " years old.");
        human.speak("Mr.nobody", 18);
    }
}

```

> 如果`方法体需要返回值`，而且`只有一行语句`，那 `{}` 大括号和 `return` 关键字都可以省略（推荐）。

```java
@FunctionalInterface
public interface Human {
    // 抽象方法，需要被实现
    String speak(String name, int age);
}

public class LambdaDemo {
    public static void main(String[] args) {
        Human human = (name, age) -> "My name is " + name + " ,I am " + age + " years old.";
        human.speak("Mr.nobody", 18);
    }
}
```

