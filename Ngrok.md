### 使用

```shell
# 安装go语言环境
yum install golang
# 克隆源码
git clone https://github.com/inconshreveable/ngrok.git

# 生成证书
openssl genrsa -out base.key 2048
openssl req -new -x509 -nodes -key base.key -days 10000 -subj "/CN=域名" -out base.pem
openssl genrsa -out server.key 2048
openssl req -new -key server.key -subj "/CN=域名" -out server.csr
openssl x509 -req -in server.csr -CA base.pem -CAkey base.key -CAcreateserial -days 10000 -out server.crt

# 替换证书
cp base.pem assets/client/tls/ngrokroot.crt

# 编译
make release-server release-client
# 编译各平台客户端
# mac
GOOS=darwin GOARCH=amd64 make release-client
# windows
GOOS=windows GOARCH=amd64 make release-client

# 启动服务端
生成的服务端 -tlsKey=server.key -tlsCrt=server.crt -domain="域名" -httpAddr=":端口" -httpsAddr=":端口"

# 添加配置文件 ngrok.cfg
server_addr: "域名:4443"  
trust_host_root_certs: false
# 客户端使用
ngrok -config=ngrok.cfg -log=ngrok.log -subdomain=子域名 端口
```

