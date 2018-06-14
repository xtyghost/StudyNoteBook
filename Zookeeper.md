### Zookeeper

一种分布式协调服务(可以在分布式系统中共享配置,协调锁资源,提供命名服务)

#### 数据结构

基于Znode节点的树,使用路径引用,每个节点数据不能超过1MB

+ Znode
  + data : Znode存储的数据信息
  + ACL : 记录Znode的访问权限,哪些人或哪些ip可访问本节点
  + stat : Znode的元数据(事物ID, 版本号, 时间戳, 大小 ...)
  + child : 当前节点的子节点引用
    + Znode
    + Znode

```shell
# 启动
./zkServer.sh start
# 查看状态
./zkServer.sh status
# 停止
./zkServer.sh stop
# 客户端
./zkCli.sh
```

#### zookeeper常用命令

```shell
ls
ls2
get
stat
# 创建节点
create 节点名 节点数据
-e # 创建临时节点
-s # 创建顺序节点
# 修改节点
set 节点名 节点数据 [节点版本]
# 删除节点
delete 节点名 [节点版本]
```

