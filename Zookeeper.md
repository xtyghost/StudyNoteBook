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

