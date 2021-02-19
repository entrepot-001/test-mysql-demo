# 数据操作语言(DML)：
	插入：insert
	修改：update
	删除：delete
		delete和truncate的区别
		delete:一条一条删除数据，配合；可以找回数据，主键不归零
			启动事务,必须是开启这个事务后，删除的数据才可以被恢复
			START TRANSACTION;

			删除数据
			DELETE FROM boys WHERE userCP='4';
			确认删除成功
			SELECT*FROM boys;
			恢复数据
			ROLLBACK;
			确认恢复成功
			SELECT*FROM boys;

		truncate:直接删完，然后新建一个属性相同的表，不可找回数据，主键归零
			TRUNCATE TABLE boys;

			SELECT*FROM boys;
	
## 插入：
	方式一：
	支持多行插入
		子查询
	1.插入值的类型一定要能与列的类型可以兼容
		INSERT INTO beauty(id,NAME,sex,borndate,phone,photo,boyfriend_id)
		VALUES('13','name13','男','2000-01-01','asdf',NULL,1);	
	
	2.不可以为Null的地方一定需要值，可以为null的地方可以直接不写列名和值;也可以写列名、值写null
		INSERT INTO beauty(id,NAME,sex,phone,boyfriend_id)
		VALUE('14','name14','男','122000000',5);

	3.列的位置不是固定不变的，但是填写的值一定是要类型匹配的
		INSERT INTO beauty(sex,id,NAME,phone)
		VALUE('15','name15','男','1232312321');

	4.列和值的个数必然一致
	5.可以省略列名，直接添加数据，但是位置要和表中的一致
		INSERT INTO beauty()
		VALUES('16','name13','男',NULL,'12312312',NULL,1);	
	
	方式二：
	不设置的值全部都是默认
		INSERT INTO beauty
		SET id='17',NAME='name17';

## 修改：
	模糊查询，然后改单表中的值			
		UPDATE beauty SET NAME='yangdi'
		WHERE NAME LIKE '%ame%';
	
	具体查询，然后改单表的值
		UPDATE beauty SET NAME='123'
		WHERE id=12

	具体查询，然后改表
		把boys和beauty两个表连接起来，把boys表中xxxx对应的beauty.phone改成111
		UPDATE boys bo
		INNER JOIN beauty b ON bo.id = b.boyfriend_id
		SET b.phone='111'
		WHERE bo.boyName='xxxx';
	
## 删除：
	单表删除
		DELETE FROM 表名WHERE筛选条件

	多表删除
		DELETC FROM 表名 
		连接方式（inner）表名 on 连接条件
		WHERE 筛选条件 