---
title: java-log4j
date: 2020-09-18 21:31:35
tags:
	- log4j
	- java
	- log
---

# log4j

> log4j包含三个组件
>
> -  Logger(记录器) : 日志类别
> - Appender(输出目的地) : 日志要输出的地方
> - Layout(日志布局) : 日志以何种形式输出

## java项目配置log4j配置步骤

pom.xml

```xml
<!--添加log4j相关jar包-->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.16</version>
</dependency>
```



resources/log4j.properties

```properties
#日志配置
log4j.rootLogger = DEBUG,stdout,file
 
#控制台输出
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.Threshold=DEBUG
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%-d{yyyy-MM-dd HH:mm:ss}[ %p ]%m%n
 
#所有文件输出
log4j.appender.file = org.apache.log4j.FileAppender
log4j.appender.file.File = D:/logs/log.log
log4j.appender.file.Encoding=UTF-8
log4j.appender.file.name = fileLogDemo
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%-d{yyyy-MM-dd HH:mm:ss} [%c {Num}] [%l] [ %t:%r ] - [ %p ]  %m%n
log4j.appender.file.append = true
```

测试类

```java
import org.apache.log4j.Logger;public class log4jTest {
    //获取日志记录器Logger，名字为本类类名
    private static Logger logger = Logger.getLogger(log4jTest.class);
  
    public static void main(String[] args) {
        for(int i=0;i<3;i++){
            // 记录debug级别的信息
            logger.debug("log4j日志输出：This is debug message.");
            // 记录info级别的信息
            logger.info("log4j日志输出：This is info message.");
            // 记录error级别的信息
            logger.error("log4j日志输出：This is error message.");
        }
    }
}
```



## 配置Logger记录器

```java
log4j.rootLogger = [ level ] , appenderName, appenderName, …
```

level表示日志记录的优先级，分为OFF、FATAL、ERROR、WARN、INFO、DEBUG、ALL或者你定义的级别。

Log4j建议使用四个级别，优先级从高到低分别是ERROR、WARN、INFO、DEBUG。

通过在这里定义的级别，你可以控制到应用程序中相应级别的日志信息的开关。比如在这里定义了INFO级别，则应用程序中所有DEBUG级别的日志信息将不被打印出来。



appenderName就是指日志输出的目的。你可以灵活地定义日志输出，也可以同时指定多个输出目的地。

Log4j配置文件实现了输出到控制台、文件、回滚文件、发送日志邮件、输出到数据库日志表、自定义标签等全套功能。

## 配置Appender输出目的地

输出目的地类型：

```properties
org.apache.log4j.ConsoleAppender（控制台），  
org.apache.log4j.FileAppender（文件），  
org.apache.log4j.DailyRollingFileAppender（每天产生一个日志文件），  
org.apache.log4j.RollingFileAppender（文件大小到达指定尺寸的时候产生一个新的文件），  
org.apache.log4j.WriterAppender（将日志信息以流格式发送到任意指定的地方）
```

 

## 配置layout日志布局

```properties
org.apache.log4j.HTMLLayout（HTML表格形式）
org.apache.log4j.SimpleLayout（简单格式的日志，只包括日志信息的级别和指定的信息字符串 ，如:DEBUG - Hello）
org.apache.log4j.TTCCLayout（日志的格式包括日志产生的时间、线程、类别等等信息）
org.apache.log4j.PatternLayout（灵活地自定义日志格式）
```