### 排序
	语法格式：SELECT *FROM 表名 ORDER BY 列名 ；
			 不描述升降序的时候，默认是升序；
			 DESC降序ASC升序
	实例：
		SELECT *FROM faccount WHERE fid>=40 ORDER BY ftime;//筛选fid>=40,然后按ftime排序

		SELECT *FROM faccount ORDER BY ftime;//按单列ftime排序

		SELECT *FROM faccount ORDER BY ftime,fid;//多个列，先按ftime排序的，然后按fid排序

		SELECT IFNULL(fmoney,0),flname FROM faccount ORDER BY IFNULL(fmoney,0);
		//使用函数，显示IFNULL(fmoney,0),flname 两列，按IFNULL(fmoney,0)排序

		SELECT fid AS C11 fmoney FROM faccount ORDER BY "C11" DESC;
		//使用重命名排序，显示C11 fmoney两列,按C11排序

		SELECT (fid*10)AS c11,flname,fmoney,fzhangHu FROM faccount ORDER BY (fid*10);
		//使用表达式排序，显示(fid*10)、flname、fmoney、fzhangHu四列，按(fid*10)排序

### 单行函数(这些函数都是可以重新起名字的）
	字符函数
	1.concat函数
		功能：拼接多个字段
		SELECT CONCAT(字符1,字符2) FROM 表名;

	2.ifnull函数
		功能：判断某字段或者表达式是否为null，不为null，返回值，为null返回0；
		SELECT IFNULL(字段名,0) FROM 表名;

	3.isnull函数
		功能：值为null返回1;值不为null返回0
		SELECT ISNULL(字段名) FROM 表名；

	4.length函数
		功能：测量长度
		SELECT LENGTH('需要测长度的串')；
		
	5.upper/lower函数
		功能：把字母变大/小写
		SELECT LOWER(flname),fmoney FROM faccount;

	6.insert函数
		功能：在a1字符串中找到a2字符串的首次出现的位置，从1开始，返回位置；若找不到则返回0
		SELECT INSTR('die you win or you die.','die') 出现;返回值为1；

	7.substr函数
		功能：截取字符串a1从第x个开始
		SELECT SUBSTR('hell world',6) out_put;##截取字符从第6个开始，即world
		功能：截取字符串a2从x开始取y个
		SELECT SUBSTR('hell world',1,4) out_put;##截取从1开始取4个，即hell

	8.trim函数
		功能：去掉前后多个'aa'字符串
		SELECT TRIM('aa' FROM 'aaaaaahello world aaaaa'); 返回hell world a
		hello world 前有6个'a'组成了3个'aa',后面5个'a'组成了2个'aa'会留下一个'a'

	9.lpad函数
		功能：若长度不够左填充；长度足够截取（从左边开始截取）
		SELECT LPAD('hello world',20,'abcd');abcdabcdahello world
	
	10.rpad函数
		功能：若长度不够右填充；长度足够截取（从右边开始截取）
		SELECT RPAD('hello world',20,'abcd');hello worldabcdabcda

	11.replace函数
		功能：替换
		SELECT REPLACE('win die win ','win','victory')替换;victory die victory

	数学函数
	
	1.round函数
		功能：第一个数的绝对值四舍五入
			 存在第二个数的时候，保留小数点后第二个数，不存在直接保留整数
		SELECT ROUND(5.555,1); 5.6

	2.ceil函数
		功能：向上取整数
		SELECT CEIL(-1.2);结果-1

	3.floor函数
		功能：向下取整
		SELECT FLOOR(-1.2);结果-2

	4.truncate函数
		功能：截断/对第一个数进行截断，即保留小数点后2位
		SELECT TRUNCATE(1.999,2);结果1.99
	5.mod函数
		功能：取模
		SELECT MOD(-10,3);计算过程-10-((-10)/(-3))*(-3)=-10-3*(-3)=-10-(-9)=-1

### 日期函数
	1.now函数
		功能：返回当前系统日期+时间
		SELECT NOW();
	
	2.curdate函数
		功能：返回当前系统日期
		SELECT CURDATE();

	3.curtime函数
		功能：返回当前时间
		SELECT CURTIME();

	4.year/month/day/hour/minute/second函数
		功能：获取时间类字符的年/月/日/小时/分钟/秒
		SELECT HOUR(NOW());

	5.DATEDIFF函数
		功能：获取两个时间中间的差值
		SELECT DATEDIFF('时间1','时间2');

	6.monthname函数
		功能：返回英文月份
		SELECT MONTHNAME('时间');

	符号代表：
		%y--二位的年份	%Y--四位的年份	%m--月份补零	 %c--月份不补零   %d--日期补零
		%H--24小时制		%h--12小时制		%i--分钟补零	 %s--补零秒

	5.ste_to_date函数
		功能：把字符转成日期;第一个是字符型的日期，第二个是日期
		SELECT STR_TO_DATE(now(),'%y-%m-%d'):输出当前时间：19-5-20

	6.date_format函数
		功能：将日期转化为字符串;
		SELECT DATE_FORMAT(NOW(),'%y-%c-%d');输出当前时间19-5-20；

	7.datefill函数
		功能：返回第二个日期-第一个日期的天数
		SELECT DATEFILL(NOW(),'1998-8-21')AS 新名字;

### 其他函数
	1.version函数
		功能：查看当期数据库版本
		SELECT VERSION();

	2.database函数
		功能：显示当前数据库名
		SELECT DATABASE();
			SHOW DATABASES;显示全部数据库名

	3.user函数
		功能：显示当前用户名
		SELECT USER();

	4.password函数
		功能：自动加密
		SELECT PASSWORD('需要加密的语句');

	5.md5函数
		功能：自动加密
		SELECT MD5('需要加密的语句');

### 流程控制
	1.if函数
		功能：相当于三元运算符
		SELECT fid,fmoney,IF(fmoney>=100,'超出','正常') AS '新名字' 
		FROM faccount;
	
	2.case1函数
		功能：和switch的效果基本相同
		SELECT fid,fmoney,
		CASE (CEIL(fid/10))
		WHEN 1 THEN fmoney*10
		WHEN 2 THEN fmoney*20
		WHEN 3 THEN fmoney*30
		ELSE fmoney
		END AS 新名字
		FROM faccount;
		//将(fid/10)向上取整得到整数然后与when中的数值进行比较，最后输出fid、fmoney、新名字散列
	3.case2函数
		功能：和switch相同		
		SELECT fid,fmoney,
		CASE
		WHEN (CEIL(fid/10)) THEN fmoney*10
		WHEN (CEIL(fid/10)) THEN fmoney*20
		WHEN (CEIL(fid/10)) THEN fmoney*30
		ELSE fmoney
		END AS 新名字
		FROM faccount;
		//将(fid/10)向上取整得到整数然后与when中的数值进行比较，最后输出fid、fmoney、新名字散列

### 分组函数
	对一下这些函数可以添加distinct 去重
				可以忽略null
				可以where 添加条件	
				' '或者AS 改名字
				和分组函数一同查询的位置是在group by后的having 追加条件中
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
			分组筛选前	原始表			GROUP BY之前 	WHERE
			分组筛选后	分组后的结果集	GROUP BY之后 	HAVING
			
			分组函数做条件，一定放到HAVING函数里
			能用分组前筛选，就尽量用分组前筛选
	