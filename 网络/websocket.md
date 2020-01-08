# websocket

#### 定义

解决了http只能由客户端发起单向通信的问题

#### 特点

1. 建立在TCP之上
2. 握手采用http，默认端口为80和443
3. 数据轻量，可发送文本或二进制数据
4. 没有同源限制
5. 协议标识符ws，加密(TLS)的为wss

#### 用法

- 客户端实现

```
const ws=new WebSocket("wss://echo.websocket.org");
ws.onopen=function(evt){		      console.log("connection open")
	ws.send("hello")
};
ws.addEventListener('open', function (event) {
  ws.send('Hello Server!');
});
ws.onmessage=function(evt){
console.log("receive"+evt.data);
ws.close();
}
ws.onclose=function(evt){
console.log("connection close")
}
ws.bufferedAmount//表示还有多少字节没被发送
```

```
ws.readyState //返回连接状态
//WebSocket.CONNECTING  0表示正在连接
//WebSocket.OPEN  1表示连接成功
//WebSocket.CLOSING  2表示连接正在关闭
//WebSocket.CLOSED  3表示连接关闭或打开连接失败
```



#### 工具

- websocketd

后台不限语言，标准输入就是ws的输入，标准输出就是ws的输出

