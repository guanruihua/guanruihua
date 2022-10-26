---
title: mysql-expand
date: 2021-01-31 22:12:06
tags: 
- mysql
- expand
---



# mysql-expand



## 变量声明与赋值



#### 单变量赋值

```sql
declare @id int(16);
set @a = '12';
set @b = (select count(*) from user);
```



#### 多变量赋值

```sql
declare id int(16);
declare name varchar(128);
select id, name into @id, @name from user where id = 1;
select @id:= id, @name:= name from user where id = 1;
```



## 循环的使用

### 函数中使用

```sql
-- 获取根分类中的根分类名称
delimiter //
drop function if exists 'getRootCateName' //
CREATE FUNCTION getRootCateName(cate_id varchar(32))
RETURNS varchar(128) DETERMINISTIC --deterministic
BEGIN
  select pid,category into @p_id,@cate_name from system_category where id=cate_id;
  set @temp_id = @p_id;
  while @temp_id > 0 do
     select pid,category into @p_id,@cate_name from system_category where id=@temp_id;
     set @temp_id = @p_id;
  end while;  
RETURN @cate_name;
end//
delimiter ;

-- 调用
select getRootCateName(28);
```



### 选择语句使用

```sql
delimiter //
create function getScoreName(score int(10))
RETURNS varchar(128) DETERMINISTIC
BEGIN
  if score>=90 then 
        set @grade ='A';  
    elseif score<90 and score>=80 then 
        set @grade ='B';  
    elseif score<80 and score>=70 then 
        set @grade ='C';  
    elseif score<70 and score>=60 then 
        set @grade ='D';  
    else 
        set @grade ='E';  
    end if; 
RETURN @grade;
END //
delimiter ;

-- 调用
select getScoreName(91);
```



### 分支语句使用

```sql
delimiter //
create function getScoreName2(score int(10))
RETURNS varchar(128) DETERMINISTIC
BEGIN
  SELECT case 
    when score>=90 then '甲' 
    when score>=80 then '乙' 
    when score>=70 then '丙' 
    when score>=60 then '丁' 
    else '差' 
    end as grade  into @commont_grade ;
RETURN @commont_grade;
END //
delimiter ;

-- 调用
select getScoreName2(98);
```





### 存储过程使用

```sql
DELIMITER $$
DROP PROCEDURE IF EXISTS 'delete_session' $$
CREATE DEFINER=`root`@`localhost` PROCEDURE `delete_session`(IN top int)  
BEGIN  
  
    DECLARE done INT DEFAULT 0;   
    DECLARE temp_id INT;  
 
    DECLARE cur CURSOR for( SELECT id from user);   
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = 1;  
 
    OPEN cur;  
    FETCH cur INTO temp_id;  
    WHILE done <> 1 DO  
      DELETE FROM user_session WHERE id = temp_id;  
      FETCH cur INTO temp_id;  
    END WHILE;  
 
    CLOSE cur;  
END $$
DELIMITER; 
```



### 存储过程输入输出变量

```sql
DELIMITER $$  
DROP PROCEDURE IF EXISTS `user_banlance` $$  
CREATE PROCEDURE user_banlance (IN `in_userId` INT , IN `in_banlance` INT, OUT out_code INT, OUT out_message VARCHAR(100))  
_return:BEGIN  
DECLARE _userId bigint(20); 
    DECLARE _banlance int; -- 余额
    DECLARE _verison INT DEFAULT 0;  
    DECLARE _error  INT DEFAULT 0;  
    DECLARE CONTINUE HANDLER FOR SQLEXCEPTION SET _error = 1;   -- 异常处理
    SET out_code = -1;
    SET out_message = '执行失败';
 
    #用户账户不存在
    IF NOT EXISTS (select id from user_banlance where id=in_userId) THEN
        SET out_code = 1;
        SET out_message = '用户不存在';
        LEAVE _return;
    END IF;
 
    select verison,banlance into _verison, _banlance from user_banlance where id=in_userId;
 
    START TRANSACTION; -- 开启事务 乐观锁的使用
    update user_banlance set banlance = banlance - in_banlance,verison = verison + 1 where id = in_userId and verison = _verison;
 
    SET @ret_update = ROW_COUNT();
    IF @ret_update = 0 THEN
        ROLLBACK;
        SET out_code = -4;
        SET out_message = '系统错误';
        LEAVE _return;
    END IF;
 
    IF  _error  <>  0   THEN
        ROLLBACK; 
        SET out_code = -3;
        SET out_message = '系统错误';
        LEAVE _return;
    ELSE    
        COMMIT;
        SET out_code = 1;
        SET out_message = '';
    END IF;
 
END $$  
 
DELIMITER ;  

-- 调用  注意输出参数必须是声明的变量，否则会报错

set @b = 0;
set @c = '';
call user_banlance(1,10,@b,@c);
 
select @b,@c
```

### 存储过程的使用( 查询游标 )

```sql
DELIMITER $$  
DROP PROCEDURE IF EXISTS `user_banlance` $$  
CREATE PROCEDURE user_banlance (IN `in_userId` INT , IN `in_banlance` INT)  
BEGIN
  #遍历数据结束标志
  DECLARE done INT DEFAULT FALSE;
  DECLARE _id INT; 
  DECLARE _banlance BIGINT(20);
#游标
  DECLARE cur CURSOR FOR SELECT id,banlance FROM user_banlance where id > in_userId and banlance > in_banlance ;
    #将结束标志绑定到游标
  DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
 
-- 建表tb_temp
 DROP TABLE IF EXISTS tb_temp;
 CREATE TEMPORARY TABLE tb_temp(
   `id` int UNSIGNED NOT NULL , 
   `banlance` bigint(20) NOT NULL,
   PRIMARY KEY (`id`)
 ) ENGINE = MYISAM DEFAULT charset = utf8 ;
 
  -- 打开游标
  OPEN cur;
  -- 开始循环
  read_loop: LOOP
    -- 提取游标里的数据
    FETCH cur INTO _id,_banlance; 
    IF done THEN
      LEAVE read_loop;
    ELSE 
    replace into tb_temp( id ,banlance) values(_id,_banlance);  -- 替换具有唯一索引 和 主键索引的 相同记录
    END IF;
 END LOOP;
 -- 关闭游标 
CLOSE cur; 
select * from tb_temp;
DROP TABLE IF EXISTS tb_temp;
END $$  
 
DELIMITER ;  

-- 调用

call user_banlance(2,1500);
```

### 游标循环另一种实现方式

```sql
-- 打开游标
OPEN cur;
-- 开始循环
REPEAT
-- 提取游标里的数据
    FETCH cur INTO _id,_banlance; 
    IF not done THEN
    insert into tb_temp( id ,banlance) values(_id,_banlance);  -- 替换具有唯一索引 和 主键索引的 相同记录
    END IF;
UNTIL done 
END REPEAT;
 -- 关闭游标 
CLOSE cur; 
```



## 存储过程与函数

### 区别

> 调用方式: 
>
> - 存储过程: call procedure_name(args, args2 ...);
> - 函数: select function_name(arges);
>
> 返回值： 
>
> - 存储过程： 多个值
> - 函数： 只有一个返回值

### 存储过程与存储函数

#### 存储过程

> - 存储过程是一组为了完成某项特定功能的sql语句集，其实质上就是一段存储在数据库中的代码，他可以由声明式的sql语句（如CREATE,UPDATE,SELECT等语句）和过程式sql语句（如IF...THEN...ELSE控制结构语句）组成。
>
> - 就是数据库 SQL 语言层面的代码封装与重用。



##### 存储过程的优缺点

**优点:**

> 1. 可增强sql语言的功能和灵活性
>    存储过程可以用流程控制语言编写，有很强的灵活性，可以完成复杂的判断和较复杂的运算。
> 2. 良好的封装性
>    存储过程被创建后，可以在程序中被多次调用，而不必担心重写编写该存储过程的sql语句。
> 3. 高性能
>    存储过程执行一次后，其执行规划就驻留在高速缓冲存储器中，以后的操作中只需要从高速缓冲器中调用已编译好的二进制代码执行即可，从而提高了系统性能。

**缺点:**

> - 存储过程，往往定制化于特定的数据库上，因为支持的编程语言不同。当切换到其他厂商的数据库系统时，需要重写原有的存储过程。

##### 创建存储过程

##### DELIMITER定界符

> - 解决存储过程/函数里面有分号会导致该存储过程/函数提前结束的问题

   **DELIMITER语法格式：**

```sql
DELIMITER ?	
```

> `?`是用户定义的结束符，通常这个符号可以是一些特殊的符号。另外应避免使用反斜杠,因为他是转义字符。 若希望换回默认的分号作为结束标记，只需再在命令行输入下面的sql语句即可。

```sql
DELIMITER ;
```

##### 存储过程创建

>  使用`CREATE PROCEDURE`语句来创建存储过程。

```sql
CREATE PROCEDURE p_name([proc_parameter[,...]])
routine_body
```

> 语法项“proc_parameter”的语法格式是：

```sql
[IN|OUT|INOUT]parame_name type
```

> p_name : 存储过程的名称
>
> proc_parameter: 指定存储过程中的参数列表
>
> - parame_name:　参数名
> - type: 参数类型
> - IN:　输入参数，OUT: 输出参数, INOUT: 输入输出类型
>
> rountine_body: 存储过程的主体部分
>
> - 以`BEGIN`开始, `END`结束
> - 若只有一条语句可以忽略`BEGIN...END`

##### 局部变量

```sql
DECLARE var_name type [DEFAULT value]
```

- "var_name"用于指定局部变量的名称；
- "type"用来声明变量的类型；
- "DEFAULT"用来指定默认值，如果没有指定则为NULL。

> 注意：局部变量只能在存储过程体的BEGIN...END语句块中;局部变量必须在存储过程体的开头处声明;局部变量的作用范围仅限于声明它的BEGIN...END语句块，其他语句块中的语句不可以使用它。

##### 用户变量

用户变量一般以@开头。
注意：滥用用户变量会导致程序难以理解及管理。

#####  SET语句

> 通过SET语句对局部变量赋值，其格式是： 

```sql
SET var_name = expr[,var_name2 = expr]....
```

##### SELECT....INTO语句

> 用SELECT...INTO语句把选定的列的值存储到局部变量中。格式是：

```sql
SELECT col_name[,..] INTO var_name[,....] table_expr
```

- "col_name"用于指定列名；
- "var_name"用于指定要赋值的变量名；
- "table_expr"表示SELECT语句中FROM后面的部分。

> 注意：SELECT...INTO语句返回的结果集只能有一行数据。

##### 流程控制语句

###### if-then-else 

```sql
DELIMITER &&  
CREATE PROCEDURE proc2(r int)  
begin 
  declare var int;  
  set var=parameter+1;  
  if var=0 then 
  	insert into t values(17);  
  end if;  
  if parameter=0 then 
  	update t set s1=s1+1;  
  else 
  	update t set s1=s1+2;  
  end if;  
end;  
&&  
DELIMITER ; 
```

###### **case语句:**

```sql
DELIMITER &&  
CREATE PROCEDURE proc3 (in parameter int)  
begin 
  declare var int;  
  set var=parameter+1;  
  case var  
  when 0 then   
  	insert into t values(17);  
  when 1 then   
  	insert into t values(18);  
  else   
  	insert into t values(19);  
  end case;  
end;  
&&  
DELIMITER ; 
```



###### **while ···· end while:**

```sql
DELIMITER &&  
CREATE PROCEDURE proc4()  
begin 
  declare var int;  
  set var=0;  
  while var<6 do  
    insert into t values(var);  
    set var=var+1;  
  end while;  
end;  
&&  
DELIMITER ;
```

###### **repeat···· end repea：**

> 在执行操作后检查结果，而 while 则是执行前进行检查。

```sql
DELIMITER &&  
CREATE PROCEDURE proc5 ()  
begin   
  declare v int;  
  set v=0;  
  repeat  
    insert into t values(v);  
    set v=v+1;  
    until v>=5  
  end repeat;  
end;  
&&  
DELIMITER ;

repeat
    --循环体
    until 循环条件  
end repeat;
```

###### **loop ·····endloop:**

loop 循环不需要初始条件，这点和 while 循环相似，同时和 repeat 循环一样不需要结束条件, leave 语句的意义是离开循环。

```sql
DELIMITER &&  
CREATE PROCEDURE proc6 ()  
begin 
  declare v int;  
  set v=0;  
  LOOP_LABLE:loop  
    insert into t values(v);  
    set v=v+1;  
    if v >=5 then 
      leave LOOP_LABLE;  -- 离开循环
    end if;  
  end loop;  
end;  
&&  
DELIMITER ;
```

###### **ITERATE迭代：**

```sql
DELIMITER &&  
CREATE PROCEDURE proc10 ()  
begin 
  declare v int;  
  set v=0;  
  LOOP_LABLE:loop  
    if v=3 then   
      set v=v+1;  
      ITERATE LOOP_LABLE;  
    end if;  
    insert into t values(v);  
    set v=v+1;  
    if v>=5 then 
    	leave LOOP_LABLE;  
    end if;  
  end loop;  
end;  
&&  
DELIMITER ;
```

###### 游标

> 可以用来存储select 语句查询到的结果集，这个结果集可以包含多行数据，从而使我们可以使用迭代的方法从游标中依次取出每行数据。

> MySQL游标的特点：
>
> 1. 只读：无法通过光标更新基础表中的数据。
> 2. 不可滚动：只能按照select语句确定的顺序获取行。不能以相反的顺序获取行。 此外，不能跳过行或跳转到结果集中的特定行。
> 3. 敏感：有两种游标：敏感游标和不敏感游标。
>    1. 敏感游标指向实际数据，不敏感游标使用数据的临时副本。
>    2. 敏感游标比一个不敏感的游标执行得更快，因为它不需要临时拷贝数据。MySQL游标是敏感的。

1. 声明游标
   游标声明必须在变量声明之后。如果在变量声明之前声明游标，MySQL将会发出一个错误。游标必须始终与select语句相关联。

```sql
declare cursor_name cursor for select_statement;
```

2. 打开游标
   使用open语句打开游标，只有先打开游标才能读取数据。

```sql
open cursor_name
```

3. 读取游标
   使用fetch语句来检索游标指向的一行数据，并将游标移动到结果集中的下一行。

```sql
fetch cursor_name into var_name;
```

4. 关闭游标
   使用close语句关闭游标。

```sql
close cursor_name;
```

当游标不再使用时，应该关闭它。 当使用MySQL游标时，还必须声明一个`not found`处理程序来处理当游标找不到任何行时的情况。 因为每次调用fetch语句时，游标会尝试依次读取结果集中的每一行数据。 当游标到达结果集的末尾时，它将无法获得数据，并且会产生一个条件。 处理程序用于处理这种情况。

```sql
declare continue handler for not found set type = 1;
```

type是一个变量，示游标到达结果集的结尾。

```sql
delimiter ?
create PROCEDURE phoneDeal()
BEGIN
    DECLARE  id varchar(64);   -- id
    DECLARE  phone1  varchar(16); -- phone
    DECLARE  password1  varchar(32); -- 密码
    DECLARE  name1 varchar(64);   -- id
    -- 遍历数据结束标志
    DECLARE done INT DEFAULT FALSE;
    -- 游标
    DECLARE cur_account CURSOR FOR select phone,password,name from account_temp;
    -- 将结束标志绑定到游标
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
    
    -- 打开游标
    OPEN  cur_account;     
    -- 遍历
    read_loop: LOOP
            -- 取值 取多个字段
            FETCH  NEXT from cur_account INTO phone1,password1,name1;
            IF done THEN
                LEAVE read_loop;
             END IF;
 
        -- 你自己想做的操作
        insert into account(id,phone,password,name) value(UUID(),phone1,password1,CONCAT(name1,'的家长'));
    END LOOP;
 
    -- 关闭游标
    CLOSE cur_account;
END ?
```

##### 调用存储过程

使用call语句调用存储过程

```sql
call sp_name[(传参)];
```

##### 删除存储过程

使用drop语句删除存储过程

```sql
DROP PROCEDURE sp_name
```

#### 存储函数

> - 存储函数和存储过程一样，都是sql和语句组成的代码块。- 
> - 存储函数不能有输入参数，并且可以直接调用，不需要call语句，且必须有一条包含RETURN语句。

##### 创建存储函数

使用CREATE FUNCTION语句创建：

```sql
CREATE FUNCTION fun_name (par_name type[,...])
RETURNS type
[characteristics] 
fun_body
```

- fun_name为函数名，并且名字唯一，不能与存储过程重名。
  - par_name是指定的参数，type为参数类型；
  - RETURNS字句用来声明返回值和返回值类型。fun_body是函数体，所有存储过程中的sql在存储函数中同样可以使用。但是存储函数体中必须包含一个RETURN 语句。
- characteristics指定存储过程的特性，有以下取值：
  - LANGUAGE SQL:说明routine_body部分是由SQL语句组成的，当前系统支持的语言为SQL，SQL是LANGUAGE特性的唯一值。
  - [NOT] DETERMINISTIC:指明存储过程执行的结果是否确定。DETERMINISTIC表示结果是确定的，每次执行存储过程时，相同的输入会得到相同的输出，NOT DETERMINISTIC表示结果是不确定的，相同的输入可能得到不同的输出，如果没有指定任意一个值，默认为NOT DETERMINISTIC。
  - [CONTAINS SQL|NO SQL|READS SQL DATA|MODIFIES SQL DATA]：指明子程序使用SQL语句的限制。CONTAINS SQL表明子程序包含SQL语句，但不包含读写数据语句；NO SQL表明子程序不包含SQL语句；READS SQL DATA说明子程序包含读数据的语句；MODIFIES SQL DATA表名子程序包含写数据的语句。默认情况下，系统会指定为CONTAINS SQL。
  - SQL SECURITY[DEFINER|INVOKER]：指明谁有权限来执行。DEFINER表示只有定义着才能执行。INVOKER表示用友权限的调用者可以执行。默认情况下，系统指定为DEFINER。
  - COMMENT 'string'：注释信息，用来描述存储过程或函数。

```sql
delimiter ?
create function getAnimalName(animalId int) 
RETURNS VARCHAR(50)
DETERMINISTIC
begin
   declare name VARCHAR(50);
   set name=(select name from animal where id=animalId);
   return (name);
end?
delimiter;

-- 调用
select getAnimalName(4)
```



###  **函数**

> [官网地址](  https://dev.mysql.com/doc/refman/8.0/en/functions.html[)
>
> mysql中的函数可分为两类：系统函数、用户自定义函数。
>
> 函数体里面不用直接`select`



| 函数                    |                         | 功能                   |
| ----------------------- | ----------------------- | ---------------------- |
| SUBSTRING               | substring               | 截取字符串             |
| CHAR_LENGTH             | char_length             | 获取字符串长度         |
| DATE_FORMAT             | date_format             | 格式化日期时间         |
| CONCAT                  | concat                  | 字符串连接             |
| CONCAT_GROUP...GOURP BY | concat_group...gourp by | 将分组的各个字符串连接 |
| SUBSTRING_INDEX         | substring_index         | 字符串截取             |
| ROUND                   | round                   | 数值保留几位小数       |

#### 普通函数

> 返回值是一个标量

创建函数：

```sql
// msyql中的函数一定有返回值
// 创建函数的语法，注意:参数和返回值一定要写长度，避免使用到关键字

create function 函数名（[形参列表]） returns 数据类型
begin
    -- 函数体
    -- 返回值 
end
// 写一个实例,通过子订单child_order_no 的订单号获取其最高父订单的订单金额sale_price 
create function calcPrice(child_order_no varchar(30)) 
returns decimal(12,2)
begin
    // 声明变量，注意：一定要写长度
    declare sale_price decimal default 0.00;
    declare f_order_no varchar(30) default order_no;
    WHILE f_order_no is not null  DO
		set order_no = f_order_no; 
        SET f_order_no = (SELECT o.PARENT_ORDER_NO FROM order o WHERE o.ORDER_NO = f_order_no); 
    END WHILE;
        SELECT f.SALE_PRICE AS SALE_PRICE FROM order f WHERE f.ORDER_NO = order_no into sale_price;
    return sale_price;
end

// 使用这个函数

select calcPrice(1000011);
```

查看自定义函数：

```
// 查看自定义的所有函数
show function status
// 可以根据名称查看所有自定义的函数，支持模糊查询
show function status like '名字';
show function status like '%me';
// 查看函数的创建语句
show create function '函数名字';
show create function getName;
```

使用函数：

```sql
// 使用select关键字查看
select getNames();
```

删除函数：

```sql
// 删除对应函数
drop function '函数名'；
drop function getNames;
```

有的时候创建函数和使用函数时，会被告诉没有权限，这时需要给用户添加上权限，如下：

| create routine | 创建函数       |
| -------------- | -------------- |
| alter routine  | 修改和删除函数 |
| execute        | 使用函数       |

上边只是对mysql中函数的使用做了简单的介绍，想要熟练使用还需要在实战中多使用。



函数类似于存储过程，只是调用方式不同

```sql
//创建函数
create function addAge(age int) returns int
    return age + 5;
//使用函数：
select addAge(age) from temp;
//删除函数
drop function if exists addAge;
drop function addAge;
//显示创建语法
show create function addAge;
```

#### 表格值函数

> 内联表格函数
>
> 多句表格值函数

- **A、**内联表格值函数：返回一个表格

语法：

```sql
create function 函数名（参数）
returns table
as
return(一条SQL语句)
```

例子：

1.创建函数：

```sql
CREATE  FUNCTION  tabcmess(@title VARCHAR(10))
RETURNS  TABLE 
AS 
return(select title,des from product where title like '%'+@title+'%')
```

2.执行：

```sql
SELECT * FROM tabcmess('aaa')
```

- **B.多句表格值函数**

语法：

```sql
 create function 函数名（参数）
   returns 表格变量名table (表格变量定义)
as
   begin
    -- SQL语句
   end
```

例子：

```sql
CREATE function tabcmessalot (@title varchar(10))
Returns @ctable table(title varchar(10) null,des varchar(100) null)
As
Begin
  Insert @ctable Select title,des from product WHERE title LIKE '%'+@title+'%'
  return
End
--执行
SELECT * FROM tabcmessalot('aaa')
```



## **触发器**

触发器分为insert、update、delete三种触发器事件类型，还有after、before触发时间

```sql
//创建触发器
create trigger trg_temp_ins
before insert
on temp for each row
begin
insert into temp_log values(NEW.id, NEW.name);
end
//删除触发器
drop trigger trg_temp_ins
```

## DEMO

### 递归查询

> 父子查询: 根据父id 查询所有的子节点数据
>
> 子父查询: 根据子id 查询上面所有父节点数据



#### **创建表，并添加测试数据**

```sql
DROP TABLE IF EXISTS `vrv_org_tab`;
CREATE TABLE `vrv_org_tab` (
`id` bigint(8) NOT NULL AUTO_INCREMENT,
`org_name` varchar(50) NOT NULL,
`org_level` int(4) NOT NULL DEFAULT ‘0’,
`org_parent_id` bigint(8) NOT NULL DEFAULT ‘0’,
PRIMARY KEY (`id`),
UNIQUE KEY `unique_org_name` (`org_name`)
) ENGINE=InnoDB AUTO_INCREMENT=18 DEFAULT CHARSET=utf8;
```

```sql
INSERT INTO `vrv_org_tab` VALUES (‘1’, ‘北信源’, ‘1’, ‘0’);
INSERT INTO `vrv_org_tab` VALUES (‘2’, ‘北京’, ‘2’, ‘1’);
INSERT INTO `vrv_org_tab` VALUES (‘3’, ‘南京’, ‘2’, ‘1’);
INSERT INTO `vrv_org_tab` VALUES (‘4’, ‘武汉’, ‘2’, ‘1’);
INSERT INTO `vrv_org_tab` VALUES (‘5’, ‘上海’, ‘2’, ‘1’);
INSERT INTO `vrv_org_tab` VALUES (‘6’, ‘北京研发中心’, ‘3’, ‘2’);
INSERT INTO `vrv_org_tab` VALUES (‘7’, ‘南京研发中心’, ‘3’, ‘3’);
INSERT INTO `vrv_org_tab` VALUES (‘8’, ‘武汉研发中心’, ‘3’, ‘4’);
INSERT INTO `vrv_org_tab` VALUES (‘9’, ‘上海研发中心’, ‘3’, ‘5’);
INSERT INTO `vrv_org_tab` VALUES (‘10’, ‘北京EMM项目组’, ‘4’, ‘6’);
INSERT INTO `vrv_org_tab` VALUES (‘11’, ‘北京linkdd项目组’, ‘4’, ‘6’);
INSERT INTO `vrv_org_tab` VALUES (‘12’, ‘南京EMM项目组’, ‘4’, ‘7’);
INSERT INTO `vrv_org_tab` VALUES (‘13’, ‘南京linkdd项目组’, ‘4’, ‘7’);
INSERT INTO `vrv_org_tab` VALUES (‘14’, ‘武汉EMM项目组’, ‘4’, ‘8’);
INSERT INTO `vrv_org_tab` VALUES (‘15’, ‘武汉linkdd项目组’, ‘4’, ‘8’);
INSERT INTO `vrv_org_tab` VALUES (‘16’, ‘上海EMM项目组’, ‘4’, ‘9’);
INSERT INTO `vrv_org_tab` VALUES (‘17’, ‘上海linkdd项目组’, ‘4’, ‘9’);
```



#### **根据父id递归查询所有子节点**

> cast( variable as variable_type ) : 将变量转换为指定的类型
>
> concat() : 连接多个数据
>
> group_concat(): 连接多个数据(消除重复值)
>
> find_in_set( str, strList):   查询字段strList中包含str的结果, 返回null 或 记录
>
> - str : 要查询的字符串
> - strList: 支付数组

```sql
create function getChildrenOrg(orgid INT)
returns varchar(4000)
BEGIN
  DECLARE oTemp VARCHAR(4000);
  DECLARE oTempChild VARCHAR(4000);
  SET oTemp = '';
  SET oTempChild = CAST(orgid AS CHAR);
  WHILE oTempChild IS NOT NULL
  DO
    SET oTemp = CONCAT(oTemp,',',oTempChild);
    SELECT GROUP_CONCAT(id) INTO oTempChild FROM vrv_org_tab WHERE FIND_IN_SET(org_parent_id,oTempChild) > 0;
  END WHILE;
  RETURN oTemp;
END;

-- 查询组织机构为1的下面的所有的组织结构
select * from vrv_org_tab where find_in_set(id, getChildrenOrg(1));
```

 

#### **根据子id递归查询所有父节点**

```sql
SELECT id,org_name,org_level,org_parent_id
    FROM ( 
        SELECT 
                @r AS _id, 
                (SELECT @r := org_parent_id FROM vrv_org_tab WHERE id = _id) AS parent_id, 
                 @l := @l + 1 AS lvl 
        FROM 
                (SELECT @r := 10000, @l := 0) vars, 
                vrv_org_tab h 
        WHERE @r <> 0) T1 
    JOIN vrv_org_tab T2 
    ON T1._id = T2.id
ORDER BY id;
```

注意：大家看到那个10000了吗，就是我们的子节点id。



注意：只支持单个查询，意思是不可以根据两个或者两个以上的子节点同时查询出所有父节点。我们可以看到，上面参数都是单个值进行递归查询的。
西面提供一个函数支持多个查询
 

#### **根据组织机构名称模糊查询所有父节点**

> distinct : 删除重复行
>
> substring( string, position ) : 截取string指定长度的字符串 
>
> substring_index( str[ 被截取字段 ], delim[ 关键字 ], count[ 关键字出现的次数 ] ) :  按关键字截取字符串
>
> length( str ): 返回字串的长度 

```sql
CREATE FUNCTION getParentOrgByOrgName(orgName VARCHAR(20))
RETURNS VARCHAR(4000)
BEGIN
    DECLARE sPid VARCHAR(1000);
    DECLARE sPidTemp VARCHAR(1000);
    DECLARE pid VARCHAR(1000);
    DECLARE count INT DEFAULT 0;
    DECLARE allpid VARCHAR(4000);
    SET sPidTemp = '';
    SELECT GROUP_CONCAT(DISTINCT(CAST(id AS CHAR))) INTO sPid 
    FROM vrv_org_tab WHERE org_name LIKE CONCAT('%',orgName,'%');
    SET allpid = '';
  WHILE count = 0
  DO
  IF sPid IS NULL THEN
  SET allpid = '-1';
  SET count = 1;
  ELSE
      SET pid = SUBSTRING_INDEX(sPid,',',1);
      SET sPidTemp = CONCAT(sPidTemp,',',pid);
      IF LENGTH(pid) = LENGTH(sPid) THEN
          SET count = 1;
          SET sPid = SUBSTRING(sPid FROM LENGTH(SUBSTRING_INDEX(sPid,',',1)) FOR LENGTH(sPid)+1);
      ELSE
          SET sPid = SUBSTRING(sPid FROM LENGTH(SUBSTRING_INDEX(sPid,',',1))+2 FOR LENGTH(sPid)+1);
      END IF;
      SELECT GROUP_CONCAT(CAST(id AS CHAR)) INTO sPidTemp
              FROM ( 
                      SELECT 
                              @r AS _id, 
                              (SELECT @r := org_parent_id FROM vrv_org_tab WHERE id = _id) AS parent_id, 
                              @l := @l + 1 AS lvl 
                      FROM 
                              (SELECT @r := pid, @l := 0) vars, 
                              vrv_org_tab h 
                      WHERE @r <> 0) T1 
              JOIN vrv_org_tab T2 
              ON T1._id = T2.id;
      SET allpid = CONCAT_WS(',',pid,sPidTemp,allpid);
  END IF;
  END WHILE;
RETURN allpid;
END
```