### 显示信息

```mysql
/*显示当前服务器版本*/
select version();

/*显示当前日期时间*/
select now();

/*显示当前用户*/
select user();
```



### 修改MySQL提示符

```mysql

-- 连接时指定
mysql -u用户名 -p密码 --prompt 提示符
-- 连接后指定
mysql>prompt 提示符
/*提示符*/
\D 完整的日期
\d 当前数据库
\h 服务器名称
\u 当前用户
```



### 数据库操作

```mysql
/*创建数据库*/
create database 数据库名;

/*查询数据库*/
show databases;

/*删除数据库*/
drop database 数据库名;

/*使用数据库*/
use 数据库名;

/* 查看当前所在数据库*/
select database();
```

### 表操作

```mysql
/*创建表*/
create table 表名(
	列名1 数据类型 约束,
  	列名2 数据类型 约束,
  	列名3 数据类型 约束
);

/*删除表*/
drop table 表名;

/*约束*/
primary key -- 主键约束
auto_increment -- 自动增长
not null -- 非空约束
unique key -- 唯一约束
default 值 -- 默认值
foreign key -- 外键约束

/*查看表*/
show tables;
/*查看指定数据库表*/
show tables from 数据库名;

/*查看表结构*/
show columns from 表名;
desc 表名;

/*修改表名*/
rename table 旧表名 to 新表名;
```

### 列操作

```mysql
/*添加列*/
alter table 表名 add 列名 数据类型 约束;

/*修改列*/
alter table 表名 modify 列名 数据类型 约束;

/*修改列名*/
alter table 表名 change 旧列名 新列名 数据类型 约束;

/*删除列*/
alter table 表名 drop 列名;
```

### 数据操作

```mysql
/*添加数据*/
insert into 表名 (列名1,列名2,列名3) values (值1,值2,值3);	-- 可以省略主键
insert into 表名 values (值1,值2,值3); -- 不可以省略主键,可以为null,default

/*批量添加数据*/
insert into 表名 values (列名1,列名2) values (值1,值2),(值1,值2);

/*修改数据*/
update 表名 set 列名1 = 值1,列名2 = 值2 where 条件;

/*删除数据*/
delete from 表名 where 条件;

/*重建表,auto_increment置为0*/
truncate table 表名;

/*条件写法*/
= -- 等于
<> -- 不等于
<= -- 小于等于
and -- 与
or -- 或
not -- 非

列名 in (值1,值2); -- 包含
列名 not in (值1,值2); -- 不包含
```

### 查询数据

```mysql
/*查询指定列*/
select 列名1,列名2 from 表名;

/*查询所有列*/
select * from 表名;

/*查询指定列去掉重复纪录*/
select distinct 列名1 from 表名;

/*查询指定列并临时命名*/
select 列名1 as 临时新列名 from 表名;

/*查询指定列并计算*/
select 列名1数学运算 from 表名;

/*查询指定列计算并临时命名*/
select 列名1数学运算 as 临时新列名 from 表名;

/*查询指定列并指定条件*/
select 列名1,列名2 from 表名 where 条件;

/*条件*/
列名 between 值1 and 值2 -- 范围在值1和值2之间（值1小于值2）

/*模糊查询*/
% -- 通配多个字符
_ -- 通配单个字符
列名 like '%值'; -- 以'值'结尾
列名 like '值%'; -- 以'值'开头
列名 like '%值%'; -- 包含'值'
列名 like '___'; -- 只有3个字符

/*查询非空*/
列名 is not null;
not (列名 is null);

/*查询并对结果排序*/
order by 列名 可选值 -- 默认升序 可选值: desc降序 asc升序

/*既有过滤条件又有排序时要后排序*/
where ... order by ...;

/*聚合函数*/
count(列名) -- 求个数 不包含null
sum(列名) -- 求和 列中非数值视为0
max(列名) -- 求最大值
min(列名) -- 求最小值
avg(列名) -- 求平均值

/*字符函数*/
concat(列名1,列名2,...) -- 字符连接
concat_ws(分隔符,列名1,列名2...) -- 指定分隔符连接字符

/*其他函数*/
round(avg(列名),小数位数) -- 对平均值四舍五入

/*分组查询*/
group by 列名;

/*分组查询后筛选*/
group by 列名 having 条件;

/*限制查询数量*/
limit 数量;
limit 起始,结束;
```

