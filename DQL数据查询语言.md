# DQL
	(数据查询语言)--细心   基本数据库的语法 真实情况不存在这个标准
## 数据库顺序
	1.写的顺序:
			select--from--where--group by--having--order by--limit
			显示----来自----条件----分组----补充条件----排序----制表
	2.运行的顺序
			from--where--group by--having--select--order by--limit
			来自----条件----分组----补充条件----显示----排序----制表
	3.代表意义
			FROM	:前面为需要显示的列名
			WHERE	:进行筛选的条件（条件在原始表上存在）
			GROUP BY:按xx分组
			HAVING	:补充查询条件（条件在原始表上不存在）
			ORDER BY:排序	ASC从小到大升序；DESC从大到小降序
			LIMIT	:取出指定行的记录，产生一张表,并将结果返回。
	Having和Where区别：where在分组前过滤数据无聚合函数，having在分组后过滤数据有聚合函数；where效率高
	distinct 去重
	concat   拼接关键字 字段值为null和任何字段进行拼接结果都是null


### 列
	1.添加列
		ALTER TABLE 表名 ADD 列名 数据类型 约束;
	
	2.修改列的数据类型或者约束
		ALTER TABLE 表名 MODIFY 列名 数据类型 约束；

	3.修改列名
		ALTER TABLE 表名 CHANGE 旧列名 新列名 数据类型 约束；
	
	4.删除列
		ALTER TABLE 表名 DROP 列名
### 表
	1.添加数据，在添加时，除了Int和double型以外，其他都是需要加''(单引号)的
		INSERT INTO 表名（列名1，列名2，列名3）VALUES(数据1，数据2，数据3)	
	
	2.添加数据，列名中不写主键最常用
		INSERT INTO 表名（列名1，列名2）VALUES (数据2，数据3)
	
	3.添加数据，可以不写列名
		INSERT INTO 表名 VALUES(数据1，数据2，数据3);

	4.添加数据，批量写入
		INSERT INTO 表名 (列名1，列名2，列名3)VALUES(数据1,数据2,数据3),(数据1,数据2,数据3)
### 修改更新
	1.给列1整列设值为 值1，列2设值 值2
		UPDATE 表名 SET 列1=值1,列2=值2 WHERE条件

	2.删除表中数据
		a.DELECTE FROM表名 WHERE 条件;
		  一条一条删除，如果没有设置条件，就是删表，不清空auto_increment记录的数
		b.DROP TABLE表名;
		  直接将表删除，重新建表，auto_increment清0

	3.存储的乱码
		因为编码方式不同，只需要输入他就可以了(临时改进)
		SET names gbk;

### 查询数据项
	1.查询2000~10000之间的数,也可以是日期
		查询列(zmoney)大于等于2000小于等于10000的值，并显示
		SELECT *FROM zhangwu（表名） WHERE zmoney（列名）>=2000 AND zmoney<=10000;
		SELECT *FROM zhangwu（表名） WHERE zmoney（列名） BETWEEN 2000 AND 10000;

	2.查询是in（x1,x2,x3）是否有x1,x2,x3
		查询列(zmoney)中等于1000，8000，10000的项并输出
		SELECT *FROM zhangwu WHERE zmoney=1000 OR zmoney=8000 OR zmoney=10000
		SELECT *FROM zhangwu WHERE zmoney IN(1000,8000，100000);

	3.查询单个（多个）列，同时还可以计算
		SELECT 列名1，列名2 FROM 表名;

	4.查询常量
		SELECT 常量;
		只是显示常量，不显示其他的

	5.给列重命名，只是改变了输出时候的名字，对原来的表没有影响
		SELECT 列名 AS 新列名，列名2 AS 新列名2 FROM 表名 WHERE 条件
		SELECT 列名    新列名	 FROM 表名(适合该单列)

	6.去除重复记录
		SELECT distinct 关键字 FROM 表名

### 查询条件
	1.修改条件的写法
		大于  	：id>6;
		大等于	：id>=6;
		小于  	：id<6;
		小等于	：id<=6;
		等于  	：id=6;
		不等于	：id!=6;
		不等于	：id<>6;
		安全等于 ：id<=>6;		
		与&&	 ：and
		或|| 	：or
		非!  	：not
	2.模糊查询配合通配符来算
		like:
			%：可以是任意个数的字符
			_：一个字符
			可以通过\ 来转义字符；也可以用 escape 关键字来声明转义字符
		查询到zname最后两个字是"支出"的项
			SELECT *FROM zhangwu(表名) WHERE zname(列名) LIKE '%支出';

		IS NOT NULL:不是空 为空0，非空1		
		IS NULL:为空 空1，非空0

		xx between xxx and xxy  从xxx到xxy 效果相同 xx>=xxx and xx<=xxy

		xx in('xx1','xx2')  xx的值为xx1或xx2 效果相同 xx='xx1' or xx='xx2'
### 运算
	1.加法+
		SELECT 数值+数值；
			直接计算
		SELECT 字符+数值；
			字符先转换成数值类型，然后求和
		SELECT null+值;
			结果都为null
### 进阶1：基础查询
	语法：
	SELECT 要查询的东西 FROM 表名;
	SELECT * FROM 表名; 
		代表显示这个表里的全部内容
	类似于Java中 :System.out.println(要打印的东西);
	特点：
	①通过select查询完的结果 ，是一个虚拟的表格，不是真实存在
	② 要查询的东西 可以是常量值、可以是表达式、可以是字段、可以是函数

### 进阶2：条件查询
	条件查询：根据条件过滤原始表的数据，查询到想要的数据
	语法：
	select 
		要查询的字段|表达式|常量值|函数
	from 
		表
	where 
		条件 ;

	分类：
	一、条件表达式
		示例：salary>10000
		条件运算符：
		> < >= <= = != <>
	
	二、逻辑表达式
	示例：salary>10000 && salary<20000
	
	逻辑运算符：

		and(&&):两个条件如果同时成立，结果为true，否则为false
		or (||)：两个条件只要有一个成立，结果为true，否则为false
		not(! )：如果条件成立，则not后为false，否则为true

	三、模糊查询
	示例：last_name like 'a%'

### 进阶3：排序查询		
	语法：
	select
		要查询的东西
	from
		表
	where 
		条件
	order by 排序的字段|表达式|函数|别名 【asc|desc】

	
### 分组函数
	对一下这些函数可以添加distinct 去重
				可以忽略null
				可以where 添加条件	
				' '或者AS 改名字
				和分组函数一同查询的字段要求是group by后的字段
		1.sum函数
			功能：求和，zmoney列名，只能放数值型的才有意义
			SELECT SUM(zmoney) FROM zhangwu; 
			
		2.avg函数
			功能:平均值
			SELECT AVG(zmoney) FROM zhangwu;

		3.max函数	
			功能：极大值
			SELECT MAX(zmoney) FROM zhangwu;

		4.min函数
			功能：极小值
			SELECT Min(zmoney) FROM zhangwu;

		5.count函数
			功能：记非空值的个数
			count(*)、count(1)都是计算行数
			SELECT COUNT(zmoney)FROM zhangwu;

	分组查询函数（传入一组值，然后返回一个值）
		推荐分成多个部分进行运算
		1.查询zname中相同项的和并输出对应项和对应金额,前面的zname是用来显示
			SELECT SUM(zmoney) ,zname FROM zhangwu GROUP BY zname;	
	
		2.模糊查询支出然后求和
			SELECT SUM(zmoney) ,zname 
			FROM zhangwu
			WHERE zname LIKE '%支出'
			GROUP BY zname;
	
		3.模糊查询支出然后求和，最后排序升序
			SELECT SUM(zmoney) ,zname 
			FROM zhangwu 
			WHERE zname LIKE '%支出'
			GROUP BY zname
			ORDER BY zmoney;

		4.当存在:OR 1/OR ture 对的

		5.对fmoney的和>1000，然后筛选出fid<10,逆序使用
			SELECT SUM(fmoney),fid
			FROM faccount 
			WHERE fid<10
			GROUP BY fid
			HAVING SUM(fmoney)>1000
			ORDER BY fid DESC ;
		特点：
						数据源			位置				关键字
			分组筛选前	原始表		   GROUP BY之前 	WHERE
			分组筛选后	分组后的结果集	GROUP BY之后 	HAVING
			
			分组函数做条件，一定放到HAVING函数里
			能用分组前筛选，就尽量用分组前筛选

## 连接
	等值连接
		当我们要查询的字段来自多个表，用连接查询
		当操作时存在有歧义(ambiguous)的列，用表名来限定
		可以为表名起别名(若起了别名则不能用之前的名字)
		n表连接至少n-1个条件限定
			1.例1
				功能：把boys.id和beauty.id对应连接起来，输出name\boyname\userCP三列数值
				SELECT NAME,boyname,userCP
				FROM boys,beauty 
				WHERE boys.id=beauty.id;
	非等值连接

	自连接（特殊的等连接）
		例子：自连接是需要两列能够比较的列，显示的是员工，其领导，连接的领导,然后再连一个领导
			SELECT e.employee_id 员工id,e.manager_id  领导id,m.employee_id  领导id
			m.manager_id 领导的领导id
			FROM employees e,employees m
			WHERE e.manager_id=m.employee_id AND e.employee_id<150 AND e.manager_id<120
			ORDER BY e.employee_id DESC;
		
### 外连接
​	左外连接
​		模板：
​			SELECT * FROM 表a LEFT (OUTER) JOIN 表b ON 条件
​		例子：把category 和 porduct 左left连接
​			SELECT * FROM category c  LEFT JOIN porduct p ON c.`cid`=p.`category_id`
​	右外连接
​		模板：
​			SELECT * FROM 表a RIGHT (OUTER) JOIN 表b ON 条件
​		例子：把category 和 porduct 右right连接
​			SELECT * FROM category c RIGHT JOIN porduct p ON c.`cid`=p.`category_id`
​	区别：左left	 交集的数据+左边数据
​		 右right	 交集的数据+右边数据

## 子查询
	模板：把一个select 的结果当成另一个select 的条件
		SELECT * FROM 表a WHERE 表a.对应键=(SELECT 对应键 FROM 表b WHERE 表b.对应键='关键字')
	例子：先从category中找到 化妆品 的cid，然后等于product 中的category_id
		SELECT * FROM product p 
		WHERE p.`category_id`=(SELECT cid FROM category c WHERE c.`cname`='化妆品')
	也可以用隐式内联接俩实现
		SELECT cid,cname,pid,pname,price FROM category c INNER JOIN product p 
		ON c.`cid`=p.`category_id` AND c.`cname`='化妆品'
## 事务---在service层使用
	一个事件有多个组成单元，这多个组成单元同时成功/失败，这些组成单元就是一个事务
	
	mysql事务
		1.显示的开启一个事务 start transaction
		2.事务提交 commit
		3.事务回滚 rollback

### DbUtils事务操作
	例子
```java
	Connection conn=null;
	try{
		QueryRunner qr = new QueryRunner();
		//通过工具获得Connection
		conn = DbUtils(工具).getConnection();
		//开启事务
		conn.setAutoCommit(false);
		qr.update(conn,sql1);
		qr.updata(conn,sql2);
		conn.commit();
	}catch(SQLException e){
		try{
			//回滚
			conn.rollback();
		}catch(SQLException el){
			el.printStackTrace();
		}
		e.printStackTrace();
	}finally{
		try{
			conn.commit();
		}catch(SQLException e){
			e.printStackTrace();
		}
	}
```
	QueryRunner
		有参构造 QueryRunner qr = new QueryRunner(DataSource datasource);
			有参构造是将数据源最为参数传入QueryRunner 它会从连接池中获取一个数据库连接资源操作数据库
			所以直接用Connection参数的update()方法即可
		无参构造 QueryRunner qr = new QueryRunner();
			无参构造没有将数据源作为参数传入QueryRunner中 
			使用QueryRunner时 要用有Connection 参数的方法
#### ThreadLocal---变量绑定线程

线程本地变量 一种特殊的线程绑定机制 将变量和线程绑定到一起 每个线程维护一个独立变量副本
相当于map，但是它没有key只有value
# sql99
	sql 在1999年定义的新语法
		语法:
			select 查询列表
			from 表1 别名（连接类型）
			join 表2 别名
			on   连接条件
			（where 筛选条件）
			（group by 按xx分组）
			（having 筛选条件）
			（order by 排序）
		分类：
			内连接 inner
			内联接	等值练级
			两个表可以交换位置
				SELECT last_name,department_name
				FROM employees e
				INNER JOIN departments d
				ON e.`department_id`=d.`department_id`;

			查询员工名、部门名、工种名，并按照部门名排序,连接条件为员工号相同，工种号相同（三表连接）
				SELECT last_name ,department_name,job_title
				FROM employees e
				INNER JOIN departments d ON e.`department_id`=d.`department_id`
				INNER JOIN jobs j ON e.`job_id`=j.`job_id`
				ORDER BY d.department_name;

			自连接,筛选出员工号大于120 小于150的员工号
				SELECT e.employee_id, m.manager_id
				FROM employees e
				INNER JOIN employees m
				ON e.manager_id= m.employee_id
				WHERE e.`employee_id` BETWEEN 120 AND 150
				ORDER BY m.employee_id DESC;

			内联接	非等值连接
			查询员工工资等级>20并且逆序排列
			使用between的时候是有顺序的，between min and max
			count(*)数的是下面第一个表的行数
				SELECT COUNT(*) , grade_level
				FROM employees e
				INNER JOIN job_grades j
				ON e.`salary` BETWEEN j.`lower_sal` AND j.`highest_sal`
				GROUP BY grade_level
				HAVING COUNT(*)>20
				ORDER BY grade_level DESC;

			外连接	
				外链接的时候主键是在前面，然后从键在后面 
				左外left (outer)
					SELECT b.`name`
					FROM beauty b
					LEFT JOIN boys bo 
					ON b.`boyfriend_id`=bo.`id`
					WHERE bo.`id` IS NULL;

				右外	right(outer)
					SELECT b.`name`,bo.*
					FROM boys bo	
					RIGHT JOIN beauty b
					ON b.`boyfriend_id`=bo.`id`
					WHERE bo.`id` IS NULL;

				全外	full (outer)
					全外连接结果=内联接结果+表1中没有但表2有+表2没有单表有
					SELECT b.* ,bo.*
					FROM beauty b
					FULL JOIN boys bo 
					ON b.boyfriend_id= bo.id;

			交叉连接	cross
				结果：等价于笛卡尔积
					SELECT bo.*,b.*
					FROM boys bo
					CROSS JOIN beauty b		
### 子查询
	概念：出现在其他语句中的select语句，又名内查询，放在（）中
	外部的查询语句称为主查询
	出现位置分类：
		SELECT后面：支持表量子查询
		FROM后面：支持表子查询
		WHERE或者HAVING后面：支持表量子查询、行子查询、列子查询
		exists后面（相关子查询）：表子查询

	按结果集分类：
		标量子查询：结果集一行一列

			查询最低工资大于60号部门的最低工资的部门的ID和其最低工资

			1查询60号部门的最工资
				SELECT MIN(salary)
				FROM employees
				WHERE department_id=60;

			2查询每个部门的最低工资,在不添加group by时，只输出最低工资及其编号
				SELECT MIN(salary),department_id
				FROM employees
				GROUP BY department_id

			3获取
				SELECT MIN(salary),department_id
				FROM employees
				GROUP BY department_id
				HAVING MIN(salary)>(
					SELECT MIN(salary)
					FROM employees
					WHERE department_id=60
				);

		行子查询：结果集一行多列
			返回location_id是1400或则1700的部门的所有员工的姓名

			1查询location_id是1400或则1700的部门
				SELECT department_id
				FROM departments 
				WHERE location_id IN(1400,1700)

			2在1的基础上得到所有员工的姓名（DISTINCT去重的意思）
				SELECT last_name
				FROM employees
				WHERE department_id IN(
					SELECT DISTINCT department_id
					FROM departments 
				WHERE location_id IN(1400,1700)
			);
		
			返回其他部门中比job_id为'IT_PROG'部门任一工资低的员工号、姓名job_id,salary
			1查询it_prog部门的任一工资
				SELECT salary
				FROM employees
				WHERE job_id='IT_PROG';
			
			2返回他们的员工号、姓名
				SELECT employee_id,last_name,job_id,salary
				FROM employees
				WHERE salary<ANY(
					SELECT salary
					FROM employees
					WHERE job_id='IT_PROG'
				);
			2返回他们的其他值
				SELECT employee_id,last_name,job_id,salary
				FROM employees
				WHERE salary<(
					SELECT max(salary)
					FROM employees
					WHERE job_id='IT_PROG'
				);

			返回其他部门中比job_id为'IT_PROG'部门所有工资低的员工号、姓名job_id,salary
			1查询it_prog部门的任一工资
				SELECT salary
				FROM employees
				WHERE job_id='IT_PROG';
			
			2返回他们的员工号、姓名
				SELECT employee_id,last_name,job_id,salary
				FROM employees
				WHERE salary<ALL(
					SELECT salary
					FROM employees
					WHERE job_id='IT_PROG'
				);
			2返回他们的其他值
				SELECT employee_id,last_name,job_id,salary
				FROM employees
				WHERE salary<(
					SELECT min(salary)
					FROM employees
					WHERE job_id='IT_PROG'
				);

		ANY:任一一个
		ALL:全部

		列子查询：结果集多行一列
		表子查询：结果集多行多列
### 分页查找
		select 需要显示的项
		from 需要查询的表
		｛
		where 查询条件
		group by 分组
		having	查询条件
		order by 排序 desc逆序；esc正序
		｝
		limit 开始位置，要选择出的条数
		
		分页查询，在jobs表中从第2+1个开始，输出5个数；当起始位置为0时，可以忽略不写
		SELECT *FROM jobs LIMIT 2 ,5;
### 联合查询
	union可以跨表使用，多个表之间没有任何联系关系
		 查询的时候列数一定是相同的
		 多个表中列最好是对应的
		 不希望自动去除重复；在union后添加一个all;即  union all

	相当于把一句有多个用"OR"连接起来的查询条件语句，分解成多条语句，然后用union联合起来

		SELECT *
		FROM jobs
		WHERE min_salary >5000 OR max_salary<16000
		ORDER BY min_salary DESC;

		//上下个代码块的效果是相同的

		SELECT *FROM jobs WHERE min_salary >5000
		UNION 
		SELECT *FROM jobs WHERE max_salary <16000
		ORDER BY min_salary DESC;