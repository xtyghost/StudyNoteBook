### SpringCloud

SpringCloud是一个开发工具集,包含多个子项目.基于Netflix开源组件的进一步封装.

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