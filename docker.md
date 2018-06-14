```shell
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
-d # 后台运行
-p 8080:80 # 本机端口:容器端口 映射

# 停止镜像
docker stop 容器ID
```

```shell
# 列出本机正在运行的容器
docker container ls
docker ps
# 列出本机所有容器，包括终止运行的容器
docker container ls -a
docker ps -a
# 删除容器
docker container rm 容器ID
docker rm 容器ID
# 删除所有容器
docker rm $(docker container ls -aq)
# 删除所有已退出容器
docker rm $(docker container ls -f "status=exited" -q)
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

