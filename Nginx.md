### Nginx安装

#### 源码编译安装

```shell
# 安装gcc的环境
yum install gcc-c++
# 安装pcre库
yum install -y pcre pcre-devel
# 安装zlib库
yum install -y zlib zlib-devel
# 安装openssl库
yum install -y openssl openssl-devel
# 解压缩
tar zxf nginx-1.8.0.tar.gz
# 使用configure命令创建makeFile文件
./configure \
--prefix=/usr/local/nginx \
--pid-path=/var/run/nginx/nginx.pid \
--lock-path=/var/lock/nginx.lock \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module \
--http-client-body-temp-path=/var/temp/nginx/client \
--http-proxy-temp-path=/var/temp/nginx/proxy \
--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
--http-scgi-temp-path=/var/temp/nginx/scgi
# 在/var下创建temp及nginx目录
mkdir /var/temp/nginx/client -p
# 编译
make
# 安装
make install
```

#### yum源安装

添加源

/etc/yum.repos.d/nginx.repo

```shell
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1
```

```shell
# 安装
yum install nginx
```

### Nginx启动停止

```shell
# 启动 sbin目录下
./nginx
# 完整停止(推荐)
./nginx -s quit
# 快速停止
./nginx -s stop
# 重新加载配置文件
./nginx -s reload
```

### 虚拟主机配置

```shell
# 通过域名区分
# 虚拟主机一
server {
        listen       80;
        server_name  域名一;

        location / {
            root   目录一;
            index  index.html index.htm;
        }
    }
    
# 虚拟主机二
server {
        listen       80;
        server_name  域名二;

        location / {
            root   目录二;
            index  index.html index.htm;
        }
    }
# 也可以通过端口区分
```

### 配置反向代理

```shell
# 服务器一
upstream 名一{
  server 服务器地址;
}
server {
        listen       80;
        server_name  域名一;

        location / {
        	# 代理转发
            proxy_pass	http://名一
            proxy_set_header Host $http_host;
    		proxy_set_header X-Real-IP $remote_addr;
    		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    		proxy_set_header X-Forwarded-Proto $scheme;
        }
    }

# 服务器二
upstream 名二{
  server 服务器地址;
}
server {
        listen       80;
        server_name  域名二;

        location / {
        	# 代理转发
            proxy_pass	http://名二
        }
    }
```

### 配置负载均衡

```shell
upstream 名一{
  server 服务器一地址;
  # 配置权重,默认都为1
  server 服务器二地址 weight=2;
}
server {
        listen       80;
        server_name  域名一;

        location / {
        	# 代理转发
            proxy_pass	http://名一
            index  index.html index.htm;
        }
    }
```

### 实现高可用

keepalived绑定虚拟ip

从服务器给主服务器发心跳包