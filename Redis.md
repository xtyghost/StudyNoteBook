### 安装

```shell
# 安装c环境
yum install gcc-c++
# 解压
tar -xvf redis.tar.gz -C 目标路径
# 编译
make
# 安装
make PREFIX=文件目录 install
# 拷贝配置文件
redis.conf
```

### 启动关闭

```shell
# 前端模式启动
./redis-server
# 后端模式启动
./redis-server redis.conf
# 关闭
./redis-cli shutdown
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

#### 存取hash

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

lrem 键 个数 值 # 从左边删除多个元素
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

### Redis集群

至少有三个节点,每个节点有一个备份机,至少需要6台服务器.

1. 修改每个Redis配置文件redis.conf中` cluster-enabled yes `
2. 启动每个Redis服务` redis-server `
3. 安装ruby` yum install ruby `
4. 安装ruby包管理器` yum install rubygems `
5. 安装redis库` gem install redis-3.0.0 `
6. 运行ruby脚本

```shell
# create 创建集群
# replicas 1 每个节点1个备份机
./redis-trib.rb create --replicas 1 节点列表
```

