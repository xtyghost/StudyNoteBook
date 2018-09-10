### ElasticSearch

基于ApacheLucene的开源搜索引擎,采用Java编写,提供RESTFulAPI

#### 开放访问

```shell
# 默认只允许本机访问, 修改config/elasticsearch.yml
network.host: 允许访问IP # (0.0.0.0 所有)
```

#### 节点与集群

+ node : 单个实例称为节点
+ cluster : 一组节点构成集群

##### 集群配置

```yaml
cluster.name: 集群名
node.name: 节点名
node.master: true # 指定为主节点
network.host: 地址
http.port: 端口
discovery.zen.ping.unicast.hosts: ["主节点地址"] # 从节点配置
```

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
# 删除数据
DELETE /索引名/数据ID
# 更新数据
PUT /索引名/数据ID JSON数据
# 查询数据
GET /索引名/数据ID
```

### 查询数据

#### 条件查询

```json
// POST /索引名/_search
{
	"query": {
        // 查询所有
		"match_all": {}
        
	},
    // 查询位置
	"from": 1,
    // 查询条数
	"size": 1
    // 排序
    "sort": [
		{"排序字段": {"order": "asc/desc"}}
	]
}
```

#### 聚合查询

```json
{
	"aggs": {
		"聚合查询名": {
			"terms": {
				"field": "聚合字段"
			}
		}
	}
}
```

#### query

```json
{
	"query": {
		"match": {
			"字段": "条件"
		},
        // 习语匹配
        "match_phrase": {
			"字段": "条件"
		},
        // 多字段匹配
        "multi_match": {
			"query": "条件",
			"fields": ["字段1","字段2",...]
		},
        // 语法查询
        "query_string": {
			"query": "(条件 AND 条件) OR 条件",
            "fields": ["字段1","字段2",...]
		},
        // 范围查询
        "range": {
			"字段": {
                "gt": 大于,
				"lt": 小于,
				"gte": 大于等于,
				"lte": 小于等于
			}
		}
	}
}
```

#### filter

```json
{
	"query": {
		"bool": {
			"filter": {
				"term": {
					"字段": 条件
				}
			}
		}
	}
}
```

#### 复合查询

```json
{
	"query": {
		"bool": {
            // AND
			"must": [
				{
					"match": {
						"字段": 条件
					}
				},
				{
					"match": {
						"字段": 条件
					}
				}
			],
            // OR
			"should": [
				{
					"match": {
						"字段": 条件
					}
				},
				{
					"match": {
						"字段": 条件
					}
				}
			],
            // NOT
            "must_not": [
				{
					"match": {
						"字段": 条件
					}
				}
			]
		}
	}
}
```

