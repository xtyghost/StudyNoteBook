#### 安装

```shell
# 已安装docker
# 添加镜像源 /etc/docker/daemon.json
{
    "registry-mirrors": ["https://fy707np5.mirror.aliyuncs.com"]
}
# 重载配置
systemctl daemon-reload
# 重启docker
systemctl restart docker
# 启动
docker run -d --restart=unless-stopped -p 8080:8080 rancher/server
```

