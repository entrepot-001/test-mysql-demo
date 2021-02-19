## DDL语言（数据定义语言）
新建一个表的时候需要自己进行定义的东西

desc 表名：查询表的定义

### 库的管理
	1.新建一个数据库	
		CREATE DATABASE [if not exists]新建数据库名字；
		if not exists：如果数据库存在则不新建这个库了

	2.删除数据库
		DROP DATABASE 删除数据库名字；

	3.显示数据库中的表的名字列表
		SHOW DATABASES;

	4.启动数据库
		USE 使用的数据库名；

	5.新建数据表
		CREATE TABLE 表名(
			列名1 数据类型 约束（可以不写）,
			列名2 数据类型 约束,
			列名3 数据类型 约束
		);
		主键约束	PRIMARY KEY：有唯一性
				AUTO_INCREMNT：自动增长
		非空约束 not null	 ：必须填
		唯一约束 ：每一个数据都不一样
		外键约束 ：

	6.删除数据表
		DROP TABLE 删除表名；

	7.显示所有数据表
		SHOW TABLE;

	8.查看表的属性
		DESC 表的名字;

	9.改表名
		RNAME TABLE旧表名 TO 新表名;

	10.复制表
		只复制表的结构
		CREATE TABLE 表名 FROM 被复制的表名;

		复制表的结构和数据
		CREATE TABLE 新表名 
		SELECT *FROM 被复制的表名（旧表）；

		只复制部分表结构
		CREATE TABLE 表名
		SELECT 列1，列2 FROM 被复制的表名
		WHERE 添加需要满足条件（当不需要添加数据的时候可以直接定义成一个恒不等式）
### 对表中的列进行操作
	新建一个新表create table 表名(属性);
	1.添加列
		ALTER TABLE 表名 ADD 列名 数据类型 约束;
	
	2.修改列的数据类型或者约束
		ALTER TABLE 表名 MODIFY 列名 数据类型 约束；

	3.修改列名
		ALTER TABLE 表名 CHANGE 旧列名 新列名 数据类型 约束；
	
	4.删除列
		ALTER TABLE 表名 DROP 列名

## 标识列
又称自增长列，必须要和一个key搭配使用，一个表中只能有一个标识列，标识列只能是数值型<br>
可以通过 SET auto_increment_increment =数值 来修改自增的跨度<br>
修改表设置标识：ALTER TABLE 表名 MODIFY COLUMN 字段名 数据类型 [约束名] auto_increment;<br>
修改表删除标识：ALTER TABLE 表名 MODIFY COLUMN 字段名 数据类型;
