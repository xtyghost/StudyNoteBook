### Common模块

```javascript
// 创建模块
module.exports = {
    
}

// 导入模块
require('模块路径')
```

### URL模块

```javascript
// 解析URL
url.parse(URL,[是否解析请求参数],[解析没有请求协议])
// 生成URL
url.format(URL对象)
// 生成URL
url.resolve(主机地址,路径)
```

#### querystring

```javascript
// 序列化参数
querystring.stringify(参数对象,[参数连接符],[参数赋值符])
// 解析参数
querystring.parse(URL参数,[参数连接符],[参数赋值符])
// URL编码
querystring.escape(未编码URL)
// URL解码
querystring.unescape(编码后URL)
```

### Call和Apply

```javascript
对象.方法.call(指向对象,方法参数)
```

### events模块

```javascript
var EventEmitter = require('events').EventEmitter;
var life = new EventEmitter();
// 设置最大监听器数量
life.setMaxListeners(数量);
// 添加监听
life.on(事件名, 回调函数);
// 移除监听
life.removeListener(事件名, 回调函数);
// 移除所有监听
life.removeAllListeners(事件名);
// 监听数量
life.listeners(事件名).length;
EventEmitter.listenerCount(life, 事件名);
var isListener = life.emit(事件名, 回调函数参数);
```



### express

```shell
# 安装生成器
npm i -g express-generator
# 生成项目
express 项目名
```

#### 路由

app.js

```javascript
const express = require('express')
const app = express()
const 路由 = require('自定义路由')
app.use('一级路由', 路由)
```

自定义路由

```javascript
const express = require('express')
var router = express.Router()

router.请求方法('二级路由', (req, res, next) => {
    // 获取请求参数
    req.param('参数名')
    // 返回json数据
    res.json(返回json)
})

module.exports = router
```



### PM2

```shell
# 安装PM2
npm i pm2 -g
# 启动项目
pm2 start 文件
# 查看项目
pm2 list
# 停止项目
pm2 stop 项目
# 重启项目
pm2 restart 项目
# 删除项目
pm2 delete 项目
# pm2面板
pm2 monit
```

### mongoose

```shell
# 安装mongoose
npm i mongoose --save
```

#### 连接mongodb

```javascript
const mongoose = require('mongoose')
mongoose.connect('mongodb://用户名@地址:27017/数据库名?authSource=admin')
mongoose.connection.on('connected', () => {
  console.log('mongodb connected success')
})
mongoose.connection.on('error', () => {
  console.log('mongodb connected fail')
})
mongoose.connection.on('disconnected', () => {
  console.log('mongodb connected disconnected')
})
```

#### 定义实体

```javascript
const mongoose = require('mongoose')
const Schema = mongoose.Schema
const 实体名 = new Schema({
  '字段名': 字段类型,
  ···
})

module.exports = mongoose.model('表名', 实体名)
```

#### 查询数据

```javascript
// 简单查询
实体.find({查询参数}, (err, doc) => {})

// 复杂查询
// 分页 跳过条数=(pageIndex-1)*pageSize 查询条数=pageSize
let 查询对象 = 实体.find({}).skip(跳过条数).limit(查询条数)
// 排序 1 升序 -1 降序
查询对象.sort({"排序字段":排序})
// 执行查询
查询对象.exec((err, doc) => {})
```

