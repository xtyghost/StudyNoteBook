### 安装

#### docker安装

```shell
# 安装
docker run -d -u root -p 本机端口:8080 -p 本机端口:50000 -v 本机绝对路径:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock jenkinsci/blueocean

# 查看启动日志
docker logs 容器ID

# 进入容器
docker exec -it 容器ID bash
```

### 配置

```shell
# 配置文件路径
/etc/sysconfig/jenkins
# jenkins_home路径
/var/lib/jenkins
```

#### 增加root权限

```shell
# 增加到root组
gpasswd -a root jenkins
# 修改配置
JENKINS_USER=root
```



### 自动部署到服务器

#### 安装插件

 Publish over ssh

### WebHook

插件

Generic Webhook Trigger