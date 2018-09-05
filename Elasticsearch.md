#### 开放访问

```shell
# 默认只允许本机访问, 修改config/elasticsearch.yml
network.host: 允许访问IP # (0.0.0.0 所有)
```

#### 节点与集群

+ node : 单个实例称为节点
+ cluster : 一组节点构成集群

### 索引与文档

数据管理的顶层单位叫Index,每个 Index的名字必须是小写.

Index里面单条的记录称为Document.许多条Document构成了一个Index.Document使用JSON格式表示.

#### 新建和删除index

```shell
# 新建index
PUT /索引名
# 删除index
DELETE /索引名
# 新增数据
PUT /索引名/数据ID JSON数据
POST /索引名 JSON数据
# 查看数据
GET /索引名/数据ID?pretty=true
# 删除数据
DELETE /索引名/数据ID
# 更新数据
PUT /索引名/数据ID JSON数据
```

