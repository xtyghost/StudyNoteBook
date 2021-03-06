# ElasticStack

+ **Kibana** 数据探索与可视化分析
+ **Elasticsearch** 数据存储、查询与分析
+ **Beats** 数据收集与处理
+ **Logstash** 数据收集与处理

## Elasticsearch

#### 配置

```yaml
# config/elasticsearch.yml
cluster.name: 集群名
node.name: 当前节点名
node.master: true # 指定为主节点
network.host: 127.0.0.1 # 监听地址
http.port: 9200

# 启动时指定参数
-Ehttp.port=9200 -Epath.data=数据路径
```

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

### Elasticsearch Query

```shell
# QueryString
GET /索引/类型/_search
# 请求参数
q 查询条件:查询字段
df 查询字段
sort 排序字段:asc
from 页数
size 每页条数

# QueryDSL
GET /索引/类型/_search
{
    "query": {
        "term": {
            字段: {
                "value": 查询条件
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

POST /索引/类型/文档ID(不指定默认生成)
{"字段":值}

# 根据ID查询文档
GET /索引/类型/文档ID

# 删除文档
DELETE /索引/类型/文档ID

# 批量操作文档
POST /_bulk
# 指令 index update create(文档已存在时报错) delete
{指令:{"_index":索引,"_type":类型,"_id":文档ID}}
{"字段":值}

# 批量指定ID查询文档
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
                # 不建立索引
                "index": false,
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

### 排序

```shell
GET /索引/_search
{
    "query"{},
    "sort":{
        "字段":desc
    }
}
```

### 分页

```shell
GET /索引/_search
{
    "from":开始位置,
    "size":数量
}
```

### Scroll

```shell
# 获取scroll_id
GET /索引/_search?scroll=快照有效期
{"size":数量}

POST _search/scroll
{
    "scroll":快照有效期,
    "scroll_id":scroll_id
}

# 删除scroll快照
DELETE _search/scroll
{"scroll_id":scroll_id}
DELETE _search/scroll/_all
```

### Search_After

```shell
# 指定sort,并保证排序值唯一
GET /索引/_search
{
    "sort":{
        "字段":desc
    }
}
# Search_After
GET /索引/_search
{
    "search_After":上次返回的sort值
}
```

### 聚合分析

```shell
# Metric聚合分析
GET /索引/_search
{
	# 不返回文档列表
	"size":0,
    "aggs":{
    	"聚合分析名":{
        	"min/max/avg/sum/cardinality/stats":{"field":字段}
        }
    }
}
```

### SpringBoot集成

依赖

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>
```

配置

```yaml
spring:
  data:
    elasticsearch:
      cluster-nodes: 127.0.0.1:9300
```

## Kibana

### 配置

```yaml
# config/kibana.yml
server.port: 5601
elasticsearch.url:  "http://elasticsearch地址"
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

## Logstash

### Queue

