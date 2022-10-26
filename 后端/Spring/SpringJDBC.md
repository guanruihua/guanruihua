---
title: SpringJDBC 
date: 2021-01-20 09:37:30 
tags: 
- Spring
- jdbc
- java
- back-end
---





# Spring JDBC

> 主要包括四个包: 
>
> - core: 包含JDBC的核心功能, 包括JdbcTemplatea等类
> - dataSource: 访问数据源的实用工具类, 他有多种数据源的实现, 可以在JavaEE容器外部测试jdbc代码
> - object: 以访问对象方式访问数据库, 他允许执行查询并将返回结果作为业务对象, 可以在数据表的列表业务对象的属性之间映射查询结果
> - support: 包含core和object包的支持类, 例如: 提供异常转换功能的SQLException类



```java
import lombok.Data;
@Data
public class User {
    private int ID;
    private String name;
    private String age;
}
```



```java
import java.util.List;
public interface UserDao {
    public int update(String sql, Object[] param);
    public List<User> query(String sql, Object[] param);
}
```



```js
@Repository("userDao")
public class UserDaoImpl implements UserDao{
    @Autowired
    private JdbcTemplate jdbcTemplate;

    public int update(String sql, Object[] param){
        return jdbcTemplate.update(sql, param);
    }

    public List<User> query(String sql, Object[] param) {
        RowMapper<User> rowMapper = new BeanPropertyRowMapper<>(User.class);
        return jdbcTemplate.query(sql, rowMapper, param);
    }
}
```



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.tutorialspoint.dao.jdbcDemo"/>
<!--    配置数据源-->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
<!--       数据库驱动-->
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
<!--        连接数据库URL-->
        <property name="url"
                  value="jdbc:mysql://localhost:3306/spring?characterEncoding=utf8&amp;useSSL=false&amp;serverTimezone=UTC&amp;rewriteBatchedStatements=true"/>
<!--        连接数据库和密码-->
        <property name="username" value="root"/>
        <property name="password" value=""/>
    </bean>
<!--    配置jdbc模版-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
<!--        默认必须使用的数据源-->
        <property name="dataSource" ref="dataSource"/>
    </bean>
</beans>
```



```java
@Test
public void DEMO(){
  ApplicationContext applicationContext = new ClassPathXmlApplicationContext("jdbc.xml");
  UserDao userDao = (UserDao) applicationContext.getBean("userDao");

  Object[] para = { "张三", 21};
  String sql = "insert into user values ( null, ?, ?)";
  userDao.update(sql, para);

  String sql1 = "select * from user";
  List<User> users = userDao.query(sql1, null);
  for (User user : users){
    System.out.println(user);
  }
}
```

