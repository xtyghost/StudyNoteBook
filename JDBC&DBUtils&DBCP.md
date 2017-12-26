## JDBC

```java
// 1.注册驱动
Class.forName("com.mysql.jdbc.Driver");

// 2.获得数据库连接
Connection con = DriverManager.getConnection(url,user,password);
//  url = "jdbc:mysql://主机ip:端口号/数据库名字"

// 3.通过数据库连接对象，获取SQL语句的执行者对象
Statement stat = con.createStatement();

// 4.执行SQL语句
// 查询数据
ResultSet rs = stat.executeQuery(SQL语句);
// 更新数据
int num = stat.executeUpdate(SQL语句);

// 5.预编译SQL语句
String sql = "...?..."; // ? 参数
PreparedStatement pst = con.prepareStatement(sql);
pst.setString(1,参数1);
...
```

### JDBC事务

```java
// 默认自动事务
执行一次SQL语句事物自动提交

// 手动控制事务
conn.setAutoCommit(false); //开启事务
conn.conn.commit(); // 提交事务
conn.rollback(); // 回滚事务

// 执行SQL的connection与事物处理的connection必须是同一个才能对事务控制
```



## DBUtils

### QueryRunner 核心类

#### 常用方法

```java
// update 对表数据增加、删除、更新操作
update(Connection con,String sql,Object... params)
  
// query 对表数据进行查询
query(Connection con,String sql,ResultSetHandler<T> rsh,Object... params)
```

#### ResultSetHandler 结果集

```java
// 将结果集 第一条 封装到 Object[] 中
ArrayHandler

// 将结果集 每一条 封装到 Object[] 中，所有数组封装到 List 中
ArrayListHandler

// 将结果集 第一条 封装到 javaBean 中
BeanHandler

// 将结果集 每一条 封装到 javaBean 中，所有数组封装到 List 中
BeanListHandler

// 将结果集 指定列 封装到 List 中
ColumnListHandler

// 接收返回的单数据
ScalarHandler

// 将结果集 第一行 封装到 Map 中 key列名 value数据
MapHandler

// 将结果集 每一行 封装到 Map 中 key列名 value数据 所有Map封装到 List 中
MapListHandler

/* 
 * javaBean 用来封装数据的java类
 * 提供 私有字段
 * 提供 get/set方法
 * 提供 空参构造函数
 */
```

## DBCP

```java
// 1.创建BasicDataSourec对象
BasicDataSource dataSource = new BasicDataSource();

// 2.配置DataSource
  // 必须项
  // 数据库驱动
  setDriverClassName("com.mysql.jdbc.Driver");
  // 数据库地址
  setUrl("jdbc:mysql://localhost:3306/homework");
  // 用户名
  setUsername("root");
  // 密码
  setPassword("1306adf");

  // 非必须项
  // 初始化连接数量
  setInitialSize(10);
  // 最大连接数量
  setMaxActive(8);
  // 最大空闲连接
  setMaxIdle(5);
  // 最小空闲连接
  setMinIdle(1);
```

