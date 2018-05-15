### FreeMarker

#### 使用步骤

```java
// 创建Configuration对象
Configuration configuration = new Configuration(Configuration.getVersion());
// 设置模板文件所在路径
configuration.setDirectoryForTemplateLoading(new File(模板文件所在路径));
// 设置模板文件使用的字符集
configuration.setDefaultEncoding("utf-8");
// 加载模板，创建模板对象
Template template = configuration.getTemplate(模板名);
// 创建数据集，可以是pojo也可以是map
Map data = new HashMap();
// 添加数据
// 创建Writer对象，指定生成的文件名
Writer out = new FileWriter(new File(生成的文件名));
// 调用模板process方法输出文件
template.process(data,out);
// 关闭流
out.close();
```

### SpringBoot中使用

引入依赖

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-freemarker</artifactId>
</dependency>
```

controller中返回ModelAndView



### FreeMarker语法

```html
${属性名}
${对象名.属性名}

<#list 集合名 as 属性名>
  	<!-- 获取索引 -->
	${属性名_index}
</#list>

<#if 表达式></#if>

${Date对象?date}
${Date对象?time}
${Date对象?datetime}
${Date对象?string(日期格式)}

${可能为null的值!默认值}

<#if 可能为null的值??>
    不为null输出
    <#else>
    为null输出
</#if>

<!-- 引入其他模板 -->
<#include 文件名> 
```

