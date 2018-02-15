### SpringBoot启动方式

### 属性配置

application.properties

```properties
# 配置端口
server.port=8080
# 配置工程路径
server.context-path=路径
```

application.yml

```
server:
  port: 8080
  context-path: 路径
```

#### 切换多个配置

application-配置文件1.yml

application-配置文件2.yml

application.yml

```
spring:
	profiles:
		active: 配置文件1
```

#### 启动时切换配置文件

` java -jar 文件.jar --spring.profiles.active=配置文件1 `

### 获取配置文件属性

```java
@Value("${属性名}")
private 属性类型 属性名; 
```

```
# 配置文件中获取属性
属性名: "${属性名}"
```

```java
@Component
@ConfigurationProperties(prefix = "属性名")
public class 属性名 {
    private 属性类型 属性名;
  	get/set方法
}
```

### 注解

```java
@RestController
// 相当于
@Controller
@RequestBody

// 类上配置url
@RequestMapping("url")
//方法上配置多个url
@RequestMapping(value = {"url1","url2"})

@GetMapping(value = "url")
// 相当于
@RequestMapping(value = "url", method = RequestMethod.GET)
```

```java
// 获取请求参数

// /url/参数值
@RequestMapping("/url/{参数名}")
public String 方法(@PathVariable 参数类型 参数名)

// /url?参数名=参数值
@RequestMapping("/url")
public String 方法(@RequestParam(参数名) 参数类型 变量名)
  
// RequestParam参数
value 参数名
required 是否必传(默认true)
defaultValue 参数默认值
```

