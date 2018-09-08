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

### SpringCloudEureka

+ EurekaServer 注册中心
+ EurekaClient 服务注册

```java
// 注册中心
@EnableEurekaServer
// 客户端
@EnableDiscoveryClient
```

#### 实现高可用

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
    方法
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

### ConfigClient

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

### RabbitMQ

依赖

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

配置

```yaml
spring:
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest
```

使用

```java
// 发送消息
@Autowired
private AmqpTemplate amqpTemplate;
amqpTemplate.convertAndSend(队列名, 消息内容);

// 接收消息
@RabbitListener(queues = 队列名)
// 创建队列并接收消息
@RabbitListener(queuesToDeclare = @Queue(队列名))
```

### Zuul

路由 + 过滤器 , 核心是一系列的过滤器

#### Zuul的四种过滤器

+ 前置 (Pre)
+ 路由 (Route)
+ 后置 (Post)
+ 错误 (Error)

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
  routes:
    # 
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
@HystrixCommand(fallbackMethod = "降级方法名")
需要降级方法(发生异常时调用降级方法)

降级方法
```

```java
@DefaultProperties(defaultFallback = "默认降级方法名")
需要降级类
```

```java
// 配置超时时间
@HystrixCommand(commandProperties = {
	@HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds", value = "超时时间")
    })
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
服务地址//hystrix
```

### 服务追踪

依赖

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
```

