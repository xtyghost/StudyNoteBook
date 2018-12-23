### 使用

```shell
# 开发模式启动
consul agent
 # 参数
 -dev # 开发模式
 -server # 指定为服务
 -config-dir=配置文件目录 # 指定配置文件
 -data-dir 目录 # 指定数据存储目录
 -client=ip # 指定IP客户端访问
 -node=名称 # 指定节点在集群中的名称
 -bind=ip # 绑定ip

# 查看集群成员
consul members
# 重载配置
consul reload
# 加入集群
consul join ip地址
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

###

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-consul-discovery</artifactId>
</dependency>
```

