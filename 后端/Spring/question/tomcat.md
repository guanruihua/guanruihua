---
title:tomcat
date:2021-4-11 10:19:22
tags：
- tomcat
---

# Tomcat

## **在请求目标中找到无效字符。有效字符在RFC 7230和RFC 3986中定义**

> 环境：tomcat9
>
> 在tomcat配置中conf/server.xml

```xml
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" 
               // 添加的
               relaxedPathChars="|{}[],%" relaxedQueryChars="|{}[],%" />
```


