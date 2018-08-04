#适合做主键的列

一张表中多列适合作为主键时，遵从最少性和稳定性，一列作为主键，理想状态下，主键列最好不做更新

like '王_华'  '%王' [a-Z][^a-z]

#字段类型

char 和 varchar类型需要制定长度 一个是可变一个是不可变的 char如果输入的不做用空格填满

text 64k长度文本

tinytext 是255字节的文本

meduimtext是16k长度文本

longtext是4GB文本

整数 可以指定是否有符号  无符号范围会增大

int 4字节

tinyint 1字节

smallint 2字节

meduimint 4字节

bigint 8字节

小数需要指定长度和小数位数 

精确类型需要使用decimal类型 如金钱 内部是用字符串存储的

float:4字节

double:8字节

日期和事件：

date:只保存日期不保存时间

datetime:表示日期和时间 1000-01-01

二进制大数据:可以保存图片一类不建议使用会使数据库过大  一般保存图片等的路径。直接通过路径引用

# 流程控制语句

```mysql
SELECT IF(10>5,'emp_id',emp_name)
FROM tbl_emp	//三元运算符，可以选择查询两个字段
```

```mysql
SELECT emp_id,		//注意要写逗号
CASE emp_id
WHEN 1060 THEN emp_id * 10
ELSE emp_id
END
FROM tbl_emp	//根据条件查询，查询出来的emp_id * 10会单独占一列
```

```mysql
SELECT emp_id,
CASE
WHEN emp_id <1080 THEN 'a'
END
FROM tbl_emp	//上面的相当于switch case这个相当于if
```

# union

```mysql
SELECT * FROM tbl_emp WHERE emp_id <1080 
UNION
SELECT * FROM tbl_emp WHERE emp_id >1080 	//联合查询，也可以从不同的表中查询相同数量字段的数据
使用union查询关键字会进行去重操作，如果不想去重可以使用UNION ALL即可
```

#语言类型

ALTER TABLE result ADD CONSTRAINT fk_result_student FOREIGN KEY (studentNo) REFERENCES student(studentNo);

dql 数据库查询语言 select

dml 数据库操作语言 insert delete update

ddl 数据库定义语言 create drop alter

tcl 事物控制。语言   commit rollback

dinstinct 去除重复记录 dinstinct x,y,z

#使用

什么情况下适合添加索引

1.该字段数据量庞大

2.该字段很少进行DML操作由于索引需要维护，DLM操作多的话，也影响检索效率。 

3.该字段经常出现where条件中

sql语句建表varchar必须给定长度，否则会报错

SELECT TIMESTAMPDIFF(YEAR,NOW(),'2017-4-2')

##创建表

create index 索引名 t_student_index on 表名 t_student(sname); 

drop index  t_student_index on t_student;

#三范式

数据库设计三范式: 范式是对数据库要求的规范不是强制性必须这么做

第一:必须有主键，每一个字段是原子性不能再分

主键的作用，是为了确定一个实体的唯一性

主键通常采用数值型或者定长字符串表示

第二:要求数据库中所有非主键字段完全依赖主键，要求数据库中所有非主键字段完全依赖主键，不能产生部分依赖;严格意义上说:尽量不要使用联合主键

多对多关系时，往往存在第三张表，体现两者之间的关系。

第三范式:建立在第二范式基础之上，要求非主键字段不能产生传递依赖于主键字段，即每个字段必须直接依赖于当前表的主键字段

一对一的关系，如妻子和丈夫的关系

如在丈夫中添加了妻子关系的字段需要为其添加一个唯一性约束，让其保持互相的唯一性

数据库的设计尽量遵循三范式，根据实际需求进行取舍，有时可能会拿冗余换速度。

#事物的四个特性：acid

- 原子性a
- 一致性c
- 隔离性i
- 持久性d

# 变量

## 系统变量

变量由系统提供，不是用户定义，属于服务器层面

- 全局变量

  - 查看所有的系统变量

  ```mysq
  SHOW GLOBAL VARIABLES	//查看全局变量	LIKE '%char%'
  SHOW SESSION VARIBLES	//查看一次会话的变量
  ```

- 会话变量

```mysq
SELECT @@global|[session].character_set_client	//查看某个变量,注意.
SET @@global|[session].系统变量名 = 值	SET @@global.autocommit = 0	//修改
```



## 自定义变量(session作用域)

- 用户变量

```mysq
set @name = 'xiaowang'
set @name := 'xiaowang'		//设置变量,再次设置相当于修改
```

```mysq
SELECT @name	//查询变量
```

- 局部变量		生命周期只在一个begin end 块中

```mysql
DECLARE result INT DEFAULT 0;
```

# 存储过程及函数

存储过程是一组预先定义好的sql语句，优点

- 预先编译
- 减少与数据库的通信次数
- 减少网络传输信息量

## 创建

```mysql
DELIMITER $			#申请一个标识符
CREATE PROCEDURE myPro()	#创建存储过程
BEGIN
	SELECT * FROM tbl_emp;
END $
```

参数会有参数模式，	In stuname VARCHAR(20);

模式共有三种	*IN OUT INOUT*

IN	调用方传输过来的值

OUT	该参数可以作为返回值，返回类型

INOUT	该参数既可以作为输入也可以作为输出

每条sql语句的结尾必须加分号，存储过程的结尾 通过DELIMETER加自己使用的标识符表示结束

##调用

```mys
CALL 存储函数名(实参列表);
CALL myPro;
```

## IN实例

带参数的

```mysql
DELIMITER $;
CREATE PROCEDURE f1(IN empID INT,IN empName varchar(20))					#创建
BEGIN
	DECLARE result INT DEFAULT 0;			#定义变量
	SELECT COUNT(1) INTO result FROM tbl_emp t WHERE t.emp_id > empID AND t.emp_name LIKE CONCAT('%',empName,'%'); 	#变量的动态赋值
	SELECT IF(result>0,'1','0');
END $;
	
CALL f1(1060,'Jerry')					#调用
```

## OUT实例

```mysql
DELIMITER $;
CREATE PROCEDURE f2(IN empID INT,IN empName varchar(20),OUT result INT)					#创建
BEGIN
	SELECT COUNT(1) INTO result FROM tbl_emp t WHERE t.emp_id > empID AND t.emp_name LIKE CONCAT('%',empName,'%'); 	#变量的动态赋值
END $;

CALL f2(1060,'e',@count);
SELECT @count;
```

```mysql
也可以同时输出多个值
SELECT x,y INTO a,b	即可
```

