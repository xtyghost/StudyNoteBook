```shell
# 查看本机所有镜像
docker images
# 拉取镜像
docker pull 镜像名
# 运行镜像
docker run 参数 镜像名
-d # 后台运行
-p 8080:80 # 本机端口:容器端口 映射

# 停止镜像
docker stop 容器ID
# 查看正在运行的容器
docker ps
```

