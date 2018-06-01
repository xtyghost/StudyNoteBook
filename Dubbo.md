### 服务提供方

service类使用dubbo的@Service注解

```xml
<!-- 发布服务的服务名 -->
<dubbo:application name="服务名" />
<!-- 配置注册中心 -->
<dubbo:registry address="zookeeper://ip:2181" />
<!-- 配置包扫描 -->
<dubbo:annotation package="service包名" />
```

### 服务消费方

controller类使用@Reference引入service接口

```xml
<!-- 使用服务的服务名 -->
<dubbo:application name="服务名" />
<!-- 配置注册中心 -->
<dubbo:registry address="zookeeper://ip:2181" />
<!-- 配置包扫描 -->
<dubbo:annotation package="controller包名" />
```

