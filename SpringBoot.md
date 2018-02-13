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

