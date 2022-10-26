---
title: ssm
date: 2021-03-20 12:44:55
tags:
- Spring
- SpringMVC
- Mybatis
---



# SSM

## ssm controller 绑定参数 根据url 读取参数

> - 
> - 通过URL路径传递参数并接收

```java
@RequestMapping(value="/path/{key}/{value}", method=RequestMethod.POST)
@ResponseBody
public String requestPath(@PathVariable String key, @PathVariable String value) {
    return "Get request is successful. Path param : key - " + key + "; value - " + value;
}
```

