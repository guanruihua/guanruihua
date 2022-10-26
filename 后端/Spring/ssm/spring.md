---
title: Spring 
date: 2020-09-08 21:33:25 
tags:  
- spring 
- java 
- back-end 
---

# Spring

## 前言

> -  Spring是一个开源框架
> - Spring 为简化企业级应用开发而生. 使用Spring可以使简单的JavaBean实现以前只有EJB才能实现的功能
> - Spring 是JavaSE/EE的一站式框架

## 优点

- 方便解耦, 简化开发

  - Spring就是一个大工厂,可以将所有对象创建和依赖关系维护, 交给Spring管理

- AOP变成的支持

  - Spring提供蜜蜡线切面编程, 可以方便的实现对程序进行权限拦截、运行监控等功能

- 声明式事务的支持

  - 只需要通过配置就可以完成对事务的管理, 而无需手动编程

- 方便程序的测试

  - Spring对Junit4支持, 可以通过注解方便的测试Spring程序

- 方便继承各种优秀框架

  - Spring不排斥各种优秀的开源框架,其内部提供对各种优秀的框架(Struts、Hibernate、MyBatis等）的直接支持

- 降低JavaEE API的使用难度

  - Spring对JavaEE开发中非常难用的一些API（JDBC、JavaMail、远程调用等），都提供了封装，使这些API应用难度大大降低

  

  ## 模块

  <img src="https://images.gitee.com/uploads/images/2020/0711/133424_2ed6aec0_6545143.png" style="zoom:25%;" />

  

## Spring IOC的底层原理

## 导入Spring核心开发包到创建工程

> commons-logging-xxx.jar
>
> spring-beans-x.x.x.RELEASE.jar
>
> spring-context-x.x.x.RELEASE.jar
>
> spring-core-x.x.x.RELEASE.jar
>
> spring-expression-x.x.x.RELEASE.jar

pom.xml
```xml
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>1.2.17</version>
</dependency>

<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-core</artifactId>
  <version>4.2.4.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context</artifactId>
  <version>4.2.4.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-beans</artifactId>
  <version>4.2.4.RELEASE</version>
</dependency>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-expression</artifactId>
  <version>4.2.4.RELEASE</version>
</dependency>
```

## 概念

>  Spring IOC
>
> - IOC Inverse of Control 反转控制的概念, 就是将原本在程序中手动创建UserService对象的控制权, 交由Spring框架管理
> - 就是将创建UserService对象控制权反转到Spring框架
> - DI Dependency Injection **依赖注入**的概念, 就是在创建这个对象的过程中, 将这个对象所依赖的属性注入进去


### Bean 使用注解开发

> id和name的作用区别 : name可以使用特殊字符

使用前 需要添加的配置 

添加前

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
	// 配置bean
  <bean id = "idName" class= "位置"></bean>
</beans>
```

添加后

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context  
                           http://www.springframework.org/schema/context/spring-context.xsd">
    <!-- 开启注释 -->
    <context:annotation-config/>
    <!-- 注释的位置 -->
    <context:component-scan base-package="com.tutorialspoint.beans" />

</beans>
```

```java
// 这里的Beans.xml 也可以写成相对路径
ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
HelloSpring helloSpring = (HelloSpring) context.getBean("helloSpring");
//通过 获取bean的实例来初始化, 该方法获取bean不需要设置bean的id
// HelloSpring helloSpring = (HelloSpring) context.getBean(HelloSpring.class); 
helloSpring.setName("Spring add");
System.out.println(helloSpring);
```


#### 注释和标签的对比

| bean.xml                      | 注解                             | 描述       |
| ----------------------------- | -------------------------------- | ---------- |
| id = "idName"                 | @Component("idName")             | bean ID    |
| lazy-init="true"              | @Lazy                            | 延迟加载   |
| scope="singleton"             | @Scope("singleton")              |            |
| init-method="functionName"    | @PostConstruct // 放在function前 | bean创建时 |
| destroy-method="functionName" | @PreDestroy // 放在function 前   | bean销毁时 |




#### Bean 的初始化和销毁前后调用方法

1. interface

   - `implements InitializingBean, DisposableBean`

   - 然后实现里面的两个方法

2. 注解

   - 在Bean中里面的方法前分别添加`@PostConstruct `和 `@PreDestory`

3. xml

   - 先在bean添加方法
   - 再在xml中, bean中init-method和destory-method注册





补充

Scope 的五种取值

- singleton:　单例模式, 在整个Spring IOC 容器中只会创建一个实例. 默认即为单例模式

- prototype:  原型模式, 每次通过`getBean`方法获取实例, 都会创建一个新的实例

- request:  在同一次`http`请求内, 只会产生一个实例 ( Web 应用 )

- session:  在同一词`http`请求内, 只会产生一个实例 ( Web 应用 )

- global session: 映射到porlet的global范围的session, 如果是普通web项目施使用, 会当做普通的session ( 在基于porlet 的web应用程序 )




```java
/**
     * Spring的方式实现
     */
    @Test
    public void demo2(){
        //创建Spring的公厂
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //通过工厂获得类
        UserService userService = (UserService) applicationContext.getBean("userService");
        userService.sayHello();
    }

    @Test
    /**
     * 读取磁盘系统中的配置文件
     */
    public void demo3(){
        //创建Spring的工厂类
        //读取c盘的配置文件
        ApplicationContext applicationContext = new FileSystemXmlApplicationContext("c:\\applicationContext.xml");
        UserService userService = (UserService) applicationContext.getBean("userService");
        userService.sayHello();
    }

    @Test
    /**
     * 传统方式的工厂类: BeanFactory
     */
    public void demo4(){
        BeanFactory beanFactory = new XmlBeanFactory(new ClassPathResource("applicationContext.xml"));
        UserService userService = (UserService) beanFactory.getBean("userService");
        userService.sayHello();
    }

    @Test
    /**
     * 传统方式的工厂类: BeanFactory
     */
    public void demo5(){
        BeanFactory beanFactory = new XmlBeanFactory(new FileSystemResource("C:\\applicationContext.xml"));
        UserService userService = (UserService) beanFactory.getBean("userService");
        userService.sayHello();
    }
```

## Bean

### 三种实例化Bean的方式

#### 类构造器实例化(默认无参数)

applicationContext.xml

```xml
<!--第一种: 无参构造器的方式-->
    <bean id="bean1" class="com.ioc.demo2.Bean1"></bean>
```

javaclass
```java
 
public void demo1(){
        //创建工厂
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //通过工厂类获得类的实例
        Bean1 bean1 = (Bean1) applicationContext.getBean("bean1");

    }
```

#### 静态工厂方法实例化(简单工厂模式)

applicationContext.xml

```xml
<!--第二种: 静态工厂的方式-->
    <bean id="bean2" class="com.ioc.demo2.Bean2Factory" factory-method="createBean2"></bean>
```

javaclass
```java
 
public void demo1(){
        //创建工厂
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //通过工厂类获得类的实例
        Bean1 bean1 = (Bean1) applicationContext.getBean("bean1");

    }
```

DEMO  

PetFactory.java

```java
public class PetFactory {
    public Ipet getPet( String type){
        if("dog".equals(type)){
            return new Dog();
        } else if( "parrot".equals(type)){
            return  new Parrot();
        } else {
            throw new IllegalArgumentException("pet type is illegal!");
        }
    }
}
```

factoryBean.xml 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="petFactory" class="com.tutorialspoint.impl.PetFactory"></bean>
    <bean id="dog" factory-bean="petFactory" factory-method="getPet">
        <constructor-arg value="dog"></constructor-arg>
    </bean>

    <bean id="parrot" factory-bean="petFactory" factory-method="getPet">
        <constructor-arg value="parrot"></constructor-arg>
    </bean>
</beans>
```
Test文件
```java
@Test
public void FactoryTest1(){
  Resource resource = new ClassPathResource("factoryBeans.xml");
  BeanFactory factory = new DefaultListableBeanFactory();
  BeanDefinitionReader beanDefinitionReader = new XmlBeanDefinitionReader((BeanDefinitionRegistry) factory);
  beanDefinitionReader.loadBeanDefinitions(resource);

  Dog dog = (Dog) factory.getBean("dog");
  Parrot parrot = (Parrot) factory.getBean("parrot");

  dog.move();
  parrot.move();
}

log:
Dog can run!
Parrot can fly!
```

#### 使用实例工厂方法实例化(工厂方法模式)

```xml
<!--第三种: 实例工厂的方式-->
    <bean id="bean3Factory" class="com.ioc.demo2.Bean3Factory"></bean>
    <bean id="bean3" factory-bean="bean3Factory" factory-method="createBean3"/>
```
javaclass
```java
 
public void demo1(){
        //创建工厂
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        //通过工厂类获得类的实例
        Bean1 bean1 = (Bean1) applicationContext.getBean("bean1");

    }
```

DEMO

PetFactory.java

```java
public class PetFactory {
    public static Ipet getPet( String type){
        if("dog".equals(type)){
            return new Dog();
        } else if( "parrot".equals(type)){
            return  new Parrot();
        } else {
            throw new IllegalArgumentException("pet type is illegal!");
        }
    }
}
```

factoryBean.xml 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dog" class="com.tutorialspoint.impl.StaticPetFactory" factory-method="getPet">
        <constructor-arg value="dog"></constructor-arg>
    </bean>

    <bean id="parrot" class="com.tutorialspoint.impl.StaticPetFactory" factory-method="getPet">
        <constructor-arg value="parrot"></constructor-arg>
    </bean>
</beans>
```



Test文件

```java
@Test
public void FactoryTest1(){
  Resource resource = new ClassPathResource("factoryBeans.xml");
  BeanFactory factory = new DefaultListableBeanFactory();
  BeanDefinitionReader beanDefinitionReader = new XmlBeanDefinitionReader((BeanDefinitionRegistry) factory);
  beanDefinitionReader.loadBeanDefinitions(resource);

  Dog dog = (Dog) factory.getBean("dog");
  Parrot parrot = (Parrot) factory.getBean("parrot");

  dog.move();
  parrot.move();
}

log:
Dog can run!
Parrot can fly!
```



#### 小结
##### 调用实例工厂方法和调用静态工厂方法创建 Bean 的异同
区别如下：
- 配置实例工厂方法创建 Bean,必须将实例工厂配置成 Bean 实例；而配置静态工厂方法创建 Bean,则无需配置工厂 Bean;
- 配置实例工厂方法创建 Bean,必须使用 `factory-bvean` 属性确定工厂 Bean; 而配置静态工厂方法创建 Bean,则使用 `class` 属性确定静态工厂类。
  相同之处如下：
- 都需要使用 `factory-method` 指定生产 Bean 实例的工厂方法；
- 工厂方法如果需要参数，都使用 `<constructor-arg.../>` 元素指定参数值；
- 普通的设值注入，都使用 `<property.../>`元素确定参数值。



### Bean的作用域

<img src="https://images.gitee.com/uploads/images/2020/0802/144827_2c930c4b_6545143.png" style="zoom:50%;" />

### Bean的生命周期

- 第一步:MAN被实例化了..., instantiate bean对象实例化
- 第二步:设置属性..., populate properties 封装属性
- 第三步,设置Bean的名称man, 如果Bean实现了BeanNameAware 执行 setBeanName
- 第四步,了解工厂的信息,如果Bean实现BeanFactoryAware 或者 ApplicationContextAware 设置工厂
- 第五步,初始化前方法, 如果存在BeanPostProcessor(后处理Bean), 执行postProcessBeforeInitialization
- 第六步,属性设置后, 如果Bean实现了InitializingBean 执行afterPropertiesSet
- 第七步:MAN被初始化了.... , 调用`<bean init-mothod="init">`
- 第八步,初始化后的方法, 如果存在类实现 BeanPostProcessor ( 处理Bean ) , 执行postProcessAfterInitialization
- 第九步: 执行业务方法, 执行业务处理
- 第十步: 执行Spring的销毁方法, 如果Bean实现DisposableBean 执行 destory
- 第十一步: MAN被销毁了....  , 掉用`<bean destroy-method="customerDestroy">`执行销毁方法customerDestroy

> 最重要的是第五步和第八步: 可以增强类的方法

> Spring
>
> ```xml
> <bean id="man" class="com.ioc.demo3.Man" init-method="setup" destroy-method="manDestory">
> ```
>
> - 初始化bean会触发init-method="functionName"
> - bean销毁时触发destory-method = "destoryFunctionName"

补充:
前面工厂内使用的class
Ipet.java

```java
public interface Ipet {
    public void move();
}
```

Dog.java

```java
public class Dog implements Ipet{
    @Override
    public void move() {
        System.out.println("Dog can run!");
    }
}
```

Parrot.java

```java
public class Parrot implements Ipet{
    @Override
    public void move() {
        System.out.println("Parrot can fly!");
    }
}
```



## Spring AOP

> - Spring 框架的一个关键组件是**面向切面的编程**(AOP [ Aspect Oriented Programming ]) 框架, 可以说是OOP（Object Oriented Programming，面向对象编程）的补充和完善。OOP引入封装、继承、多态等概念来建立一种对象层次结构，用于模拟公共行为的一个集合。
>   - 不过OOP允许开发者定义纵向的关系，但并不适合定义横向的关系，例如日志功能。日志代码往往横向地散布在所有对象层次中，而与它对应的对象的核心功能毫无关系对于其他类型的代码，如安全性、异常处理和透明的持续性也都是如此，这种散布在各处的无关的代码被称为横切（cross cutting），在OOP设计中，它导致了大量代码的重复，而不利于各个模块的重用。
>   - AOP技术恰恰相反，它利用一种称为"横切"的技术，剖解开封装的对象内部，并将那些影响了多个类的公共行为封装到一个可重用模块，并将其命名为"Aspect"，即切面。所谓"切面"，简单说就是那些与业务无关，却为业务模块所共同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块之间的耦合度，并有利于未来的可操作性和可维护性。
> - 面向方面的编程需要把程序逻辑分解成不同的部分称为所谓的关注点。
>   - 跨一个应用程序的多个点的功能被称为**横切关注点**，这些横切关注点在概念上独立于应用程序的业务逻辑。有各种各样的常见的很好的方面的例子，如日志记录、审计、声明式事务、安全性和缓存等。
> - 在 OOP 中，关键单元模块度是类，而在 AOP 中单元模块度是方面。依赖注入帮助你对应用程序对象相互解耦和 AOP 可以帮助你从它们所影响的对象中对横切关注点解耦。AOP 是像编程语言的触发物，如 Perl，.NET，Java 或者其他。
> - Spring AOP 模块提供拦截器来拦截一个应用程序，例如，当执行一个方法时，你可以在方法执行之前或之后添加额外的功能。
> - 使用"横切"技术，AOP把软件系统分为两个部分：**核心关注点**和**横切关注点**。业务处理的主要流程是核心关注点，与之关系不大的部分是横切关注点。横切关注点的一个特点是，他们经常发生在核心关注点的多处，而各处基本相似，比如权限认证、日志、事物。AOP的作用在于分离系统中的各种关注点，将核心关注点和横切关注点分离开来。
> - AOP分为两种类型: 
>   - 静态AOP : 在编译期进行加入, 就是对切面进行的任何修改, 都要进行重新编译程序 
>   - 动态AOP : 在代码执行过程中进行加入,  他的切面代码不是编译进class 文件分钟, SpringAOP就是动态AOP



### AOP术语

| 项                        | 描述                                                         |
| ------------------------- | ------------------------------------------------------------ |
| 横切关注点                | 对那些方法进行拦截, 拦截后如何处理, 这些关注点成为横切关注点 |
| Aspect ( 切面 )           | 类就是对物体特征的抽象， 切面就是对横切关注点的抽象， 一个模块具有一组提供横切需求的 APIs。例如，一个日志模块为了记录日志将被 AOP 方面调用。应用程序可以拥有任意数量的方面，这取决于需求。 |
| Join point ( 连接点)      | 在你的应用程序中它代表一个点，你可以在插件 AOP 方面。你也能说，它是在实际的应用程序中，其中一个操作将使用 Spring AOP 框架。 |
| Advice( 通知)             | 这是实际行动之前或之后执行的方法。这是在程序执行期间通过 Spring AOP 框架实际被调用的代码。 |
| Pointcut( 切入点 )        | 对连接点进行拦截的定义， 这是一组一个或多个连接点，通知应该被执行。你可以使用表达式或模式指定切入点正如我们将在 AOP 的例子中看到的。 |
| Introduction( )           | 在不修改代码的前提下， 引入可以在运行期， 为类动态地添加一些方法或字段，引用允许你添加新方法或属性到现有的类中。 |
| Target object( 目标对象 ) | 被一个或者多个方面所通知的对象，这个对象永远是一个被代理对象。也称为被通知对象。 |
| Weaving( 织入 )           | Weaving 把方面连接到其它的应用程序类型或者对象上，并创建一个被通知的对象。这些可以在编译时，类加载时和运行时完成。 |



### **Spring对AOP的支持**

**Spring中AOP代理由Spring的IOC容器负责生成、管理，其依赖关系也由IOC容器负责管理**。因此，AOP代理可以直接使用容器中的其它bean实例作为目标，这种关系可由IOC容器的依赖注入提供。Spring创建代理的规则为：

1、**默认使用Java动态代理来创建AOP代理**，这样就可以为任何接口实例创建代理了

2、**当需要代理的类不是代理接口的时候，Spring会切换为使用CGLIB代理**，也可强制使用CGLIB

AOP编程其实是很简单的事情，纵观AOP编程，程序员只需要参与三个部分：

1、定义普通业务组件

2、定义切入点，一个切入点可能横切多个业务组件

3、定义增强处理，增强处理就是在AOP框架为普通业务组件织入的处理动作

所以进行AOP编程的关键就是定义切入点和定义增强处理，一旦定义了合适的切入点和增强处理，AOP框架将自动生成AOP代理，即：**代理对象的方法=增强处理+被代理对象**的方法。

下面给出一个Spring AOP的.xml文件模板，名字叫做aop.xml，之后的内容都在aop.xml上进行扩展：

 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-4.2.xsd">
            
  	<!-- bean definition & AOP specific configuration -->
  
</beans>
```

 

 

#### **基于Spring的AOP简单实现**

注意一下，在讲解之前，说明一点：使用Spring AOP，要成功运行起代码，只用Spring提供给开发者的jar包是不够的，请额外上网下载两个jar包：

1、aopalliance.jar

2、aspectjweaver.jar

开始讲解用Spring AOP的XML实现方式，先定义一个接口：

```java
public interface HelloWorld
{
    void printHelloWorld();
    void doPrint();
}
```

定义两个接口实现类：

 

```java
public class HelloWorldImpl1 implements HelloWorld
{
    public void printHelloWorld()
    {
        System.out.println("Enter HelloWorldImpl1.printHelloWorld()");
    }
    
    public void doPrint()
    {
        System.out.println("Enter HelloWorldImpl1.doPrint()");
        return ;
    }
}
```

 

 

```java
public class HelloWorldImpl2 implements HelloWorld
{
    public void printHelloWorld()
    {
        System.out.println("Enter HelloWorldImpl2.printHelloWorld()");
    }
    
    public void doPrint()
    {
        System.out.println("Enter HelloWorldImpl2.doPrint()");
        return ;
    }
}
```

 

横切关注点，这里是打印时间：

 

```java
public class TimeHandler
{
    public void printTime()
    {
        System.out.println("CurrentTime = " + System.currentTimeMillis());
    }
}
```

 

有这三个类就可以实现一个简单的Spring AOP了，看一下aop.xml的配置：

 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-4.2.xsd">
        
        <bean id="helloWorldImpl1" class="com.xrq.aop.HelloWorldImpl1" />
        <bean id="helloWorldImpl2" class="com.xrq.aop.HelloWorldImpl2" />
        <bean id="timeHandler" class="com.xrq.aop.TimeHandler" />
        
        <aop:config>
            <aop:aspect id="time" ref="timeHandler">
                <aop:pointcut id="addAllMethod" expression="execution(* com.xrq.aop.HelloWorld.*(..))" />
                <aop:before method="printTime" pointcut-ref="addAllMethod" />
                <aop:after method="printTime" pointcut-ref="addAllMethod" />
            </aop:aspect>
        </aop:config>
</beans>
```



写一个main函数调用一下：



```java
public static void main(String[] args)
{
    ApplicationContext ctx = 
            new ClassPathXmlApplicationContext("aop.xml");
        
    HelloWorld hw1 = (HelloWorld)ctx.getBean("helloWorldImpl1");
    HelloWorld hw2 = (HelloWorld)ctx.getBean("helloWorldImpl2");
    hw1.printHelloWorld();
    System.out.println();
    hw1.doPrint();
    
    System.out.println();
    hw2.printHelloWorld();
    System.out.println();
    hw2.doPrint();
}
```



运行结果为：



```shell
CurrentTime = 1446129611993
Enter HelloWorldImpl1.printHelloWorld()
CurrentTime = 1446129611993

CurrentTime = 1446129611994
Enter HelloWorldImpl1.doPrint()
CurrentTime = 1446129611994

CurrentTime = 1446129611994
Enter HelloWorldImpl2.printHelloWorld()
CurrentTime = 1446129611994

CurrentTime = 1446129611994
Enter HelloWorldImpl2.doPrint()
CurrentTime = 1446129611994
```



看到给HelloWorld接口的两个实现类的所有方法都加上了代理，代理内容就是打印时间

 

#### ***\*基于Spring的AOP使用其他细节\****

1、增加一个横切关注点，打印日志，Java类为：

```java
public class LogHandler
{
    public void LogBefore()
    {
        System.out.println("Log before method");
    }
    
    public void LogAfter()
    {
        System.out.println("Log after method");
    }
}
```

aop.xml配置为：



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-4.2.xsd">
        
        <bean id="helloWorldImpl1" class="com.xrq.aop.HelloWorldImpl1" />
        <bean id="helloWorldImpl2" class="com.xrq.aop.HelloWorldImpl2" />
        <bean id="timeHandler" class="com.xrq.aop.TimeHandler" />
        <bean id="logHandler" class="com.xrq.aop.LogHandler" />
        
        <aop:config>
            <aop:aspect id="time" ref="timeHandler" order="1">
                <aop:pointcut id="addTime" expression="execution(* com.xrq.aop.HelloWorld.*(..))" />
                <aop:before method="printTime" pointcut-ref="addTime" />
                <aop:after method="printTime" pointcut-ref="addTime" />
            </aop:aspect>
            <aop:aspect id="log" ref="logHandler" order="2">
                <aop:pointcut id="printLog" expression="execution(* com.xrq.aop.HelloWorld.*(..))" />
                <aop:before method="LogBefore" pointcut-ref="printLog" />
                <aop:after method="LogAfter" pointcut-ref="printLog" />
            </aop:aspect>
        </aop:config>
</beans>
```



测试类不变，打印结果为：



```shell
CurrentTime = 1446130273734
Log before method
Enter HelloWorldImpl1.printHelloWorld()
Log after method
CurrentTime = 1446130273735

CurrentTime = 1446130273736
Log before method
Enter HelloWorldImpl1.doPrint()
Log after method
CurrentTime = 1446130273736

CurrentTime = 1446130273736
Log before method
Enter HelloWorldImpl2.printHelloWorld()
Log after method
CurrentTime = 1446130273736

CurrentTime = 1446130273737
Log before method
Enter HelloWorldImpl2.doPrint()
Log after method
CurrentTime = 1446130273737
```



要想让logHandler在timeHandler前使用有两个办法：

（1）aspect里面有一个order属性，order属性的数字就是横切关注点的顺序

（2）把logHandler定义在timeHandler前面，Spring默认以aspect的定义顺序作为织入顺序

2、我只想织入接口中的某些方法

修改一下pointcut的expression就好了：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-4.2.xsd">
        
        <bean id="helloWorldImpl1" class="com.xrq.aop.HelloWorldImpl1" />
        <bean id="helloWorldImpl2" class="com.xrq.aop.HelloWorldImpl2" />
        <bean id="timeHandler" class="com.xrq.aop.TimeHandler" />
        <bean id="logHandler" class="com.xrq.aop.LogHandler" />
        
        <aop:config>
            <aop:aspect id="time" ref="timeHandler" order="1">
                <aop:pointcut id="addTime" expression="execution(* com.xrq.aop.HelloWorld.print*(..))" />
                <aop:before method="printTime" pointcut-ref="addTime" />
                <aop:after method="printTime" pointcut-ref="addTime" />
            </aop:aspect>
            <aop:aspect id="log" ref="logHandler" order="2">
                <aop:pointcut id="printLog" expression="execution(* com.xrq.aop.HelloWorld.do*(..))" />
                <aop:before method="LogBefore" pointcut-ref="printLog" />
                <aop:after method="LogAfter" pointcut-ref="printLog" />
            </aop:aspect>
        </aop:config>
</beans>
```

表示timeHandler只会织入HelloWorld接口print开头的方法，logHandler只会织入HelloWorld接口do开头的方法



测试aop其他几个标签



LogHandler.java

```java
package com.tutorialspoint.beans.SpringAOP.DynamicAOPPackage;

import org.aspectj.lang.ProceedingJoinPoint;

public class LogHandler {

    public void LogBefore() {
        System.out.println("Log before method");
    }

    public void LogAfter() {
        System.out.println("Log after method");
    }



// 环绕通知 需要通过ProceedingJoinPoint 类型的参数指定增强执行的时机
    public void round(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("方法执行前");
        // 执行增强的方法
        joinPoint.proceed();
        System.out.println("方法执行后");
    }

    public void returning() {
        System.out.println("after-returning");
    }

    public void throwing() {
        System.out.println("after-throwing");
    }
}

```







```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-4.2.xsd">

    <bean id="helloWorldImpl1" class="com.tutorialspoint.beans.SpringAOP.DynamicAOPPackage.HelloWorldImpl1" />
    <bean id="helloWorldImpl2" class="com.tutorialspoint.beans.SpringAOP.DynamicAOPPackage.HelloWorldImpl2" />
    <bean id="timeHandler" class="com.tutorialspoint.beans.SpringAOP.DynamicAOPPackage.TimeHandler" />
    <bean id="logHandler" class="com.tutorialspoint.beans.SpringAOP.DynamicAOPPackage.LogHandler" />

    <aop:config>

        <aop:aspect id="time" ref="timeHandler" order="1">
            <aop:pointcut id="addAllMethod"
                  expression="execution(* com.tutorialspoint.beans.SpringAOP.DynamicAOPPackage.Hello.*(..))"/>
            <aop:before method="printTime" pointcut-ref="addAllMethod" />
            <aop:after method="printTime" pointcut-ref="addAllMethod" />
        </aop:aspect>

        <aop:aspect id="log" ref="logHandler" order="2">
            <aop:pointcut
                id="pringLog"
                expression="execution(* com.tutorialspoint.beans.SpringAOP.DynamicAOPPackage.Hello.*(..))"/>
            <aop:before method="LogBefore" pointcut-ref="pringLog"/>
            <aop:after method="LogAfter" pointcut-ref="pringLog" />
            <aop:around method="round" pointcut-ref="pringLog" />
            <aop:after-returning method="returning" pointcut-ref="pringLog"/>
            <aop:after-throwing method="throwing" pointcut-ref="pringLog"/>
        </aop:aspect>

    </aop:config>

</beans>
```



执行

```shell
CurrentTime = 1610941059733
Log before method
方法执行前
Enter HelloWorldImpl1.printHelloWorld() 
after-returning
方法执行后
Log after method
CurrentTime = 1610941059736
...
```





补充:

1. `execution`函数: 在通知中定义切入点, 通过`execution`函数, 可以定义切入点的方法切入
2. 切入点: 就是增强切入点的方法
3. 常用的表达式: 
   1. `excution(<访问类型>?<返回类型><方法名>(<参数>) <异常>)`
   2. `excution(* com.test.bean.Book.wirteBook(..))`:　表示类Book里面的writeBook的方法
   3. `execution(* com.test.bean.Book.*(..))`: 表示Book下的所有方法
   4. `execution(* *.*(..))` : 表示所有
   5. `execution(public *.*(..))`: 所有public的方法
   6. `execution(* com.test.bean.*(..))`: 匹配指定路径下所有类的方法 ( 不包含子包 )
   7. `execution(* com.test.bean..*(..))`: 匹配指定路径下所有类的方法（ 包含包，子包的所有类 ）
   8. `execution(* com.test.bean.Book.*(..))`: 匹配指定类的所有方法
   9. `execution(* com*(..))`: 匹配特定开头的方法
4. 配置增强类型
   1. `aop:before` : 前置通知, 指定增强的方法在切入点之前执行
   2. `aop:after` : 后置通知, 指定增强方法在切入点之后执行
   3. `aop:after-returning`: 最终通知, 无论增强方法之后异常都会执行
   4. `aop:after-throwing` : 异常抛出通知, 指定增强的方法在出现异常时执行 
   5. `aop:around`: 环绕通知, 增强方法在切入点方法之前和之后都执行

3、强制使用CGLIB生成代理

前面说过Spring使用动态代理或是CGLIB生成代理是有规则的，高版本的Spring会自动选择是使用动态代理还是CGLIB生成代理内容，当然我们也可以强制使用CGLIB生成代理，那就是<aop:config>里面有一个"proxy-target-class"属性，这个属性值如果被设置为true，那么基于类的代理将起作用，如果proxy-target-class被设置为false或者这个属性被省略，那么基于接口的代理将起作用。 

### 注解式开发



XML文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-4.2.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
// 组件扫描
    <context:component-scan base-package="com.tutorialspoint.beans.SpringAOP.AOPAnnontation2"/>
// aop自动扫描  
  <aop:aspectj-autoproxy/>

</beans>
```

接口

```java
public interface ITargetInterface {
    public void save();
}
```



实现类

```java
package com.tutorialspoint.beans.SpringAOP.AOPAnnontation2;

import org.springframework.stereotype.Component;

@Component("target")
public class Target implements ITargetInterface{
    @Override
    public void save() {
        System.out.println("save running...");
    }
}

```

创建切面类



```java
package com.tutorialspoint.beans.SpringAOP.AOPAnnontation2;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

@Component("myAspect")
@Aspect
public class MyAspect {

//    @Before("execution(* com.tutorialspoint.beans.SpringAOP.AOPAnnontation2.*.*(..))")
//    public void before(){
//        System.out.println("before");
//    }
//
//    @AfterReturning("execution(* com.tutorialspoint.beans.SpringAOP.AOPAnnontation2.*.*(..))")
//    public void afterReturning() {
//        System.out.println("afterReturning");
//    }
//
//    @Around("execution(* com.tutorialspoint.beans.SpringAOP.AOPAnnontation2.*.*(..))")
//    public Object around(ProceedingJoinPoint pjp) throws Throwable {
//        System.out.println("around before");
//        Object proceed = pjp.proceed();
//        System.out.println("around after");
//        return proceed;
//    }
//
//    @AfterThrowing("execution(* com.tutorialspoint.beans.SpringAOP.AOPAnnontation2.*.*(..))")
//    public void afterThrowing() {
//        System.out.println("throwing");
//    }
//
//    @After("execution(* com.tutorialspoint.beans.SpringAOP.AOPAnnontation2.*.*(..))")
//    public void after() {
//        System.out.println("returning");
//    }

    @Before("pointcut()")
    public void before(){
        System.out.println("before");
    }

    @AfterReturning("pointcut()")
    public void afterReturning() {
        System.out.println("afterReturning");
    }

    @Around("pointcut()")
    public Object around(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("around before");
        Object proceed = pjp.proceed();
        System.out.println("around after");
        return proceed;
    }

    @AfterThrowing("pointcut()")
    public void afterThrowing() {
        System.out.println("throwing");
    }

    @After("pointcut()")
    public void after() {
        System.out.println("returning");
    }

    @Pointcut("execution(* com.tutorialspoint.beans.SpringAOP.AOPAnnontation2.*.*(..))")
    public void pointcut(){}

}
```

> @Pointcut注解定义切点表达式, 然后可以在增强注解中使用



测试类

```java
package com.tutorialspoint.beans.SpringAOP.AOPAnnontation2;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:aop-anno.xml")
public class AnnoTest {

    @Autowired
    private ITargetInterface iTargetInterface;

    @Test
    public void test1(){
        iTargetInterface.save();
    }
}
```



## Spring 基于注解的配置

### @Required

> 注释应用于 bean 属性的 setter 方法，它表明受影响的 bean 属性在配置时必须放在 XML 配置文件中，否则容器就会抛出一个 BeanInitializationException 异常

### @Autowired

> 自动装配

@Autowired 注释应用于构造函数

```java
public class MovieRecommender {
 
    private final CustomerPreferenceDao customerPreferenceDao;
 
    @Autowired
    public MovieRecommender(CustomerPreferenceDao customerPreferenceDao) {
        this.customerPreferenceDao = customerPreferenceDao;
    }
 
    // ...
}
```

@Autowired 注释应用于 setter 方法

```java
public class SimpleMovieLister {
 
    private MovieFinder movieFinder;
 
    @Autowired
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
 
    // ...
}
```

将 @Autowired 注释应用于具有任意名称和多个参数的方法

```java
public class MovieRecommender {
 
    private MovieCatalog movieCatalog;
 
    private CustomerPreferenceDao customerPreferenceDao;
 
    @Autowired
    public void prepare(MovieCatalog movieCatalog,
            CustomerPreferenceDao customerPreferenceDao) {
        this.movieCatalog = movieCatalog;
        this.customerPreferenceDao = customerPreferenceDao;
    }
 
    // ...
}
```

 @Autowired 注释应用于字段，或者将其与构造函数混合，如以下示例所示

```java
public class MovieRecommender {
 
    private final CustomerPreferenceDao customerPreferenceDao;
 
    @Autowired
    private MovieCatalog movieCatalog;
 
    @Autowired
    public MovieRecommender(CustomerPreferenceDao customerPreferenceDao) {
        this.customerPreferenceDao = customerPreferenceDao;
    }
 
    // ...
}
```

直接应用于字段是我们使用的最多的一种方式，但是使用构造方法注入从代码层面却是更加好的。除此之外，还有以下不太常见的几种方式

将 @Autowired 注释添加到需要该类型数组的字段或方法，则 Spring 会从 ApplicationContext 中搜寻符合指定类型的所有 bean，如以下示例所示：

```java
public class MovieRecommender {
 
    @Autowired
    private MovieCatalog[] movieCatalogs;
 
    // ...
}
```

数组可以，我们可以马上举一反三，那容器也可以吗，答案是肯定的，下面是 set 以及 map 的例子：

```java
public class MovieRecommender {
 
    private Set<MovieCatalog> movieCatalogs;
 
    @Autowired
    public void setMovieCatalogs(Set<MovieCatalog> movieCatalogs) {
        this.movieCatalogs = movieCatalogs;
    }
 
    // ...
}
public class MovieRecommender {
 
    private Map<String, MovieCatalog> movieCatalogs;
 
    @Autowired
    public void setMovieCatalogs(Map<String, MovieCatalog> movieCatalogs) {
        this.movieCatalogs = movieCatalogs;
    }
 
    // ...
}
```

以上就是 @Autowired 注释的主要使用方式，经常使用 Spring 的话应该对其中常用的几种不会感到陌生。

### **@Qualifier**

> 可能会有这样一种情况，当你创建多个具有相同类型的 bean 时，并且想要用一个属性只为它们其中的一个进行装配，在这种情况下，你可以使用 **@Qualifier** 注释和 **@Autowired** 注释通过指定哪一个真正的 bean 将会被装配来消除混乱



```java
public class Profile {
    @Autowired
    @Qualifier("student1")//对应xml中bean id为student1
    private Student student;
    public Profile() {
        System.out.println("Inside Profile constructor");
    }
    public void printAge(){
        System.out.println("Age : " + student.getAge() );
    }
    public void printName() {
        System.out.println("Name: " + student.getname() );
    }
}
```



```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 					   http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config></context:annotation-config>

    <bean id="student" class="com.tutorialspoint.beans.Annontation.Student">
        <property name="name" value="grh"/>
        <property name="age" value="11"/>
    </bean>
    <bean id="student1" class="com.tutorialspoint.beans.Annontation.Student">
        <property name="name" value="grh2"/>
        <property name="age" value="22"/>
    </bean>
    <bean id="profile" class="com.tutorialspoint.beans.Annontation.Profile">
    </bean>
</beans>
```



```shell
Inside Profile constructor
Name: grh2
Age : 22
```



### @PostConstruct 和 @PreDestroy

> 这两个分别对应``init-method` 和 `destroy-method`
>
> 即bean的安装和卸载



### @Resource

> 使用一个'name'属性, 該属性以一个bean名称的形式被注入, 遵循by-name中自动连接语句

```java
package com.tutorialspoint;
import javax.annotation.Resource;
public class TextEditor {
   private SpellChecker spellChecker;
   @Resource(name= "spellChecker")
   public void setSpellChecker( SpellChecker spellChecker ){
      this.spellChecker = spellChecker;
   }
   public SpellChecker getSpellChecker(){
      return spellChecker;
   }
   public void spellCheck(){
      spellChecker.checkSpelling();
   }
}
```



### @Configuration 和 @Bean 注解

> **@Configuration** 的注解类表示这个类可以使用 Spring IoC 容器作为 bean 定义的来源。
>
> **@Bean** 注解告诉 Spring，一个带有 @Bean 的注解方法将返回一个对象，该对象应该被注册为在 Spring 应用程序上下文中的 bean。



```java
public class HelloWorld {
    private String message;
    public void setMessage(String message){
        this.message = message;
    }
    public void getMessage() {
        System.out.println("Your message: " + message);
    }
}
```



```java
package com.tutorialspoint.beans.Annontation;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class HelloWorldConfig {
    @Bean
    public HelloWorld helloWorld() {
        return new HelloWorld();
    }
}

```



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config></context:annotation-config>
    <context:component-scan base-package="com.tutorialspoint.beans.Annontation"/>

</beans>
```



Test

```java
ApplicationContext context = new ClassPathXmlApplicationContext("Annontation.xml");
HelloWorld helloWorld = context.getBean(HelloWorld.class);
helloWorld.setMessage("Hello World!");
helloWorld.getMessage();
```



### @import

> 允许从另一个配置类中加载@Bean定义



```java
@Configuration
public class ConfigA {
   @Bean
   public A a() {
      return new A(); 
   }
}
```

你可以在另一个 Bean 声明中导入上述 Bean 声明，如下所示：

```java
@Configuration
@Import(ConfigA.class)
public class ConfigB {
   @Bean
   public B b() {
      return new B(); 
   }
}
```

现在，当实例化上下文时，不需要同时指定 ConfigA.class 和 ConfigB.class，只有 ConfigB 类需要提供，如下所示：

```java
public static void main(String[] args) {
   ApplicationContext ctx = 
   new AnnotationConfigApplicationContext(ConfigB.class);
   // now both beans A and B will be available...
   A a = ctx.getBean(A.class);
   B b = ctx.getBean(B.class);
}
```

> 注意这里的`ApplicationContext ctx = 
>    new AnnotationConfigApplicationContext(ConfigB.class);`



### 生命周期回调@Bean(initMethod = "", destroyMethod = "")

> @Bean 注解支持指定任意的初始化和销毁的回调方法，就像在 bean 元素中 Spring 的 XML 的初始化方法和销毁方法的属性：

```java
public class Foo {
   public void init() {
      // initialization logic
   }
   public void cleanup() {
      // destruction logic
   }
}

@Configuration
public class AppConfig {
   @Bean(initMethod = "init", destroyMethod = "cleanup" )
   public Foo foo() {
      return new Foo();
   }
}
```

指定 Bean 的范围：

默认范围是单实例，但是你可以重写带有 @Scope 注解的该方法，如下所示：

```java
@Configuration
public class AppConfig {
   @Bean
   @Scope("prototype")
   public Foo foo() {
      return new Foo();
   }
}
```

### @Primary

> 对于同一个接口, 可能有几种不同的实现类, 而默认只会采取其中一种情况下 @Primary 的作用就出来了
>
> 自动装配时当出现多个Bean候选者时, 被注解为@Primary将作为首选者, 否则将会抛出异常



## 事件处理

> - Spring核心是ApplicationContext,负责调用beans的生命周期
>
> - Spring的时间处理是单线程的, 所有如果一个时间被发布 , 直至并且除非所有的接收者得到的该消息, 该进程被阻塞并且流程将不会继续 

| 事件名                    | Spring 内置事件 & 描述                                       |
| ------------------------- | ------------------------------------------------------------ |
| **ContextRefreshedEvent** | ApplicationContext 被初始化或刷新时，该事件被发布。这也可以在 ConfigurableApplicationContext 接口中使用 refresh() 方法来发生。 |
| **ContextStartedEvent**   | 当使用 ConfigurableApplicationContext 接口中的 start() 方法启动 ApplicationContext 时，该事件被发布。你可以调查你的数据库，或者你可以在接受到这个事件后重启任何停止的应用程序。 |
| **ContextStoppedEvent**   | 当使用 ConfigurableApplicationContext 接口中的 stop() 方法停止 ApplicationContext 时，发布这个事件。你可以在接受到这个事件后做必要的清理的工作。 |
| **ContextClosedEvent**    | 当使用 ConfigurableApplicationContext 接口中的 close() 方法关闭 ApplicationContext 时，该事件被发布。一个已关闭的上下文到达生命周期末端；它不能被刷新或重启。 |
| **RequestHandledEvent**   | 这是一个 web-specific 事件，告诉所有 bean HTTP 请求已经被服务。 |

#### 监听上下文事件



```java
package com.tutorialspoint;
public class HelloWorld {
   private String message;
   public void setMessage(String message){
      this.message  = message;
   }
   public void getMessage(){
      System.out.println("Your Message : " + message);
   }
}
```

下面是 **CStartEventHandler.java** 文件的内容：

```java
package com.tutorialspoint;
import org.springframework.context.ApplicationListener;
import org.springframework.context.event.ContextStartedEvent;
public class CStartEventHandler 
   implements ApplicationListener<ContextStartedEvent>{
   public void onApplicationEvent(ContextStartedEvent event) {
      System.out.println("ContextStartedEvent Received");
   }
}
```

下面是 **CStopEventHandler.java** 文件的内容：

```java
package com.tutorialspoint;
import org.springframework.context.ApplicationListener;
import org.springframework.context.event.ContextStoppedEvent;
public class CStopEventHandler 
   implements ApplicationListener<ContextStoppedEvent>{
   public void onApplicationEvent(ContextStoppedEvent event) {
      System.out.println("ContextStoppedEvent Received");
   }
}
```

下面是 **MainApp.java** 文件的内容：

```java
package com.tutorialspoint;

import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MainApp {
   public static void main(String[] args) {
      ConfigurableApplicationContext context = 
      new ClassPathXmlApplicationContext("Beans.xml");

      // Let us raise a start event.
      context.start();

      HelloWorld obj = (HelloWorld) context.getBean("helloWorld");

      obj.getMessage();

      // Let us raise a stop event.
      context.stop();
   }
}
```

下面是配置文件 **Beans.xml** 文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <bean id="helloWorld" class="com.tutorialspoint.HelloWorld">
      <property name="message" value="Hello World!"/>
   </bean>

   <bean id="cStartEventHandler" 
         class="com.tutorialspoint.CStartEventHandler"/>

   <bean id="cStopEventHandler" 
         class="com.tutorialspoint.CStopEventHandler"/>

</beans>
```

一旦你完成了创建源和 bean 的配置文件，我们就可以运行该应用程序。如果你的应用程序一切都正常，将输出以下消息：

```shell
ContextStartedEvent Received
Your Message : Hello World!
ContextStoppedEvent Received
```

## 自定义事件

> 编写和发布自定义事件

**CustomEvent.java** 

```java
package com.tutorialspoint;
import org.springframework.context.ApplicationEvent;
public class CustomEvent extends ApplicationEvent{ 
   public CustomEvent(Object source) {
      super(source);
   }
   public String toString(){
      return "My Custom Event";
   }
}
```

下面是 **CustomEventPublisher.java** 文件的内容：

```java
package com.tutorialspoint;
import org.springframework.context.ApplicationEventPublisher;
import org.springframework.context.ApplicationEventPublisherAware;
public class CustomEventPublisher 
   implements ApplicationEventPublisherAware {
   private ApplicationEventPublisher publisher;
   public void setApplicationEventPublisher
              (ApplicationEventPublisher publisher){
      this.publisher = publisher;
   }
   public void publish() {
      CustomEvent ce = new CustomEvent(this);
      publisher.publishEvent(ce);
   }
}
```

下面是 **CustomEventHandler.java** 文件的内容：

```java
package com.tutorialspoint;
import org.springframework.context.ApplicationListener;
public class CustomEventHandler 
   implements ApplicationListener<CustomEvent>{
   public void onApplicationEvent(CustomEvent event) {
      System.out.println(event.toString());
   }
}
```

下面是 **MainApp.java** 文件的内容：

```java
package com.tutorialspoint;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
   public static void main(String[] args) {
      ConfigurableApplicationContext context = 
      new ClassPathXmlApplicationContext("Beans.xml");    
      CustomEventPublisher cvp = 
      (CustomEventPublisher) context.getBean("customEventPublisher");
      cvp.publish();  
      cvp.publish();
   }
}
```

下面是配置文件 **Beans.xml**：



```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <bean id="customEventHandler" 
      class="com.tutorialspoint.CustomEventHandler"/>

   <bean id="customEventPublisher" 
      class="com.tutorialspoint.CustomEventPublisher"/>

</beans>
```

一旦你完成了创建源和 bean 的配置文件后，我们就可以运行该应用程序。如果你的应用程序一切都正常，将输出以下信息：



```shell
My Custom Event
My Custom Event
```

