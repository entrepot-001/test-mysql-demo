## JAVA连接SQL数据库的时候需要使用JDBC
看到link，mysql服务没有开启
### eclipse导入jar文件
	1.新建一个文件夹，用来存放jar文件
	2.右键jar文件，Builder Path->addxxxxx  结束
 	3.看不了源代码的时候：点击Attach Source弹出的页面中,点击External location(外部位置)
					   添加sources（来源）.jar文件
	4.所有的jar的压缩包，解压后，它的 API==index.html
	5.数据库必须要配置的4项
		数据驱动名称:setDriverClassName("com.mysql.jdbc.Driver");
					com.mysql.jdbc.Driver
		数据库地址:	setUrl("jdbc:mysql"//localhost:3306/aaa");
					jdbc:mysql://localhost（IP）:3306（端口号）/aaa（数据库名）
		用户名	：	setUsername("用户名");
		密码		:	setPassword("密码")；
	  选择配置的4项
		最大连接数量:setMaxActive(int);	int类型的数字，表示最大连接数
		最小空闲连接:setMinldle(int);		int类型的数字，表示最大连接数
		最大空闲连接:setMaxldle(int);		int类型的数字，表示最大连接数
		初始化连接  :setIinitialSize(int);int类型的数字，表示最大连接数

### JDBC
	1.概念：java访问数据库的规范
	
	2.本质：是一组接口或这类，面向接口的开发对象，访问数据库的一种规范

	3.JDBC操作数据库的6步：
		1.注册驱动（相同）
			通过反射技术，把驱动添加到内存中
			使用的是固定形式：	Class.froName("com.mysql.jdbc.Driver");

		2.获得连接（相同）
			通过DriverManager.getConnecion(String url,String user,Sting password)
			返回的是一个接口connecion
			url(固定形式)：jdbc:mysql://连接主机IP：端口号/数据库名
			user:用户名 passwork:密码
			//一行语句，获得连接
			Connecion c= DriverManager.getConnecion
				("jdbc:mysql://连接主机IP:端口号/数据库名","用户名","密码");
			//四行语句，获得连接，和上面一行语句的效果相同
			String msql ="jdbc:mysql://连接主机的IP:端口号/数据库名";
			String muser = "root";
			String mpasswork = "123";
			Connecion c = DeiverManager.getConnecion(msql,muser,mpasswork);
		
		3.获得语句执行平台（相同）
			通过数据库连接对象，获取sql语句执行者对象
			Statement sta = con.createStatement();
			返回类型是Statement这个接口
		
		4.执行sql语句
			String sql ="INSERT INTO zhangwu(zname,zmoney) VALUES('娱乐支出',1000);";
			设置一个字符串里面存放的是，sql语句的操作，任意操作都是可以的
			int row = sta.executeUpdate(sql);
				返回的是row的值为操作成功的行数
			RecultSet re = sta.executeQuery(sql);
				返回的是结果集

		5.处理数据
			对应操作
	
		6.释放资源（相同）
			连接者对象.close;
			执行者对象.close;
	
	4.实例
```java	
	public static void main(String[] args) throws ClassNotFoundException, SQLException {
		// 1.数据库注册驱动，最好是通过反射技术，把驱动类加到内存中
		Class.forName("com.mysql.jdbc.Driver");

		// 2.数据的获得
		Connection con = DriverManager.getConnection
				("jdbc:mysql://localhost:3306/aaa", "root", "123");
		
		// 3.通过语句执行平台，通过数据库连接对象，获取sql语句执行者对象
		Statement sta = con.createStatement();
		
		// 4.执行sql语句
		int row = sta.executeUpdate("INSERT INTO zhangwu(zname,zmoney)
					VALUES('娱乐支出',1000),('收入',1230);");
		//6.关闭数据流
		sta.close();
		con.close();
	}
```		

	5.几个不同的函数的区别
		executeQuery()
		通过executeQuery()下达对sql指令，把结果存在RsultSet类中，以供我们使用
       	作用：用于产生单行的语句，可以通过executeQuery().next()方法来遍历然后输出
	 	execute()
		应该仅在语句能返回多个ResultSet对象、多个更新计数或ResultSet对象与更新计数的组合时使用
		executeUpdate()
		返回的int型，表示的是受影响的行数

### properties文件
	里面主要是用来存放：
	SQL注册驱动器：com.mysql.cj.jdbc.Driver
	地址：jdbc:mydwl://连接主机的IP：端口号/数据库名?Timezone=GMT%2B8
	账号：用户名	
	密码：对应的密码
	放到src文件下，文件后缀properties
	
### JDBC工具类
```java
	private JDBC_t_1() {}
		//注册驱动
		static {
			try {
				Class.forName("com.mysql.jdbc.Driver");
			} catch (Exception e) {
				e.printStackTrace();
			}
		}
		//新建连接
		public static Connection getconnection() {
			Connection con = null;
			String sql = "jdbc:mysql://localhost:3306/aaa";
			String sname = "root";
			String spass = "123";
			try {
				con = DriverManager.getConnection(sql, sname, spass);
			} catch (SQLException e) {
				e.printStackTrace();
			}
			return con;
		}
		//释放资源
		public static void close(Connection con ,Statement sta,ResultSet rs) {
			if(con!=null) {
				try{
					con.close();
				}catch(Exception ex){}			
			}
		
			if(sta!= null){
				try{
					sta.close();
				}catch(Exception ex){}
			}
			if(rs!= null) {
				try {
					rs.close();
				}catch(Exception ex) {}
			}
		}
		//释放资源
		public static void close(Connection con, Statement sta) {
			if(con !=null) {
				try {
					con.close();
				}catch(Exception ex) {}
			}
			
			if(sta !=null) {
				try {
					sta.close();
				}catch(Exception ex) {}
			}
		}
	}
```
### DBU（工具类）
	1.DbUtils：连接数据库对象----jdbc辅助方法的集合类，线程安全
			构造方法：DbUtils()
		 	作用：控制连接，控制书屋，控制驱动加载额一个类。
	2.QueryRunner：一个核心类,有4个核心方法
			query(Connection conn, String sql, Object[] params, ResultSetHandler rsh);
			执行选择查询，在查询中，对象阵列的值被用来作为查询的置换参数

			query(String sql, Object[] params, ResultSetHandler rsh);
			方法本身不提供数据库连接，执行选择查询，在查询中，对象阵列的值被用来作为查询的置换参数

			query(Connection conn, String sql, ResultSetHandler rsh);
			执行无需参数的选择查询

			update(Connection conn, String sql, Object[] params);
			被用来执行插入、更新或删除（DML）操作
### JavaBean（类）
	作用：开发的时候用来封装数据
	提供无参构造，私有字段，getter\setter方法

### 调用Query()方法，存储数据
	1.array_handler类中arrayhandler()
		// 通过调用Query(Connection con ,String sql,ArrayHandler al);
		// 结果集封装到一个数据对象中，只返回数据表中第一行的数据
	2.array_handler类中arraylisthandler();
		// 通过调用Query(Connection con ,String sql,ArrayListHandler alh);
		// 结果集的每一行封装到一个数据对象中，对象数组存放到一个集合中，然后使用循环输出整个数据表
	3.bean_handler类中beanhandler()
		// 通过调用Query(Connection con ,String sql ,BeanHandler<T>(class);
		// 结果集的的第一条数据，封装到一个指定JavaBean对象中
	4.bean_handler类中beanhandler();
		// 通过调用Query(Connection con ,String sql ,BeanListHandler<T>(class));
		// 结果集中的每一条数据都封装到指定JavaBean对象中，然后把这些JavaBean对象封装到List中
	5.column_handler()类中columnlisthandler()
		//query(Connection con , String sql , new ColumnListHandler<T>("要封装列的列名"));
		//结果集，为指定列的数据，存储到List集合中,一次只能封装一列
	6.scalar_handler类中scalarhandler();
		// 通过调用Query(Connection con , String sql , new ScalarHandler<T>());
		// 对于查询以后只有一个结果的时候试用，只有一行或则一列
	7.map_handler类中MapHandler();
		// 通过调用Query(Connection con , String sql , new MapHandler());
		// 把结果集的第一行存储到Map<键，值>中，键：列名；值：
	8.map_handler类中MapListHandler();
		/*  List<Map<String,Object>> list = Query(con,sql,new MapListHandler());
		 *  for(Map<String , Object> m :list){
		 *  	for(Object key : m.keySet()){
		 *  		System.out.print(key+" "+ m.get(key));
		 *  	}
		 *  }
		 */
		// 把结果集的每一行都存储到Map<键， 值>中，键：列名；值：数据
		// 再把Map存放到List集合中

### 连接池
	DBCP连接池
		类似于一个工具类，在这个类中设置好静态值，必须设置数据驱动名称、数据库地址、用户名、密码
		选择设置初始化连接个数、最大连接数、最大空闲连接数、最小空心啊连接数
```java	
static {
	// 设置基本4项
	// 数据库驱动名称、数据库地址、用户名、密码
	datasource.setDriverClassName("com.mysql.jdbc.Driver");
	datasource.setUrl("jdbc:mysql://localhost:3306/aaa");
	datasource.setUsername("root");
	datasource.setPassword("123");
	// 配置其他
	// 初始化连接个数为10个
	datasource.setInitialSize(10);
	// 最大连接数8个
	datasource.setMaxActive(8);
	// 最大空闲连接为数5
	datasource.setMaxIdle(5);
	// 最小空闲连接为数2
	datasource.setMinIdle(2);
}
```
### c3p0连接池
	user
		用户名
	password
		密码
	driverClass
		sql驱动
	jdbcUrl
		sql路径
	acquireInecrement
		 连接池无空闲连接的时候，一次新建连接的个数 
	initalPoolSize
		初始化连接的个数
	maxPoolSize
		最大连接个数
	minPoolSize
		最小连接个数
	maxldleTime
		最大空闲连接
	maxConnectionAge
		最大生存时间，若超过连接池断开，反之close断开
	maxldleTimeExcessConntions 
		配置为0时候，会将连接池中的连接个数保持在minPoolSize以内
	maxStatements
		连接池为数据源缓存的PreparedStatement数
	maxStatementsPerConnion
		连接池为数据源单个Conntion缓存的PreparedStatement数

### DBUTils
	BeanHandler
		把结果集的第一条封装到一个javaBean中
	BeanListHandler
		把结果中的数据存放到一个list集合中去
	scalarHandle
## 数据库的配置文件
### database.properties文件
```properties	
	class=com.mysql.cj.jdbc.Driver
	url=jdbc:mysql://localhost:3306/aaa?serverTimezone=GMT%2B8
		$serverTimezone = GMT%2B8  调整时区
	user=root
	password=123
```
### c3p0
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<c3p0-config>
		<default-config>
			<property name="driverClass">com.mysql.jdbc.Driver</property>
			<property name="jdbcUrl">jdbc:mysql://localhost:3306/aaa</property>
			<property name="user">root</property>
			<property name="password">123</property>
		</default-config> 
		<default-config name="aaa">
			<property name="driverClass">com.mysql.jdbc.Driver</property>
			<property name="jdbcUrl">jdbc:mysql://localhost:3306/aaa</property>
			<property name="user">root</property>
			<property name="password">123</property>
		</default-config> 
	</c3p0-config> 
	<!-- 最小连接数 -->
		minPoolSize(3)
	<!-- 初始化连接数 -->
		initialPoolSize(3)
	<!-- 最大连接数 -->
		maxPoolSize(15)
	<!-- 最大等待时间 -->
		maxIdleTime(0秒)
```
## JDBC
### execute、executeQuery、executeUpdate的区别
	executeQuery(使用最多):sql语句是以 SELECT开头的，用这个方法
	返回的是ResultSet对象
	executeUpdate(使用一般)：sql语句是以 INSERT、UPDATE、DELETE、以及sql定义语句
	返回的是收到影响的行数		
	execute(使用较少)：执行后返回的是多个结果集、多个更新计数或则二者组合的语句
### 例1
	prepareStatemenet:大多数时候是预编译过了,执行时，只需要DBMS运行SQL语句，就不用在编译了
	多次执行某一个语句使用这个比较好
```java
String sql1 = "UPDATE faccount SET ? = ? WHERE fid=?";
ps = con.prepareStatement(sql1);
ps.setString (1,flname)
ps.setDouble(2, 1.0);
ps.setInt(3, 1);
运行出的结果是
UPDATE faccount SET 'flname'=1.0 WEHRE fid = 1
```		