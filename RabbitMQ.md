### AMQP协议模型

```
                             Server
                   +----------------------------+
                   |       Virtual host         |
                   |   +-------------------+    |
                   |   |     Exchange      |    |
+-------------+    |   |     +-------+     |    |
|  Publisher  |  ----------> |       |     |    |
| application |    |   |     +---+---+     |    |
+-------------+    |   |         |         |    |
                   |   |      Message      |    |
                   |   |       Queue       |    |
+-------------+    |   |     +--------+    |    |
|  Consumer   |  <---------  +--------+    |    |
| application |    |   |     +--------+    |    |
+-------------+    |   |     +--------+    |    |
                   |   +-------------------+    |
                   +----------------------------+
```

### SpringBoot集成

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
amqpTemplate.convertAndSend(交换机名, 路由, 消息内容);

// 接收消息
@RabbitListener(queues = 队列名)
// 创建队列并接收消息
@RabbitListener(queuesToDeclare = @Queue(队列名))
// 绑定Exchange
@RabbitListener(bindings = @QueueBinding(
	value = @Queue("队列名"),
	exchange = @Exchange("交换机名"),
    key = "路由"
))
```