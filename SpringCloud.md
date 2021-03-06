### SpringCloud

SpringCloud是一个开发工具集,包含多个子项目.基于Netflix开源组件的进一步封装.



#### SpingCloud组件

+ Eureka 服务发现
  + EurekaServer
  + EurekaClient
+ Config 配置中心
  + ConfigServer
  + ConfigClient
+ Ribbon 通信
  + RestTemplate
  + Feign
+ Zuul 动态路由
+ Hystrix 熔断机制

#### 微服务架构基础框架/组件

+ 服务注册发现
+ 服务网关
+ 后端服务(中间层服务)
+ 前端服务(边缘服务)



### Eureka

+ EurekaServer 注册中心
+ EurekaClient 客户端

启动类注解

```java
// 注册中心
@EnableEurekaServer
// 客户端
@EnableDiscoveryClient
```

配置

```yaml
eureka:
  client:
    service-url:
      # 默认注册中心地址
      defaultZone: http://localhost:8761/eureka/
    # 不注册到注册中心
    register-with-eureka: false
  server:
    # 关闭上线率过低警告
    enable-self-preservation: false
spring:
  application:
  	# 服务名
    name: eureka
```

实现高可用

注册中心互相注册,客户端同时注册多个注册中心

#### LoadBalancerClient

```java
@Autowired
private LoadBalancerClient lc;
// 根据服务名获取服务实例
ServiceInstance si = lc.choose(服务名);
// 获取服务地址
si.getHost();
// 获取服务端口
si.getPort()
```



### Feign

依赖

```xml
<!--Feign通信组件-->
<dependency>
	<groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-feign</artifactId>
</dependency>
```

启动类注解

```java
@EnableFeignClients
```

使用

```java
@FeignClient(name = 服务名)
public interface 类名 {
    @GetMapping(路径)
    方法(@RequestParam("参数名") 参数类型 参数名);
}
```



### ConfigServer统一配置中心

依赖

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

启动类注解

```java
@EnableConfigServer
```

配置

```yaml
spring:
  cloud:
    config:
      server:
        git:
          uri: Git仓库地址
          username: 用户名
          password: 密码
          basedir: 本地仓库路径
```

使用

```url
/分支名(默认master)/服务名-环境.格式(yml,properties,json)
```

#### ConfigClient

依赖

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-config-client</artifactId>
</dependency>
```

配置 bootstrap.yml

```yaml
spring:
  application:
    name: 项目名
  cloud:
    config:
      discovery:
        enabled: true
        service-id: 配置中心名
      profile: 配置名(env)
```



### SpringCloudBus

依赖

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-bus-amqp</artifactId>
</dependency>
```



### SpringCloudStream

依赖

```xml
<!-- 使用RabbitMQ -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
</dependency>
```

配置

```yaml
spring:
  cloud:
    stream:
      bindings:
        消息名(RabbitMQ的Exchange):
          group: 分组名(RabbitMQ的队列名)
          # 将java对象序列化为json
          content-type: application/json
```

使用

```java
// 创建接口
public interface StreamClient {
    @Input("消息名(RabbitMQ的Exchange)")
    SubscribableChannel input();

    @Output("消息名(RabbitMQ的Exchange)")
    MessageChannel output();
}

// 接收端
@Component
@EnableBinding(StreamClient.class)
public class StreamReceiver {
    @StreamListener("消息名(RabbitMQ的Exchange)")
    // 收到消息后发送消息
    @SendTo("消息名(RabbitMQ的Exchange)")
    public String 方法名(消息类型 消息) {
        return 发送的消息;
    }
}

// 发送端
streamClient.output().send(MessageBuilder.withPayload("消息内容").build());
```



### Zuul

路由 + 过滤器 , 核心是一系列的过滤器

#### Zuul的四种过滤器

+ 前置 (Pre) (2.0 Inbound)
+ 路由 (Route) (2.0 Endpoint)
+ 后置 (Post) (2.0 Outbound)
+ 错误 (Error)

#### Zuul1.0 Zuul2.0区别

Zuul1.0: 基于Servlet, 同步调用

Zuul2.0: 基于Netty, 异步调用, 支持HTTP/2

依赖

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-zuul</artifactId>
</dependency>
```

启动类注解

```java
@EnableZuulProxy
```

#### 路由

```url
Zuul地址/服务名/服务接口
```

##### 自定义路由规则

```yaml
zuul:
  # 全部服务忽略敏感头
  sensitive-headers:
  routes:
    # 定义路由规则
    规则名:
      path: /路径名/**
      serviceid: 服务名
    # 简洁格式
    服务名: /路径名/**
```

##### 自定义禁止路由地址

```yaml
zuul:
  ignored-patterns:
    - 路径
```

#### 过滤器

```java
@Component
public class 自定义过滤器 extends ZuulFilter {
    /** 过滤器类型 */
    @Override
    public String filterType() {
        return PRE_TYPE;
    }

    /** 过滤器排序 */
    @Override
    public int filterOrder() {
        return PRE_DECORATION_FILTER_ORDER - 1;
    }

    /** 是否拦截 */
    @Override
    public boolean shouldFilter() {
        return true;
    }

    @Override
    public Object run() {
        RequestContext requestContext = RequestContext.getCurrentContext();
        HttpServletRequest request = requestContext.getRequest();
        HttpServletResponse response = currentContext.getResponse();
        return null;
    }
}
```



### SpringCloudHystrix

+ 服务降级
+ 依赖隔离
+ 服务熔断
+ 监控

依赖

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-hystrix</artifactId>
</dependency>
```

启动类注解

```java
@EnableCircuitBreaker
```

#### 服务降级

```java
// 类默认降级
@DefaultProperties(defaultFallback = "默认降级方法名")
需要降级类

@HystrixCommand
需要降级方法(发生异常时调用默认降级方法)
    
默认降级方法

@HystrixCommand(fallbackMethod = "降级方法名")
需要降级方法(发生异常时调用降级方法)

降级方法
```

```java
// 配置超时时间
@HystrixCommand(commandProperties = {
	@HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds", value = "超时时间")
    })
```

#### 服务熔断

```java
@HystrixCommand(commandProperties = {
            @HystrixProperty(name = "circuitBreaker.enabled", value = "true"),
            @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold", value = "10"),
    		// 休眠时间窗时间(到期后熔断器进入半开状态)
            @HystrixProperty(name = "circuitBreaker.sleepWindowInMilliseconds", value = "10000"),
    		// 断路器打开需要的错误百分比
            @HystrixProperty(name = "circuitBreaker.errorThresholdPercentage", value = "60")
    })
```

#### Feign开启Hystrix

配置

```yaml
feign:
  hystrix:
    enabled: true
```

使用

```java
@FeignClient(name = 服务名, 服务降级类.class)
```

#### 依赖隔离

#### HystrixDashboard

依赖

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-hystrix-dashboard</artifactId>
</dependency>
<dependency>
<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

启动类注解

```java
@EnableHystrixDashboard
```

使用

```url
# 访问HystrixDashboard
服务地址/hystrix
```

### 链路监控

#### SpringCloudSleuth

依赖

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
```

