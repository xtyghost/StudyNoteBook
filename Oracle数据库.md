### 常用SQL


```sql
-- 显示当前用户
show user
-- 显示所有表
select * from tab;
-- 显示表结构
desc 表名
-- 显示设置行宽
show linesize
set linesize
-- 设置列宽
col 列名 for a字符个数 -- 设置字符串列宽
col 列名 for 9999 -- 设置数字列宽,每个9代表一位
-- 执行上条语句
/
-- 设置别名 别名中有空格,数字,关键字时必须加双引号
列名 as "别名"
列名 "别名"
列名 别名

-- 去重
distinct -- 作用于后面所有列

-- select语句必须有from,当与表没有关联时使用dual(伪表);

-- 排序
order by 列名|表达式|别名|序号
-- order by作用与后面所有列
-- desc只作用于离他最近的列

-- where和having区别: where不能使用多行函数

-- 分页查询
select
	*
from
	(select rownum num,表别名.* from 表 表别名 where rownum <= 尾行)
where
	num >= 起始行;
	
-- 基于排序的分页查询
select
	*
from
	(select
    	rownum num,
    	表别名.*
    from
    	(select * from 表 order by 排序列) 表别名
    where
    	rownum <= 尾行)
where
	num >= 起始行;
```

### SQL优化原则

1. 尽量使用列名
2. where解析顺序是从右往左
3. where和having都能使用时,尽量使用where

### SQL中的null

1. 包含null的表达式都为null
2. null != null
3. 如果集合中含有null,不能使用not in,可以使用in
4. null值最大
5. 多行函数会自动滤空,可以嵌套滤空函数屏蔽

### 函数

+ 单行函数
  + 字符函数
    + length(字符串) : 求字符串长度
    + substr(字符串,起始字符,个数) : 取子串
    + conca(字符串1,字符串2) : 拼接字符串(相当于 字符串1||字符串2)
  + 数值函数
    + round(数字,保留小数位数) : 四舍五入
    + trunc(数字,保留小数位数) : 数字截取
    + mod(数字1,数字2) : 取模
  + 日期函数
    + add_months(日期,月数) : 加月
    + last_day(日期) : 求所在月最后一天
  + 转换函数
    + to_char(数字) : 数字转字符串
    + to_number(字符串) : 字符串转数字
    + to_char(日期,'日期格式') : 日期转字符串
    + to_date(字符串,'日期格式') : 字符串转日期
  + 通用函数
    + nvl(检测的值,为null时显示的值) : 空值处理
    + nvl2(检测的值,不为null时显示的值,为null时显示的值) : 空值处理
    + decode(条件值,值1,翻译值1,值2,翻译值2,…,缺省值) : 条件取值
+ 多行函数

### 数据类型

1. 字符型
   1. CHAR : 固定长度字符类型,最大2000字节
   2. VARCHAR2 : 可变长度字符类型,最大4000字节
   3. LONG : 长文本类型,最大2G
2. 数值型
   1. NUMBER(5) : 总共5位,最大99999
   2. NUMBER(5,2) : 总共5位,整数3位,小数2位,最大999.99
3. 日期型
   1. DATE : 精确到秒
   2. TIMESTAMP : 精确到秒的小数点后9位
4. 二进制型
   1. CLOB : 存储字符,最大4G
   2. BLOB : 存储图像,声音,视频等二进制数据,最大4G



### 集合运算

+ 并集 union (去掉重复记录)
+ 并集 union all (包含重复记录)
+ 交集 intersect
+ 差集 minus

### 伪列

+ rowid : 物理地址
+ rownum : 结果集序号

### 复制表

```sql
-- 复制表结构及数据
create table 新表名 as select * from 旧表;
-- 复制表结构
create table 新表名 as select * from 旧表 where 1=2;
-- 复制表数据
insert into 表名 (列1,列2,...) select (列1,列2,...) from 旧表;
```

### 视图

虚表,不占用空间,修改视图内容,基表内容也会改变;

```sql
/* 创建视图 */
create [or replace] [force] view 视图名 as 查询语句 [with check option] [with read only];
-- or replace : 若视图已存在,重建该视图
-- force : 不管基表是否存在都创建视图
-- with check option : 
-- with read only : 创建只读视图

/* 删除视图 */
drop view 视图名;
```

### 物化视图

结果集的副本,占用空间

```sql
/* 创建物化视图 */
create meterialized view 物化视图名
[build immediate | build deferred]
refresh [fast | complete | force]
[on [commit | demand] | start with (start time) next (next time)] as 查询语句;

-- build immediate : 创建物化视图时就生成数据(默认值)
-- build deferred : 创建物化视图时不生成数据

-- refresh fast : 增量刷新,只刷新上次修改
-- refresh complete : 刷新整个物化视图
-- refresh force : 自动选择,不可以fast时自动complete

-- on commit : 基表提交时刷新
-- on demand : 手动刷新(默认值)

-- 创建增量物化视图前必须创建物化视图日志,物化视图中必须有基表的rowid
create meterialized view log on 基表名 with rowid
```

### 序列

```sql
-- 创建简单序列
create sequence 序列名;
-- 查询序列的下一个值
select 序列名.nextval from dual;
-- 查询序列当前值
select 序列名.currval from dual;

-- 创建复杂序列
create sequence 序列名
[increment by n] -- 递增值,默认值1
[start with n] -- 起始值,递增默认值minvalue,递减默认值maxvalue
[maxvalue n] -- 最大值
[minvalue n] -- 最小值
[cycle|nocycle] -- 循环|不循环 默认不循环
[cache n|nocache] -- 分配并存入到内存中,默认cache 20
```

### 索引

```sql
-- 创建普通索引
create index 索引名 on 表名(列名);
-- 创建唯一索引
create unique index 索引名 on 表名(列名);
-- 创建复合索引
create index 索引名 on 表名(列名1,列名2);
-- 创建反向索引
create index 索引名 on 表名(列名) reverse;
```

### PL/SQL

```sql
declare
	-- 声明变量
	变量名 类型(长度);
	-- 引用列类型
	变量名 表名.列名%type%;
	-- 行记录类型
	变量名 表名%rowtype%;
begin
	-- 变量赋值
	变量名:=变量值;
	select 列名1,列名2... into 变量名1,变量名2... from 表名 where 条件;
	-- 行记录类型变量赋值
	select * into 变量名 from 表名 where 条件;
	
	-- 输出
	dbms_output.put_line(要输出的值);
exeception
	-- 异常(例外)
	when 异常类型 then 异常处理
end;
```

```sql
-- 条件判断
if 条件 then 执行语句 end if;
if 条件 then 执行语句 else 执行语句 end if;
if 条件 then 执行语句 elsif 条件 then 执行语句... else 执行语句 end if;
```

```sql
-- 循环

-- 无条件循环
loop
	循环语句
	exit when 结束条件;
end loop;

-- 有条件循环
while 循环条件
loop
	循环语句
end loop;

-- for循环
for 变量 in 起始 .. 结束
loop
	循环语句
end loop;
```

### 游标

```sql
declare
	-- 声明游标
	游标名 is 查询语句;
	-- 行记录类型变量
	变量名 表名%rowtype%;
begin
	-- 打开游标
	open 游标名;
		loop
			-- 提取游标
			fetch 游标名 into 变量名;
			-- 退出循环
			exit when 游标名%notfound;
			-- 输出值
			dbms_output.put_line(变量名.列名);
		end loop;
	-- 关闭游标
	close 游标名;
end;
```

