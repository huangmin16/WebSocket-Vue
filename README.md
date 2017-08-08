#基于websocket的聊天室
@(WebSocket)[|Vue|Nodejs]
##WebSocket 
服务器端是基于nodejs实现的，使用了nodejs-websocket模块创建服务，当收到消息时调用broadcast函数发送给所有当前已连接的用户。
``` javascript
var ws = require("nodejs-websocket");
var server = ws.createServer(function(conn){
    conn.on("text", function (str) {
        broadcast(server,str)
    })
}).listen(3001)

function broadcast(server, msg) {
    server.connections.forEach(function (conn) {
        conn.sendText(msg)
    })
}
```

##Vue
前端界面是基于Vue2实现的，目前是一个登陆界面和一个聊天界面，登陆界面中输入名称，点击进入进入聊天界面中。
在聊天界面中组件加载完毕后调用mounted()连接WebSocket服务，并监听。
```javascript
  mounted() {
    this.name = sessionStorage.getItem("name")
    var that = this
    this.ws = new WebSocket("ws://127.0.0.1:3001");//服务端地址
    this.ws.onmessage = function (evt) {
      that.getMessage(evt.data)
    }
  },
```
##使用
1. 下载 `git  clone https://github.com/huangmin16/WebSocket-Vue.git`
2. 安装依赖 `npm install`
3. 启动服务 `node sever`
4. 启动客户端`npm run dev`
