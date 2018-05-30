[WebSocket 教程-阮一峰](http://www.ruanyifeng.com/blog/2017/05/websocket.html)

### WebSocket

```javascript
var ws = new WebSocket("wss://echo.websocket.org");

// 连接成功后回调
ws.onopen = function(evt) { 
  console.log("连接打开"); 
  // 向服务器发送数据
  ws.send("Hello WebSockets!");
};

// 收到服务器数据后回调
ws.onmessage = function(evt) {
  console.log( "接收消息: " + evt.data);
  ws.close();
};

// 连接关闭后回调
ws.onclose = function(evt) {
  console.log("连接关闭");
};
```

#### readyState返回的状态码

|    字段    |  值  |         含义         |
| :--------: | :--: | :------------------: |
| CONNECTING |  0   |       正在连接       |
|    OPEN    |  1   |       可以通信       |
|  CLOSING   |  2   |       正在关闭       |
|   CLOSED   |  3   | 已关闭或打开连接失败 |

