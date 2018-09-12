# ElasticStack

+ **Kibana** 数据探索与可视化分析
+ **Elasticsearch** 数据存储、查询与分析
+ **Beats** 数据收集与处理
+ **Logstash** 数据收集与处理

## Elasticsearch

### 集群

```shell
# 查看集群节点
GET _cat/nodes
GET _cat/nodes?v
# 查看集群详情
GET _cluster/stats
```

### Query

```shell
# QueryString
GET index/type/_search?q=value
# QueryDSL
GET index/type/_serarch
{
    "query": {
        "term": {
            filed: {
                "value": value
            }
        }
    }
}
```

### 索引API

```shell
# 创建索引
PUT /索引
# 删除索引
DELETE /索引
# 查看索引
GET _cat/indices
```

### 文档API

```shell
# 创建文档
PUT /索引/类型/文档ID
{"字段":值}
# 不指定ID创建文档
POST /索引/类型
{"字段":值}

# 根据ID查询文档
GET /索引/类型/文档ID
# 查询所有文档
GET /索引/类型/_search

# 批量操作文档
POST /_bulk
# 指令 index update create(文档已存在时报错) delete
{指令:{"_index":索引,"_type":类型,"_id":文档ID}}
{"字段":值}

# 批量查询文档
GET /_mget
{"docs":[{"_index":索引,"_type":类型,"_id":文档ID}]}
```

### 分词API

```shell
# 指定分词器分词
POST /_analyze
{"analyzer":分词器,{"text":内容}}
# 指定字段分词
POST /索引/_analyze
{"field":字段,{"text":内容}}
```



## Kibana

### 配置

```shell
elasticsearch.url: elasticsearch地址
```

### 常用功能

+ Discover 数据搜索查看
+ Visualize 图表制作
+ Dashboard 仪表盘制作
+ Timelion 时序数据的高级可视化分析
+ DevTools 开发者工具
+ Management 配置

## Beats

+ Filebeat 日志文件
+ Metricbeat 度量数据
+ Packebeat 网络数据
+ Winlogbeat Windows数据
+ Heartbeat 健康检查