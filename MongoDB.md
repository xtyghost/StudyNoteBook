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

