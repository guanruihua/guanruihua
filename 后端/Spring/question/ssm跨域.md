---
title: ssm跨域
date: 2021-3-12 09:31:40
tags:
- ssm
- 跨域
---

# ssm跨域





创建一个类实现javax.servlet.Filter接口

```java
public class MyCORSFilter implements Filter{  
  @Override  
  public void destroy() {}  

  @Override  
  public void doFilter(ServletRequest req, ServletResponse res,  
          FilterChain chain) throws IOException, ServletException {  
          HttpServletResponse response = (HttpServletResponse) res;  
          response.setHeader("Access-Control-Allow-Origin", "*");  
          response.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS, DELETE");  
          response.setHeader("Access-Control-Max-Age", "3600");  
          response.setHeader("Access-Control-Allow-Headers", "x-requested-with");  
          chain.doFilter(req, res);  
  }  

  @Override  
  public void init(FilterConfig arg0) throws ServletException {}  
}
```


修改web.xml文件
添加Filter的配置

```xml
<filter>  
  <filter-name>cors</filter-name>  
  <filter-class>com.ma.filter.MyCORSFilter</filter-class>  
</filter>  
<filter-mapping>  
  <filter-name>cors</filter-name>  
  <url-pattern>/*</url-pattern>  
</filter-mapping>
```

