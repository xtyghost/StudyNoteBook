### 安装

```shell
# 添加源 /etc/yum.repos.d/mongodb-org-4.0.repo
[mongodb-org-4.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.0.asc

# 安装
sudo yum install -y mongodb-org
# 启动
service mongod start
# 停止
service mongod stop
# 重启
service mongod restart
# 登陆
mongo
```

### 用户操作

#### 创建用户

```javascript
use admin
// 创建用户
db.createUser({
    user: "用户名",
    pwd: "密码",
    roles: ["权限"]
})
// 修改密码
db.updateUser("用户名", {pwd: "新密码"})
// 删除用户
db.system.user.romve({user: "用户名"})
```

#### 启用用户验证

```shell
# 修改配置文件 /etc/mongod.conf
security:
  authorization: enabled
# 重启
```

#### 登陆

```javascript
use admin
db.auth("用户名","密码")
```



### 数据库操作

```shell
# 查看所有数据库
show dbs
# 创建数据库
use 数据库名
# 删除数据库
db.dropDatabase()
```

### 集合操作

```shell
# 创建集合
db.createCollection("集合名")
# 查询所有集合
show collections
# 删除集合
db.集合名.drop()
```

### 数据操作

```shell
# 创建集合并插入数据
db.集合名.insert({列,值})
# 查询数据
db.集合名.find(条件)
# 修改数据
db.集合名.update({},{$set:{}})
# 删除数据
db.集合名.remove(条件)
```

### 导入导出数据

```shell
# 导入数据
mongoimport -d 数据库 -c 集合 --file 文件
```

