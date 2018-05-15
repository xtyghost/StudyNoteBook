#### 安装SpringBoot CLI

```shell
# MacOS
# 引入Pivotal的tap
brew tap pivotal/tap
# 安装SpringBoot CLI
brew install springboot
# 查看版本
spring --version
```

#### 启动引导Spring

+ @SpringBootApplication
  + @Configuration 使用基于Java配置
  + @ComponentScan 启用组件扫描
  + @EnableAutoConfiguration 开启自动配置