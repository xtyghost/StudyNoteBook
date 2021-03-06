### 镜像命令

```shell
# 查看版本
docker version
# 查看本机所有镜像
docker image ls
docker images
# 删除镜像
docker image rm 镜像ID
docker rmi 镜像ID
# 拉取镜像
docker pull 镜像名
# 运行镜像
docker run 参数 镜像名
--name 容器名 # 指定容器名
-d # 后台运行
-p 本机端口:容器端口 # 端口映射
-v 本机目录:容器目录 # 目录映射
-e 环境变量名=环境变量值 # 设置环境变量
-it # 交互式运行
# 导出镜像
docker save -o 导出文件名 镜像名
# 导入镜像
docker load -i 导入文件名
# 查看镜像分层
docker history 镜像名
```

### 容器命令

```shell
# 列出本机正在运行的容器
docker container ls
docker ps
# 列出本机所有容器，包括终止运行的容器
docker container ls -a
docker ps -a
# 启动容器
docker start 容器ID
# 停止容器
docker stop 容器ID
# 删除容器
docker container rm 容器ID
docker rm 容器ID
# 删除所有容器
docker rm $(docker container ls -aq)
# 删除所有已退出容器
docker rm $(docker container ls -f "status=exited" -q)
# 执行命令
docker exec -it 容器ID 命令
# 查看容器详细信息
docker inspect 容器ID
# 复制文件到容器
docker cp 文件 容器ID:路径
# 查看容器运行日志
docker logs 容器ID
```

#### Data Volume

```shell
docker volume ls
docker volume rm
docker volume inspect
```

### 构建镜像

#### 使用已有容器构建

```shell
# 将容器构建成镜像(镜像内容不可知,不推荐)
docker commit 容器名 镜像名
```

#### 使用Dockerfile构建

Dockerfile

```shell
# 指定基础镜像,scratch不使用基础镜像
FROM 基础镜像
# 设置镜像信息
LABEL maintainer="作者"
LABEL version="版本"
LABEL description="描述"
# 执行命令,避免无用层,合并多条命令,长命令用\换行
RUN 执行命令
# 切换工作目录,没有会自动创建(不要使用RUN cd切换目录, 尽量使用绝对路径)
WORKDIR 目录
# 添加本地文件到镜像,自动解压
ADD 本地文件 目录
# 复制本地文件到镜像
COPY 本地文件 目录
# 定义常量
ENV 常量名 常量值
# 引用常量
"${常量名}"
# 指定容器启动时默认执行命令,启动时指定其他命令,默认命令会被忽略,有多个CMD时,只执行最后一个
CMD 执行命令
# 指定容器启动时一定执行的命令
ENTRYPOINT 执行命令
```

```shell
# 构建镜像
docker build -t 镜像名 dockerfile目录
```

### 远程仓库

```shell
# 登陆docker
docker login
# 设置tag
docker tag 镜像ID 用户名/仓库名:版本
# 推送镜像到仓库
docker push 镜像名
```

### CentOS安装docker

```shell
# 删除旧版本
sudo yum remove docker \
                docker-client \
                docker-client-latest \
                docker-common \
                docker-latest \
                docker-latest-logrotate \
                docker-logrotate \
                docker-selinux \
                docker-engine-selinux \
                docker-engine
# 
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
#
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
# 阿里镜像
sudo yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
#
sudo yum install docker-ce
# 启动docker
sudo systemctl start docker
```

### docker-machine

```shell
# 创建虚拟机
docker-machine create 虚拟机名
# 查看所有虚拟机
docker-machine ls
# 进入虚拟机
docker-machine ssh 虚拟机名
# 停止虚拟机
docker-machine stop 虚拟机名
```

### Docker网络

```shell
# 查看所有network
docker network ls

# bridge
# host
# none
# overlay

# 创建network
docker network create -d 类型 名称
# 连接容器到network
docker network connect network名 容器名
# 查看network详情
docker network inspect network名

# 启动时指定network
--network network名
# 链接到其他容器
--link 容器名
```

### Docker Compose

#### 安装

```shell
# 下载
sudo curl -L "https://github.com/docker/compose/releases/download/1.23.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# 添加执行权限
sudo chmod +x /usr/local/bin/docker-compose
# 查看版本
docker-compose --version
```

#### 配置文件

```yaml
version: '3'

services:

  服务名:
    image: 镜像名
    ports:
      -端口映射
    environment:
      环境变量
    networks:
      - network名

  服务名:
    build:
      context: .
      dockerfile: Dockerfile

volumes:
  mysql-data:

networks:
  network名:
    driver: bridge
```

#### 使用

```shell
# 启动
docker-compose -f 配置文件 up
docker-compose -f 配置文件 up --scale 服务名=数量
# 查看
docker-compose ps
# 停止
docker-compose stop
# 停止并删除
docker-compose down
# 运行
docker-compose start
```

### Docker Swarm

```shell
# 初始化manager节点
docker swarm init --advertise-addr=本机IP
# 加入worker节点
docker swarm join --token manager节点token manager节点IP:2377

# 显示所有节点
docker node ls

# 创建service
docker service create --name 名称 镜像
# 查看所有service
docker service ls
# 修改service数量
docekr service scale 服务名=数量
# 查看service运行进程
docker service ps 服务名
# 删除service
docker service rm 服务名
```

