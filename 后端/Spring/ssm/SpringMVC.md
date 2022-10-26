---
title: SpringMVC 
date: 2020-10-13 21:31:23 
---

# SpringMVC

> - [Spring官网](https://spring.io/)
> - [SpringMVC](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/web.html)
> - Spring体系的轻量级web mvc框架
> - 核心Controller控制器, 用于处理请求, 产生响应
> - 基于SpringIOC容器运行, 所有对象被IOC管理
> - MVC
>   - M: 模型封装应用数据, 通常他们由pojo组成
>   - V: 呈现数据模型, 并且通常由它生成客户端的浏览器可以解释的HTML输出
>   - C: 控制器主要用于处理用户请求, 并且构建合适的模型并将其传递到视图呈现

版本变化

> Spring 5.x最低要求JDK8余J2EE 7(Servlet 3.1 / Tomcat 8.5+)
>
> 支持响应式编程



## DispatchServlet

> -  Spring Web 的mvc模型是围绕dispatcherServlet设计的, dispatcherServlet用来处理所有http请求和响应
>
> - Spring MVC DispatcherServlet 请求的工作流程
>   - 收到一个http请求后, dispatcherServlet根据HanderMapping 来选择适当的控制器
>   - 控制器接收请求, 并基于使用的get或post方法来调用service方法, service方法将设置基于业务逻辑的模型数据, 并返回视图名称到dispatcherServlet中
>   - dispatcherServlet 会用viewResolver获取帮助, 为请求检取定义视图
>   - 一旦确定思路, dispatcherServlet 将把模型数据传输给视图, 最后呈现在浏览器中



## 入门

### 环境搭建

#### Maven依赖spring-webmvc

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>SpringMVC_DEMO1</artifactId>
    <version>1.0-SNAPSHOT</version>
<!--    添加阿里云的镜像路径, 方便下载依赖-->
    <repositories>
        <repository>
            <id>aliyun</id>
            <name>aliyun</name>
            <url>https://maven.aliyun.com/repository/public</url>
        </repository>
    </repositories>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
    </dependencies>
</project>
```



#### web.xml配置DispatcherServlet

#### 配置applicationContext的mvc标记

#### 开发Controller控制器

## 数据绑定

## Restful开发风格

## 拦截器