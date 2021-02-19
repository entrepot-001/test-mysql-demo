## 结构化程序查询语言（SQL）
	数据模型三要素
		数据结构、数据操作、数据完整性约束
	数据库存放特点
		1.数据存放到表中，然后表放到库里
		2.一个库中有很多表，每张表都有唯一的表名来标识
		3.表中有多个“字段”，等价于“属性”
		4.每行数据，等价于“对象”
	数据库的好处
		1.持久化数据到本地
		2.可以实现结构化查询，方便管理

## 数据库相关概念
	1、DB：数据库，保存一组有组织的数据的容器
	2、DBMS：数据库管理系统，又称为数据库软件（产品），用于管理DB中的数据
	3、SQL:结构化查询语言，用于和DBMS通信的语言

## 数据库存储数据的特点
	1、将数据放到表中，表再放到库中
	2、一个数据库中可以有多个表，每个表都有一个的名字，用来标识自己。表名具有唯一性。
	3、表具有一些特性，这些特性定义了数据在表中如何存储，类似java中 “类”的设计。
	4、表由列组成，我们也称为字段。所有表都是由一个或多个列组成的，每一列类似java 中的”属性”
	5、表中的数据是按行存储的，每一行类似于java中的“对象”。

### MySQL服务的登录和退出   
	方式一：通过mysql自带的客户端
	只限于root用户

	方式二：通过windows自带的客户端
	登录：
	mysql 【-h主机名 -P端口号 】-u用户名 -p密码

	退出：
	exit或ctrl+C

### MySQL的常见命令 
	1.查看当前所有的数据库
	show databases;
	2.打开指定的库
	use 库名
	3.查看当前库的所有表
	show tables;
	4.查看其它库的所有表
	show tables from 库名;
	5.创建表
	create table 表名(

		列名 列类型 约束条件,
		列名 列类型 约束条件
	);
	6.查看表结构
	desc 表名;
	7.删除表
	drop table 表名;
	8.查看服务器的版本
	方式一：登录到mysql服务端
	select version();
	方式二：没有登录到mysql服务端
	mysql --version  或		mysql --V
	9.注释
		单行注释：#注释文字
		单行注释：-- 注释文字
		多行注释：/* 注释文字  */
### 删除

方式1：delete语句 

单表的删除： 
	delete from 表名 【where 筛选条件】

多表的删除：
	delete 别名1，别名2
	from 表1 别名1，表2 别名2
	where 连接条件
	and 筛选条件;

方式2：truncate语句
	truncate table 表名

两种方式的区别【面试题】
	1.truncate不能加where条件，而delete可以加where条件
	
	2.truncate的效率高
	
	3.truncate 删除带自增长的列的表后，如果再插入数据，数据从1开始
		delete 删除带自增长列的表后，如果再插入数据，数据从上一次的断点处开始
	
	4.truncate删除不能回滚，delete删除可以回滚
	
	5.truncate 删除后元素的数组下标改为1，delete删除后元素的下标在原有基础上开始自增
### 常见类型

	整型：int 
		 longint
	小数：
		浮点型double 
		定点型foalt
	字符型：char / varchar
	日期型：
	Blob类型：
### 常见约束

	//使用的时候直接添加到列名的后面，这是属于列约束条件
	NOT NULL	非空

	DEFAULT		默认值（设置默认值）

	PRIMARY KEY	主键   保证唯一性，但是不能为空，一张表中只能有一个主键，多个字段可以组合成一个主键（不推荐）

	UNIQUE 		唯一键 保证唯一性，可以为空

	//下面这两个不经常使用
	整体约束，使用的时候放到最后CONSTRAINT 新名字 约束条件;
	CHECK		查询键

	FOREIGN KEY	外键(在从表添加外键约束，用于引用主表中的某列的值)
	FOREIGN KEY(主键列) REFERENCES 从表名(从键列); 主表中的主键可以对应从表中主(唯一)键
	左连接：左边的表+交集部分
	右连接：右边的表+交集部分
	
	行约束：PRIMARY KEY（主键）、NOT NULL（非空）、UNIQUE（唯一）、DEFAULT（默认值）
		   可以再新建完表后设置约束条件
		   ALTER TABLE 表名 MODIFY COLUMN 列名 列的类型 约束条件	
	
	整体约束:FOREING KEY

	组合主键：只要组合有任意一个值不相等即可

	AUTO_INCREMENT:代表自增；必须和有唯一性的键一起使用；必须是数值类型的数据；
				   一个表中可以有多个列带有这个AUTO_INCREMENT限定；
				   可以通过SET AUTO_INCREMENT=3设置其实值；
	在修改表时候
	添加列级约束：ALTER TABLE 表名 MODIFY COLUMN 字段名 字段类型 约束类型;
	添加表级约束：ALTER TABLE 表名 ADD [CONSTRAINT 约束名] 约束类型（字段名） [外键引用];

	删除非空约束：ALTER TABLE 表名 MODIFY COLUMN 字段名 字段类型 NULL;
	删除默认约束：ALTER TABLE 表名 MODIFY COLUMN 字段名 字段类型;
	删除主键约束：ALTER TABLE 表名 DROP PRIMARY KEY;
	删除唯一约束：ALTER TABLE 表名 DROP INDEX 字段名;
	删除外键约束：ALTER TABLE 表名 DROP FOREING KEY 外键名;
## 事务:
	事务并发问题如何发生？
		当多个事务同时操作同一个数据库的相同数据时
	事务的并发问题有哪些？
		脏读：事务A读到了事务B没有提交的数据
		不可重复读：事务A，重复读取同一条数据，读取的结果不一致
		幻读：事务A读取数据时，事务B更新，导致事务A读取到的是未更新的数据
	如何避免事务的并发问题？
		通过设置事务的隔离级别
		1、READ UNCOMMITTED Read unconmmitted（读未提交）
		2、READ COMMITTED   Read committed 	 （读提交）可以避免脏读
		3、REPEATABLE READ 	repeatable read	 （可重复读）可以避免脏读、不可重复读和一部分幻读
		4、SERIALIZABLE		Serializable     （串行化）可以避免脏读、不可重复读和幻读
	设置隔离级别：
		set session|global  transaction isolation level 隔离级别名;
	查看隔离级别：
		select @@tx_isolation;

## 视图
含义：理解成一张虚拟的表

视图和表的区别：
	
	视图	使用方式完全相同	不占用物理空间，仅仅保存的是sql逻辑
	表	使用方式完全相同	占用物理空间

视图的好处：

	1、sql语句提高重用性，效率高
	2、和表实现了分离，提高了安全性

### 视图的创建
	语法：
	CREATE VIEW  视图名
	AS
	查询语句;
### 视图的增删改查
	1、查看视图的数据 ★
	
	SELECT * FROM my_v4;
	SELECT * FROM my_v1 WHERE last_name='Partners';
	
	2、插入视图的数据
	INSERT INTO my_v4(last_name,department_id) VALUES('虚竹',90);
	
	3、修改视图的数据
	
	UPDATE my_v4 SET last_name ='梦姑' WHERE last_name='虚竹';
	
	4、删除视图的数据
	DELETE FROM my_v4;
### 某些视图不能更新

	1.包含以下关键字：分组函数、distinct、group by、having、union或者union all
	2.常量视图
	3.select中包含字查询
	4.存在连接表
	5.from后面跟的是一个视图时，不能更新
	6.where子查询引用了from子句中的表
### 视图逻辑的更新
	#方式一：
	CREATE OR REPLACE VIEW test_v7
	AS
	SELECT last_name FROM employees
	WHERE employee_id>100;
	
	#方式二:
	ALTER VIEW test_v7
	AS
	SELECT employee_id FROM employees;
	
	SELECT * FROM test_v7;
### 视图的删除
	DROP VIEW test_v1,test_v2,test_v3;
### 视图结构的查看	
	DESC test_v7;
	SHOW CREATE VIEW test_v7;


## 系统变量
### 一、全局变量
作用域：针对于所有会话（连接）有效，但不能跨重启

	1.查看所有全局变量
	SHOW GLOBAL VARIABLES;
	2.查看满足条件的部分系统变量
	SHOW GLOBAL VARIABLES LIKE '%char%';
	3.查看指定的系统变量的值
	SELECT @@global.autocommit;
	4.为某个系统变量赋值
	SET @@global.autocommit=0;
	SET GLOBAL autocommit=0;

### 二、会话变量
作用域：针对于当前会话（连接）有效

	1.查看所有会话变量
	SHOW SESSION VARIABLES;
	2.查看满足条件的部分会话变量
	SHOW SESSION VARIABLES LIKE '%char%';
	3.查看指定的会话变量的值
	SELECT @@autocommit;
	SELECT @@session.tx_isolation;
	4.为某个会话变量赋值
	SET @@session.tx_isolation='read-uncommitted';
	SET SESSION tx_isolation='read-committed';

## 自定义变量
### 一、用户变量
声明并初始化：

	SET @变量名=值;
	SET @变量名:=值;
	SELECT @变量名:=值;
赋值：

	方式一：一般用于赋简单的值
	SET 变量名=值;
	SET 变量名:=值;
	SELECT 变量名:=值;


	方式二：一般用于赋表 中的字段值
	SELECT 字段名或表达式 INTO 变量
	FROM 表;
使用：

	select @变量名;

### 二、局部变量
声明：

	declare 变量名 类型 【default 值】;
赋值：

	方式一：一般用于赋简单的值
	SET 变量名=值;
	SET 变量名:=值;
	SELECT 变量名:=值;


	方式二：一般用于赋表 中的字段值
	SELECT 字段名或表达式 INTO 变量
	FROM 表;

使用：

	select 变量名

二者的区别：
		
||作用域|				定义位置|				语法|
|:-:|:-|:-|:-|
|	用户变量|	当前会话|				会话的任何地方|		加@符号，不用指定类型|
|	局部变量|	定义它的BEGIN END中| 	BEGIN END的第一句话	|一般不用加@,需要指定类型|

## 存储过程
含义：一组经过预先编译的sql语句的集合
好处：

	1、提高了sql语句的重用性，减少了开发程序员的压力
	2、提高了效率
	3、减少了传输次数

分类：

	1、无返回无参
	2、仅仅带in类型，无返回有参
	3、仅仅带out类型，有返回无参
	4、既带in又带out，有返回有参
	5、带inout，有返回有参
	注意：in、out、inout都可以在一个存储过程中带多个
### 创建存储过程
语法：

	create procedure 存储过程名(in（参数作为输入）|out（参数作为输出）|inout（参数作为输入/出） 参数名  参数类型,...)
	begin
		存储过程体
	end $

类似于方法：

	修饰符 返回类型 方法名(参数类型 参数名,...){

		方法体;
	}

注意

	1、需要设置新的结束标记
	delimiter 新的结束标记
	示例：
	delimiter $

	CREATE PROCEDURE 存储过程名(IN|OUT|INOUT 参数名  参数类型,...)
	BEGIN
		sql语句1;
		sql语句2;
	END $

	2、存储过程体中可以有多条sql语句，如果仅仅一条sql语句，则可以省略begin end

	3、参数前面的符号的意思
	in:该参数只能作为输入 （该参数不能做返回值）
	out：该参数只能作为输出（该参数只能做返回值）
	例子
		create procedure 存储过程名 (out 返回1 字段类型,out 返回2 字段类型)
		begin
			select 字段1 into 返回1,字段2 into 返回2
			from 表
		end$
	调用
		call 存储过程名(变量,变量)
	inout：既能做输入又能做输出
	

### 调用存储过程
	call 存储过程名(实参列表)$

	使用out时，一般会自定义一个变量，out 后面+变量，最后用select 变量查询值
### 删除存储过程
	drop procedure 存储过程名1，存储过程名2（一次可以删除一个或多个存储过程）
### 查看存储过程信息
	show create procedure 存储过程名;
## 函数

### 创建函数

学过的函数：LENGTH、SUBSTR、CONCAT等
语法：

	CREATE FUNCTION 函数名(参数名 参数类型,...) RETURNS 返回类型
	BEGIN
		函数体
	
	END

### 函数操作
调用函数：SELECT 函数名（实参列表）<br>
查看函数：show create function 函数名<br>
删除函数：drop function 函数名
### 函数和存储过程的区别

				关键字		调用语法			返回值			应用场景
	函数     FUNCTION	   SELECT 函数()	 	一个			用于查询结果为一个值并返回时
	存储过程 PROCEDURE 	   CALL存储过程()	可以有0个或多个		一般用于更新

## 流程控制结构
### 顺序结构
程序从上到下一次执行
### 分支结构
程序从两条或多条路径中选择一条执行<br>
if函数：实现简单的双分支结构；语法IF(表达式1,表达式2,表达式3);当表达式1成立时，返回表达式2，否则返回表达式3<br>
case函数<br>
情况一：类似于switch语句，一般用于等值判断

	case 变量|表达式|字段
	when 要判断的值1 then 返回值1或语句1
	when 要判断的值2 then 返回值2或语句2
	...
	else 返回值3或语句3
	end case;
情况二：类似于多重if语句，一段用于区间判断

	case 
	when 要判断的条件1 then 返回值1或语句1
	when 要判断的条件2 then 返回值2或语句2
	...
	else 返回值3或语句3
	end case;
可以作为表达式嵌套使用，也可以独立使用；这里when最后默认存在break的效果

### 循环结构
程序在一定条件下，重复执行一段代码<br>
while语法：

	一般在这个位置要设置一个变量并赋值
	[标签:] while 循环条件 do
		循环体;
	end while [标签]; 
iterate类似于 continue，结束本次循环，继续下一次<br>
leave  类似于 break，跳出，结束当前所在的循环

## TCL事务控制语言
	一个或者一组sql语句组成一个执行单元
### 事务的属性ACID
	原子性：一个事务一个不可分割的工作单位，不执行或者全执行
	一致性：一个事务从一个一致状态，转到另一个一致状态
	隔离性：一个事务的执行，不被其他事务干扰，非并发
	持久性：一个事务执行后，是一个永久性的操作

### 其他
MyISAM引擎：.frm 存放结构，.myd 存放表数据，.myi 存放表索引<br>
数据库分为连接层、服务层、引擎层、存储层<br>
MySql存储引擎一般用InnoDB(适合高并发支持事务)、MyISAM(性能高)<br>
第一范式：属性不可分割
第二范式：所有的非主属性都依赖于主属性
第三范式：非主属性依赖主键
第四范式：不能一对多
#### MyISAM和InnoDB引擎区别
MyISAM：不支持事务、不支持外键、存储时三个文件：.frm存放结构、.myd存放数据、.myi存放索引、高性能查询多的时候推荐使用
InnoDB：支持事务、支持外键、在主存中建立专用的缓冲池用于告诉缓冲数据、索引、执行删改操作较多的时候使用


