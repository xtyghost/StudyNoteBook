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