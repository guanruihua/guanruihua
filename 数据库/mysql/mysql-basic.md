---
title: mysql-basic
date: 2021-01-15 14:08:57
tags:
- mysql
- basic
---



# MySQL-basic

> [MySQL :: Download MySQL Community Server (Archived Versions)](https://downloads.mysql.com/archives/community/)
>
> [MySQL :: MySQL Community Downloads](https://dev.mysql.com/downloads/)

## 推荐文章

> [MYSQL创建表的约束条件（可选） - 迎风而来 - 博客园 (cnblogs.com)](https://www.cnblogs.com/sui776265233/p/9343690.html)
>
> [(1条消息) MySQL索引的创建与使用_JustryDeng-CSDN博客_mysql创建索引](https://blog.csdn.net/justry_deng/article/details/81458470)

## 环境搭建

> [免安装版安装](https://blog.csdn.net/qq_39135287/article/details/82117234)
>
> [官方下载路径](https://downloads.mysql.com/archives/community/)

## 基础使用

```shell
mysqld --install # 安装mysql服务
net start mysql # 启动mysql服务
net stop mysql # 关闭mysql服务
mysql -u root -p[密码] # 登录 [-p后面没有空格]
```

登录成功后

```shell
ALLER USER 'root'@'localhost' IDENTFIED BY '新密码'; # 修改密码
exit; # 退出
```

## 控制台使用mysql

```sql
select user(); # 查询用户

show databases; # 显示所有数据库
create database [数据库名] # 创建数据库
drop database [数据库名] # 删除数据库 
create database if not exists # [数据库名] # 判断数据库时候存在

use [数据库名] # 使用数据库
show tables; # 显示所有表
# 显示数据表的字段
show columns from [数据表名];
describe [数据表名];
desc [数据表名];
select * from [数据表]; # 查询数据

# 其他
select now(); # 查询时间
select version(); # 查询数据库版本

source C:\RUIHUA\Project\mysql\init.sql # 执行本地脚本
```

## 基础使用

### 基本数据类型

> 整数类型：BIT、BOOL、TINY INT、SMALL INT、MEDIUM INT、 INT、 BIG INT
>
> 浮点数类型：FLOAT、DOUBLE、DECIMAL
>
> 字符串类型：CHAR、VARCHAR、NVARCHAR、TINY TEXT、TEXT、MEDIUM TEXT、LONGTEXT、TINY BLOB、BLOB、MEDIUM BLOB、LONG BLOB
>
> 日期类型：Date、DateTime、TimeStamp、Time、Year
>
> 其他数据类型：BINARY、VARBINARY、ENUM、SET、Geometry、Point、MultiPoint、LineString、MultiLineString、Polygon、GeometryCollection等

### 数据库

> 创建: `crate database databaseName`
>
> 删除: `drop database databaseName`
>
> 使用: `use databaseName`

### 表

> 删除: `drop table tableName`
>
> 插入数据: `insert into table_name( field1, field2, ...) values ( value1, value2, ...)`

#### 创建表

```sql
CREATE TABLE IF NOT EXISTS `user`(
   `user_id` INT UNSIGNED AUTO_INCREMENT,
   `user_title` VARCHAR(100) NOT NULL,
   `user_author` VARCHAR(40) NOT NULL,
   `submission_date` DATE,
   PRIMARY KEY ( `user_id` ))ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

> auto_increment: 自增的属性, 一般用于主键
>
> not null: 不可为空
>
> primary key: 关键字用于定义主键,  使用多列来定义组件, 列间以逗号分隔开
>
> engine: 设置存储引擎
>
> charset : 设置编码

#### 更新

> `update table_name set field1 = new_value1, field2 = new_value2...[ where clause ]`

### 列

#### 添加列，修改列，删除列

```sql
ALTER TABLE：添加，修改，删除表的列，约束等表的定义。

查看列：desc 表名;
修改表名：alter table t_book rename to bbb;
添加列：alter table 表名 add column 列名 varchar(30);
删除列：alter table 表名 drop column 列名;
修改列名MySQL： alter table bbb change nnnnn hh int;
修改列名SQLServer：exec sp_rename't_student.name','nn','column';
修改列名Oracle：lter table bbb rename column nnnnn to hh int;
修改列属性：alter table t_book modify name varchar(22);
```

#### MySQL 查看约束，添加约束，删除约束 添加列，修改列，删除列

```sql
查看表的字段信息：desc 表名;
查看表的所有信息：show create table 表名;
添加主键约束：alter table 表名 add constraint 主键 （形如：PK_表名） primary key 表名(主键字段);
添加外键约束：alter table 从表 add constraint 外键（形如：FK_从表_主表） foreign key 从表(外键字段) references 主表(主键字段);
删除主键约束：alter table 表名 drop primary key;
删除外键约束：alter table 表名 drop foreign key 外键（区分大小写）;
修改表名：alter table t_book rename to bbb;
添加列：alter table 表名 add column 列名 varchar(30);
删除列：alter table 表名 drop column 列名;
修改列名MySQL： alter table bbb change nnnnn hh int;
修改列属性：alter table t_book modify name varchar(22);
```

### **select查询**

```sql
//查询某张表所有数据
select * from temp;

//查询指定列和条件的数据
//查询name和age这两列，age等于22的数据
select name,age from temp where age = 22;

//as对列重命名
//as可以省略不写，如果重命名的列名出现特殊字符，如单引号，那就需要用双引号引在外面
select name as '名称' from temp;

//给table去别名
select t.name Name from temp as t;

//where条件查询
>、>=、<、<=、=、<>都可以出现在where语句中
select from t where a > 2 or a>=3 or a<5 or a<=6 or a=7 or a<>0;

//and 并且
//查询名称等于Jack并且年龄大于20的
select * from temp where age > 20 and name = 'jack';

//or或者
--满足一个条件即可
select * from temp where name = 'jack' or name = 'jackson';

//between v and v2
--大于等于v且小于等于v2
select * from temp where age between 20 and 25;

//in 查询
--可以多个条件，类似于or
--查询id 在括号中出现的数据
select *from temp where id in (1, 2, 3);

//like模糊查询
--查询name以j开头的
select * from temp where name like 'j%';
--查询name包含k的
select * from temp where name like '%k%';
--escape转义,指定\为转义字符，上面的就可以查询name中包含“_”的数据
--select * from temp where name like '\_%' escape '\';

//is null、is not null
--查询为null的数据
select * from temp where name is null;
--查询不为null 的数据
select * from temp where name is not null;

//order by
--排序，升序(desc)、降序(asc)
--默认升序
select * from temp order by id;
select * from temp order by id asc;
--多列组合
select * from temp order by id, age;

//not
select * from temp where not (age > 20);
select * from temp where id not in(1, 2);

//distinct去掉重复数据
select distinct id from temp;
//多列将是组合的重复数据
select distinct id, age from temp;

//查询常量
select 5+2;
select concat('a', 'bbb');

//concat函数，字符串连接
//concat和null进行连接，会导致连接后的数据成为null
select concat(name, '-eco') from temp;

//对查询的数据进行运算操作
select age +2, age / 2, age - 2, age * 2 from temp where age - 2 > 22;
```

### 分页

```sql
select * from t_areainfo where limit 10,10; // 查询第11-20条

// 使用子查询
```

### 索引

### 分区
