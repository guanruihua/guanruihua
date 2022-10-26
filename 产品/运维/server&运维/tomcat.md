---
title: tomcat
date: 2021-02-01 14:44:16
tags:
- tomcat
---

# Tomcat

## 安装

> <http://tomcat.apache.org/>
>
> 下载``64-bit_Windows_zip`
>
> idea 配置 : <https://blog.csdn.net/weixin_41249041/article/details/88375420>

## 配置环境变量

> 不配置也可以使用, 知识每次使用都要到bin文件夹下运行``startup.bat`
>
> 配置Tomcat环境变量
>
> 1，新建变量名：CATALINA_BASE，变量值：E:\Develop\apache-tomcat-7.0.93
>
> 2，新建变量名：CATALINA_HOME，变量值：E:\Develop\apache-tomcat-7.0.93
>
> 3，打开PATH，添加变量值：%CATALINA_HOME%\lib  %CATALINA_HOME%\bin
>
> 到tomcat的bin目录测试是否可以访问。  

## 常识补充

> 文件目录:
>
> - bin: 命令文件, 开启和关闭
> - conf: tomcat的配置文件
> - lib: tomcat所需的依赖库(jar包)
> - logs: 日志文件
> - temp: 运行产生的临时文件
> - webapps: 需要发布的项目都会放在这个目录里面
