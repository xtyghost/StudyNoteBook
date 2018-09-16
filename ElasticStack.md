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
# 查看集群健康状态
GET _cluster/health
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

# 自定义分词
PUT /索引
{"settings":{
    "analysis":{
        "char_filter":{},
        "tokenizer":{},
        "filter":{},
        "analyzer":{}
    }
}}
```

### Mapping

```shell
# 查看mapping
GET /索引/_mapping

# 自定义mapping
PUT /索引
{"mappings":{
    "类型":{
        "properties":{
            "字段":{
                "type":类型,
                "null_value":默认值
            }
        }
    }
}}

# 自动识别
PUT /索引
{"mappings":{
    "类型":{
    	# 自定义日期格式
        "dynamic_date_formats":["日期格式"],
        # 关闭日期自动识别
        "date_detection":false,
        # 开启数字自动识别
        "numeric_detection":true
    }
}}

# Dynamic Templates
PUT /索引
{"mappings":{
    "类型":{
    	"dynamic_templates":[{
            "模板名":{
            	# 根据字段类型匹配
                "match_mapping_type":类型,
                # 根据字段名匹配
                "match":字段名,
                # 根据字段名匹配不包含
                "unmatch":字段名,
                "mapping":{
                    "type":类型
                }
            }
    	}]
    }
}}
```

### 索引模板

```shell
PUT _template/模板名
{
	# 匹配索引
    "index_patterns":["索引名"],
    # 顺序
    "order":顺序,
    "settings":{
        
    }
}
```

### Search API

#### URI Search

```shell
GET /索引/_search?q=查询语句&df=默认查询字段&sort=排序字段&timeout=超时时间&from=页数&size=每页数量

# 查询语法
AND(&&) OR(||) NOT(!)
+(%2B) 一定包含 - 一定不包含
[a TO b] a到b包含ab
[a TO b} a到b不包含b
[a TO] 大于等于a
[* TO b] 小于等于b
```

#### Request Body Search

```shell
# Match Query
GET /索引/_search
{"query":{
    "match":{"字段名":查询语句}
}}
{"query":{
    "match":{
    	"字段名":{
    		"query":查询语句,
    		"operator":or/and,
    		# 至少包含字段数
    		"minimum_should_match":数值
    }
}}

# Term Query (不分词)
GET /索引/_search
{"query":{
    "term":{"字段":条件}
}}
```

### Query DSL复合查询

```shell
# constant_score query
GET /索引/_search
{"query":{
    "constant_score":{
        "filter":{
            "match":{"字段名":查询语句}
        }
    }
}}
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