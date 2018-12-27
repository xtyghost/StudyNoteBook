### 使用

#### 命令

```shell
# 开发模式启动
consul agent
 # 参数
 -dev # 开发模式
 -server # 指定为服务
 -config-dir=配置文件目录 # 指定配置文件
 -data-dir 目录 # 指定数据存储目录
 -client=ip # 指定可访问IP
 -node=名称 # 指定节点在集群中的名称
 -bind=ip # 绑定ip
 -join=ip # 加入集群

# 查看集群成员
consul members
# 重载配置
consul reload
# 加入集群
consul join ip地址
```

#### HTTP API

```shell
# 查看所有节点
服务地址/v1/catalog/nodes
# 查看服务信息
服务地址/v1/catalog/service/服务名
```

#### DNS API

```shell
# 查看节点ip
dig @127.0.0.1 -p 8600 节点名.node.consul
# 查看服务ip
dig @127.0.0.1 -p 8600 服务名.service.consul
```

#### KV存储

```shell
# 添加键值
consul kv put 键 值
-flags=标记值 # 指定标记
# 获取键值
consul kv get 键
# 获取键值元数据
consul kv get -detailed 键
# 获取所有键值
consul kv get -recurse
# 删除键值
consul kv delete 键
# 删除指定前缀键值
consul kv delete -resurse 键前缀
```

### 配置

```json
{
  "service": {
    "name": "服务名",
    "tags": ["标签"],
    "port": 端口,
    "connect": { "sidecar_service": {} }
  }
}
```

### Spring Cloud Consul

#### 依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-consul-discovery</artifactId>
</dependency>
<!-- 健康检查 -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

#### 配置

```yaml
spring:
  application:
    name: 服务名
  cloud:
    consul:
      host: consul地址 (默认localhost)
      port: consul端口 (默认8500)
      discovery:
        healthCheckPath: 健康检查地址 (默认/health或/actuator/health)
        healthCheckInterval: 间隔时间 (默认10s)
        tags: 标签1, 标签2
```

### 配置中心

#### 依赖

```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-consul-config</artifactId>
</dependency>
```

#### 配置

```yaml
spring:
  cloud:
    consul:
      config:
        # 配置文件格式
        format: yaml
  profiles:
    # 指定配置文件
    active: dev
```

#### 配置文件

```shell
# consul KV 路径
config/项目名,指定配置文件/data
```

