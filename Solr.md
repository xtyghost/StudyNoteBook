### Solr

基于Lucene的全文搜索服务器,它是一个搜索引擎服务,可以独立运行

#### Solr目录结构

+ **bin** : solr的运行脚本
+ **contrib** : solr的插件
+ **dist** : war和jar文件
+ **example**
  + **lib**
    + **ext** : 日志相关jar包

#### Solr安装

1. 复制solr/dist下war文件到tomcat/webapps中
2. 复制example/lib/ext下jar包到solr工程下WEB-INF/lib中
3. 复制出example/solr目录作为solrhome目录
4. 修改solr工程web.xml文件

```xml
<env-entry>
    <env-entry-name>solr/home</env-entry-name>
	<env-entry-value>solrhome目录路径</env-entry-value>
	<env-entry-type>java.lang.String</env-entry-type>
</env-entry>
```

#### 配置中文分析器

1. IKAnalyzer2012FF_u1.jar添加到solr工程下WEB-INF/lib中
2. 修改solrhome/collection1/conf/schema.xml配置文件

```xml
<fieldType name="text_ik" class="solr.TextField">
	<analyzer class="org.wltea.analyzer.lucene.IKAnalyzer"/>
</fieldType>
```

#### SolrJ

##### 添加

```java
// 创建SolrServer对象
SolrServer solrServer = new HttpSolrServer(solr地址);
// 创建文档对象SolrInputDocument
SolrInputDocument document = new SolrInputDocument();
// 向文档中添加域
document.addField(域名,值);
// 删除文档
solrServer.deleteByQuery("id:id值");
// 把文档写入索引库
solrServer.add(document);
// 提交
solrServer.commit();
```

##### 查询

```java
// 创建SolrServer对象
SolrServer solrServer = new HttpSolrServer(solr地址);
// 创建SolrQuery对象
SolrQuery solrQuery = new SolrQuery();

// 设置查询条件
solrQuery.setQuery(查询条件);
// 查询所有
solrQuery.setQuery("*:*");
solrQuery.set("q","*:*");

// 分页查询
solrQuery.setStart(0);
solrQuery.setRows(20);

// 设置默认搜索域
solrQuery.set("df",域);

// 设置查询高亮
// 开启高亮
solrQuery.setHighlight(true);
// 添加高亮域
solrQuery.addHighlightField(域);
// 设置高亮前后缀
solrQuery.setHighlightSimplePre("<em>");
solrQuery.setHighlightSimplePost("</em>");

// 执行查询
QueryResponse queryResponse = solrServer.query(solrQuery);
SolrDocumentList solrDocumentList = queryResponse.getResults();

// 获取总记录数
solrDocumentList.getNumFound();

// 取高亮显示
Map<String, Map<String, List<String>>> highlighting = queryResponse.getHighlighting();
List<String> list = highlighting.get(solrDocument.get("id")).get(域);
高亮 = list.get(0);
```

