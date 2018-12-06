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

### PM2

```shell
# 安装PM2
npm i pm2 -g
# 启动项目
pm2 start 文件
```

