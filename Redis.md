### 安装

```shell
# 安装c环境
yum install gcc-c++
# 解压
tar -xvf redis.tar.gz -C 目标路径
# 编译
make
# 安装
make PREFIX=安装目录 install
# 拷贝配置文件到安装目录
redis.conf
# 修改配置文件
daemonize yes
```

### 启动关闭

```shell
# 前端模式启动
./redis-server
# 后端模式启动
./redis-server redis.conf
# 关闭
./redis-cli shutdown
# 默认端口号6379
```

### 常用命令

#### 存储字符串

```shell
get 键 # 取值
set 键 值 # 设置值
getset 键 值 # 取值并设置值
del 键 # 删除值

incr 键 # 自增（没有定义过的键默认值为0）
decr 键 # 自减
incrby 键 自增值 # 指定值自增
decrby 键 自减值 # 指定值自减

append 键 字符串 # 拼接字符串
```

#### 存储hash

```shell
hset 键 子键 值 # 设置值
hmset 键 子键1 值1 子键2 值2 # 批量赋值

hget 键 子键 值 # 取值
hmget 键 子键1 值1 子键2 值2 # 批量取值

hgetall 键 # 取出该键下所有子键和值

hdel 键 子键 # 删除子键 可以批量删除
del 键 # 删除整个hash

hlen 键 # 获取子键个数
hkeys 键 # 获取所有子键
hvals 键 # 获取所有子键的所有值
```

#### 存储list

```shell
lpush 键 值1 值2 # 在左边添加
rpush 键 值1 值2 # 在右边添加 
lrange 键 开始范围 结束范围 # 查询指定范围值(0 -1表示全部)

lpop 键 # 从左边弹出
rpop 键 # 从右边弹出

llen 键 # 获取个数

lrem 键 个数 值 # 删除多个元素 个数>0 从左开始 个数<0 从右开始 个数=0 删除所有
```

#### 存储Set

```shell
sadd 键 值1 值2 # 添加
srem 键 值 # 删除
smembers 键 # 查询

sdiff 键1 键2 # 取差集 键1 - 键2
sinter 键1 键2 # 取交集
sunion 键1 键2 # 取并集
```

#### 存储ZSet

```shell
zadd 键 分数1 值1 分数2 值2 # 添加,根据分数排序
zrang 键 开始范围 结束范围 [withsocres] # 查询指定范围值(0 -1表示全部) 降序
zrevrang 键 开始范围 结束范围 [withsocres] # 查询指定范围值(0 -1表示全部) 升序
```

#### 通用keys操作

```shell
keys * # 查询所有key
keys *名 # 以名结尾
keys 名* # 以名开头
rename 旧键名 新键名 # 重命名
type 键名 # 显示该键类型

expire 键名 秒数 # 设置过期时间
persist 键名 # 清除过期时间
ttl 键名 # 查询过期时间 -1 未设置过期时间 -2 不存在键
```

#### 数据库操作

```shell
select 数据库编号 # 切换数据库
dbsize # 当前数据库key的数量
flushall # 清空所有数据库的所有key
```

### Redis集群

#### 搭建集群

至少有三个节点,每个节点有一个备份机,至少需要6台服务器.

1. 修改每个Redis配置文件redis.conf中` cluster-enabled yes `
2. 启动每个Redis服务` redis-server `
3. 安装ruby` yum install ruby `
4. 安装ruby包管理器` yum install rubygems `
5. 安装redis库` gem install redis-3.0.0 `
6. 运行ruby脚本(在任意一台服务器上执行)

```shell
# create 创建集群
# replicas 1 每个节点1个备份机
./redis-trib.rb create --replicas 1 节点列表
```

#### 连接集群

` redis-cli -p 端口号 -c `

### Jedis使用

```java
// 创建jedis对象
Jedis jedis = new Jedis(地址, 端口号);
// 设置值
jedis.set(键,值);
// 取值
String str = jedis.get(键);
// 关闭连接
jedis.close();
```

#### Jedis连接池

```java
// 创建连接池对象
JedisPool jedisPool = new JedisPool(地址, 端口号);
// 从连接池中获得连接
Jedis resource = jedisPool.getResource();
// 关闭连接,连接池回收资源
resource.close();
// 关闭连接池
jedisPool.close();
```

#### JedisCluster连接集群

```java
// 创建连接节点集合
Set<HostAndPort> nodes = new HashSet<>();
// 添加多个连接
nodes.add(new HostAndPort(地址, 端口号);
// ...
// 创建JedisCluster对象操作redis
JedisCluster jedisCluster = new JedisCluster(nodes);
// 关闭JedisCluster对象
jedisCluster.close();
```

### 集成SpringBoot

依赖

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

配置

```yaml
spring:
  redis:
    host: localhost
    port: 6379
```

