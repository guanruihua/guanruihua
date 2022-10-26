

# Mybatis



## 静态SQL

### mapper.xml

```java
package com.how2java.mapper;
 
import java.util.List;

import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Select;
import org.apache.ibatis.annotations.Update;

import com.how2java.pojo.Category;
 
public interface CategoryMapper {
 
    @Insert(" insert into category_ ( name ) values (#{name}) ")  
    public int add(Category category);  
       
    @Delete(" delete from category_ where id= #{id} ")  
    public void delete(int id);  
       
    @Select("select * from category_ where id= #{id} ")  
    public Category get(int id);  
     
    @Update("update category_ set name=#{name} where id=#{id} ")  
    public int update(Category category);   
       
    @Select(" select * from category_ ")  
    public List<Category> list();  
}

```



### mybatis-config.xml

```xml-dtd
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <typeAliases>
      <package name="com.how2java.pojo"/>
    </typeAliases>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
            <property name="driver" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost:3306/dbName?characterEncoding=UTF-8"/>
            <property name="username" value="root"/>
            <property name="password" value="admin"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="com/how2java/pojo/Category.xml"/>
        <mapper class="com.how2java.mapper.CategoryMapper"/>  
    </mappers>
</configuration>

```



### Test Class3

```java
package com.how2java;
  
import java.io.IOException;
import java.io.InputStream;
import java.util.List;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import com.how2java.mapper.CategoryMapper;
import com.how2java.pojo.Category;
  
public class TestMybatis {
  
    public static void main(String[] args) throws IOException {
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        SqlSession session = sqlSessionFactory.openSession();
        CategoryMapper mapper = session.getMapper(CategoryMapper.class);
 
//        add(mapper);
//        delete(mapper);
//        get(mapper);
//        update(mapper);
        listAll(mapper);
             
        session.commit();
        session.close();
  
    }
 
    private static void update(CategoryMapper mapper) {
        Category c= mapper.get(8);
        c.setName("修改了的Category名稱");
        mapper.update(c);
        listAll(mapper);
    }
 
    private static void get(CategoryMapper mapper) {
        Category c= mapper.get(8);
        System.out.println(c.getName());
    }
 
    private static void delete(CategoryMapper mapper) {
        mapper.delete(2);
        listAll(mapper);
    }
 
    private static void add(CategoryMapper mapper) {
        Category c = new Category();
        c.setName("新增加的Category");
        mapper.add(c);
        listAll(mapper);
    }
  
    private static void listAll(CategoryMapper mapper) {
        List<Category> cs = mapper.list();
        for (Category c : cs) {
            System.out.println(c.getName());
        }
    }
}
	
```



## 动态SQL

> - mapper使用
>
>   -  @UpdateProvider(type=VideoSqlProvider.class,method="updateVideo")  更新
>   - @InsertProvider   插入
>   - @DeleteProvider   删除
>   - @SelectProvider   查询
>
>   
>
> -  method使用
>   - UPDATE()
>   - INSERT()
>   - SELECT()
>   - SET()
>   - WHERE()
>   - VALUE()
>   - DELETE()
>   - OR()
>   - FORM()





```java
import org.apache.ibatis.jdbc.SQL; 
/**
 * video构建动态sql语句
 */
public class VideoProvider {
 
    /**
     * 更新video动态语句
     * @param video
     * @return
     */
    public String updateVideo(final Video video){
        return new SQL(){{
            UPDATE("video");
            //条件写法.
            if(video.getTitle()!=null){
                SET("title=#{title}");
            }
            if(video.getSummary()!=null){
                SET("summary=#{summary}");
            }
            if(video.getCoverImg()!=null){
                SET("cover_img=#{coverImg}");
            }
            if(video.getViewNum()!=null){
                SET("view_num=#{viewNum}");
            }
            if(video.getPrice()!=null){
                SET("price=#{price}");
            }
            if(video.getOnline()!=null){
                SET("online=#{online}");
            }
            if(video.getPoint()!=null){
                SET("point=#{point}");
            }
            WHERE("id=#{id}");
        }}.toString();
    }
 
}
```

