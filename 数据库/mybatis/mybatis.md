---
title: mybatis
date: 2021-01-21 15:23:36
tags:
- mybatis
- mysql
---

# mybatis

> [mybatis-spring –](http://mybatis.org/spring/zh/getting-started.html)

## Mybatis 注解增删改查

### 要点

- 有另一种方法来完成语句映射。 它们映射的语句可以不用 XML 来配置，而可以使用 Java 注解来配置。
- **使用注解来映射简单语句会使代码显得更加简洁，但对于稍微复杂一点的语句，Java 注解不仅力不从心，还会让你本就复杂的 SQL 语句更加混乱不堪。**
- **如果你需要做一些很复杂的操作，最好用 XML 来映射语句。**
- 需要在config.xml中注册Java接口

```xml
<mappers>
        <mapper class="com.mybatis.DAO.PeopleMapper"/>
</mappers>
```

> 要用class=

### 查

```java
public interface PeopleMapper {
    @Select("select * from people")
    List<People> getPeopleList();
}
```

### 增

可以先开启事务自动提交

```java
public static SqlSession getSqlSession(){
       return sqlSessionFactory.openSession(true);
    }
}
```

Mapper.java

```java
public interface PeopleMapper {
    @Select("select * from people")
    List<People> getPeopleList();
    @Insert("insert into people(id, name, age, address) VALUES (#{id},#{name},#{age},#{address})")
    int addPeople(People people);
}
```

test

```java
public class PeopleDAOtest {
    @Test
    public void print() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        PeopleMapper peopleMapper = sqlSession.getMapper(PeopleMapper.class);
        List<People> people = peopleMapper.getPeopleList();
        for (People p :people){
            System.out.println(p);
        }
        sqlSession.close();
    }
    @Test
    public void add(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        PeopleMapper peopleMapper = sqlSession.getMapper(PeopleMapper.class);
        peopleMapper.addPeople(new People(6,"圣迭戈",456,"啥地方"));
        print();
    }
}
```

> 因为已经自动提交了，所以不需要sqlSession.commit();

### 删

- **注解@Param**

1. 只能用于基本数据类型
2. 传入的参数只能和sql语句中参数一样

![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b6be7cb5164e48d2b417e8af39fddfa2~tplv-k3u1fbpfcp-watermark.image)

1. 多个参数

![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/753abf07c5f04361a1e7de31c2ff8175~tplv-k3u1fbpfcp-watermark.image)

- 样例

Mapper.java

```java
public interface PeopleMapper {
    @Delete("delete people from people where id=#{uid}")
    int delPeople(@Param("uid") int i);
}
```

test

```java
public class PeopleDAOtest {
    @Test
    public void del(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        PeopleMapper peopleMapper = sqlSession.getMapper(PeopleMapper.class);
        peopleMapper.delPeople(6);
        print();
    }
}
```

### 改

Mapper.java

```java
public interface PeopleMapper {
    @Update("update mybatis.people set name=#{name} ,age=#{age} ,address=#{address} where id=#{id}")
    int updateP(People people);
}
```

test

```java
public class PeopleDAOtest {
    @Test
    public void update(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        PeopleMapper peopleMapper = sqlSession.getMapper(PeopleMapper.class);
        peopleMapper.updateP(new People(5,"圣迭戈",456,"啥地方"));
        print();
    }
}
```





> https://blog.csdn.net/weixin_43168190/article/details/102959304

## DEMO

### pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>org.example</groupId>
  <artifactId>MybatisDemo</artifactId>
  <version>1.0-SNAPSHOT</version>

  <name>MybatisDemo</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.2.8</version>
    </dependency>
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.18</version>
    </dependency>
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.17</version>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>RELEASE</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter</artifactId>
      <version>RELEASE</version>
      <scope>compile</scope>
    </dependency>
  </dependencies>

  <build>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <!-- clean lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#clean_Lifecycle -->
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- default lifecycle, jar packaging: see https://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_jar_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-jar-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
        <!-- site lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#site_Lifecycle -->
        <plugin>
          <artifactId>maven-site-plugin</artifactId>
          <version>3.7.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-project-info-reports-plugin</artifactId>
          <version>3.0.0</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>

```



### log4j.properties

```properties
log4j.rootLogger=DEBUG, stdout

log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=[service] %d - %c -%-4r [%t] %-5p %c %x - %m%n

#log4j.appender.R=org.apache.log4j.DailyRollingFileAppender
#log4j.appender.R.File=../logs/service.log
#log4j.appender.R.layout=org.apache.log4j.PatternLayout
#log4j.appender.R.layout.ConversionPattern=[service] %d - %c -%-4r [%t] %-5p %c %x - %m%n

#log4j.logger.com.ibatis = debug
#log4j.logger.com.ibatis.common.jdbc.SimpleDataSource = debug
#log4j.logger.com.ibatis.common.jdbc.ScriptRunner = debug
#log4j.logger.com.ibatis.sqlmap.engine.impl.SqlMapClientDelegate = debug
#log4j.logger.java.sql.Connection = debug
log4j.logger.java.sql.Statement = debug
log4j.logger.java.sql.PreparedStatement = debug
log4j.logger.java.sql.ResultSet =debug
```



### SqlMapConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!-- MyBatis的主配置文件 -->
<configuration>
    <!-- 配置环境 -->
    <environments default="mysql">
        <!-- 配置MySQL环境 -->
        <environment id="mysql">
            <!-- 配置事务类型 -->
            <transactionManager type="JDBC"/>
            <!-- 配置数据源/连接池 -->
            <dataSource type="POOLED">
                <!-- 配置连接数据库的4个基本信息  -->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/node?useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=Asia/Shanghai"/>
                <property name="username" value="root"/>
                <property name="password" value=""/>
            </dataSource>
        </environment>
    </environments>

    <!-- 指定映射配置文件的位置 -->
    <mappers>
<!--        <mapper resource="IUserDao.xml"/>--> //使用配置文件的用法
        <mapper class = "org.example.dao.IUserDao"/> // 使用注解开发的用法
    </mappers>
</configuration>
```



### User

```java
package org.example.entity;

import lombok.Data;
import java.util.Date;

@Data
public class User {
    private int id;
    private String username;
    private Date birthday;
    private String sex;
    private String address;

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday=" + birthday +
                ", sex='" + sex + '\'' +
                ", address='" + address + '\'' +
                '}';
    }
}
```



### IUserDao.java

```java
package org.example.dao;

import org.apache.ibatis.annotations.Select;
import org.example.entity.User;

import java.util.List;

public interface IUserDao {
    List<User> findAll();
    @Select("select * from eesy_mybatis")
    List<User> findAllUser();
}
```



### IUserDao.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.example.dao.IUserDao">
    <!-- 配置查询所有 -->
    <select id="findAll" resultType="org.example.entity.User">
        select * from eesy_mybatis
    </select>
</mapper>
```



### MybatisTest.java

```java
package org.example.test;

import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.example.dao.IUserDao;
import org.example.entity.User;
import org.junit.jupiter.api.Test;

import org.apache.ibatis.io.Resources;
import java.io.InputStream;
import java.util.List;

public class MybatisTest {
    @Test //使用mapper.xml开发
    public void DEMO() throws Exception{
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(in);
        SqlSession session = factory.openSession();
        IUserDao userDao = session.getMapper(IUserDao.class);
        List<User> users = userDao.findAll();
        for(User user: users){
            System.out.println(user);
        }
        session.close();
        in.close();
    }

    @Test // 使用注解开发 
    public void DEMO2() throws Exception{
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(in);
        SqlSession session = factory.openSession();
        IUserDao userDao = session.getMapper(IUserDao.class);
        List<User> users = userDao.findAllUser();
        for(User user: users){
            System.out.println(user);
        }
        session.close();
        in.close();
    }

}
```



## 动态注解SQL

```java
import org.apache.ibatis.jdbc.SQL;
public class CategoryDynaSqlProvider {
public String list() {
return new SQL()
  .SELECT("*")
  .FROM("category_")
  .toString();
}
public String get() {
return new SQL()
  .SELECT("*")
  .FROM("category_")
  .WHERE("id=#{id}")
  .toString();
}
public String add(){
  return new SQL()
  .INSERT_INTO("category_")
  .VALUES("name", "#{name}")
  .toString();
}
public String update(){
  return new SQL()
  .UPDATE("category_")
  .SET("name=#{name}")
  .WHERE("id=#{id}")
  .toString();
}
public String delete(){
  return new SQL()
  .DELETE_FROM("category_")
  .WHERE("id=#{id}")
  .toString();
  }
}
```



```java
@Insert(" insert into category_ ( name ) values (#{name}) ")
public int add(Category category);
//修改为了注解@InsertProvider配合CategoryDynaSqlProvider的add方法
@InsertProvider(type=CategoryDynaSqlProvider.class,method="add")
public int add(Category category);
```



```java
public interface CategoryMapper {
  @InsertProvider(type=CategoryDynaSqlProvider.class,method="add")
  public int add(Category category);
  @DeleteProvider(type=CategoryDynaSqlProvider.class,method="delete")
  public void delete(int id);
  @SelectProvider(type=CategoryDynaSqlProvider.class,method="get")
  public Category get(int id);
  @UpdateProvider(type=CategoryDynaSqlProvider.class,method="update")
  public int update(Category category);
  @SelectProvider(type=CategoryDynaSqlProvider.class,method="list")
	public List<Category> list();
}
  

**步骤** **7** **:** **关于SQL类**
SQL类是用于进行动态SQL生成的，如下代码是一个相对复杂的SQL类的使用举例，以后需要用到的时候，再来参考。
private String selectPersonSql() {
  return new SQL() {{
    SELECT("P.ID, P.USERNAME, P.PASSWORD, P.FULL_NAME");
    SELECT("P.LAST_NAME, P.CREATED_ON, P.UPDATED_ON");
    FROM("PERSON P");
    FROM("ACCOUNT A");
    INNER_JOIN("DEPARTMENT D on D.ID = P.DEPARTMENT_ID");
    INNER_JOIN("COMPANY C on D.COMPANY_ID = C.ID");
    WHERE("P.ID = A.ID");
    WHERE("P.FIRST_NAME like ?");
    OR();
    WHERE("P.LAST_NAME like ?");
    GROUP_BY("P.ID");
    HAVING("P.LAST_NAME like ?");
    OR();
    HAVING(P.FIRST_NAME like ?");
    ORDER_BY("P.ID");
    ORDER_BY("P.FULL_NAME");
  }}.toString();
}
```

