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

```yaml
server:
  port: 8080
  context-path: 路径
```

#### 切换多个配置

application-配置文件1.yml

application-配置文件2.yml

application.yml

```yaml
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

```yaml
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

#### 加载自定义yaml配置文件

```java
@Bean
public static PropertySourcesPlaceholderConfigurer properties() {
	PropertySourcesPlaceholderConfigurer configurer = new PropertySourcesPlaceholderConfigurer();
    YamlPropertiesFactoryBean yaml = new YamlPropertiesFactoryBean();
    // 使用文件路径
    yaml.setResources(new FileSystemResource("配置文件.yml"));
    // 使用classpath
	yaml.setResources(new ClassPathResource("配置文件.yml"));
    configurer.setProperties(yaml.getObject());
    return configurer;
}
```

### 注解配置

#### Controller类配置

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

// 异常处理
@ExceptionHandler
public 返回值 方法名(Exception e)
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

```java
// 事务
@Transactional
```

```java
// 表单验证
public String 方法(@Valid 对象类型 对象名, BindingResult bindingResult)
```

#### 实体类配置

```java
// 配置主键
@Id
// 自增
@GeneratedValue

// 表单验证
@Min(value = 最小值, message = 错误信息)
```

#### AOP配置

依赖

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-aop</artifactId>
</dependency>
```

类

```java
@Aspect
@Component
public class HttpAspect {
    @Before("execution(public * 拦截类.*(..))")
    public void 方法名(JoinPoint joinPoint)
      
    @Pointcut("execution(public * 拦截类.*(..))")
    public void 切点方法()
    @Before("切点方法")
    public void 方法名()
    @After("切点方法")
    public void 方法名()
}
```

#### 统一异常处理

```java
@ControllerAdvice
public class ExceptionHandle {
    @ExceptionHandler(Exception.class)
    @ResponseBody
    public Result handle(Exception e)
}
```

### 单元测试

```java
@RunWith(SpringRunner.class)
@SpringBootTest
```

打包时跳过单元测试: ` mvn clean package -Dmaven.test.skip=true `

### 日志

```java
private final Logger logger = LoggerFactory.getLogger(当前类.class);

logger.info();
logger.debug();
logger.error();
```

application.yml配置

```yaml
logging:
  pattern:
  	# 输出格式
    console: "%d - %msg%n"
  file: 日志文件
  path: 日志文件目录
  level: 日志级别
  	# 给单独类设置日志级别
  	全类名: 日志级别
  	
spring:
  output:
    ansi:
      enabled: detect # detect always never
```

### SpringBoot应用

```java
@Controller
@Configuration
@SpringBootApplication
public class HelloWorldApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloWorldApplication.class, args);
    }

    @RequestMapping("/hello")
    @ResponseBody
    public String hello(){
        return "hello world";
    }
}
```

```java
// 自动配置,SpringBoot会根据项目中依赖的jar包自动配置
@EnableAutoConfiguration
// 默认扫描@SpringBootApplication所在类的同级目录及它的子目录
@ComponentScan
```

### 返回Date格式化

```properties
spring.jackson.date-format=yyyy-MM-dd HH:mm:ss
spring.jackson.time-zone=GMT+8
```

#### 热部署

```xml
spring-boot-devtools
```

### SpringScheduled

```java
// 启动类注解
@EnableScheduling

// 方法注解
@Scheduled(参数)
// 参数
fixedRate=毫秒 // 调用间隔时间(不等待上一次调用完成)
fixedDelay=毫秒 // // 上次调用后间隔执行时间
initialDelay=毫秒 // 初次执行任务之前需要等待的时间
```

#### cron

```shell
# 字段含义
秒 分钟 小时 日 月 星期 年
# 
* # 匹配任意值
值1/值2 # 从值1时间开始,间隔值2时间执行
值1,值2 # 在值1和值2时间执行
值1-值2 # 在值1和值2时间内,每时间单位执行一次
```

### SpringCache

注解

```java
// 启动类注解
@EnableCaching
// 存入缓存
@Cacheable(value = "缓存名", key = "#键")
// 删除缓存
@CacheEvict(value = "缓存名", key = "#键")
```

### 注入Bean

方法一

```java
@Configuration
public class 配置类 {
    @Bean
    public 注入类 对象名() {
        return new 注入类();
    }
}
```

方法二

```java
@Component
public class 注入类 {
    @Value("值")
    private 字段类型 字段名;
    
    get/set
}
```

