# https

### 含义

https（超文本传输安全协议）
给http通信进行加密，加了一层SSL（安全套接层）或TLS（安全传输层协议）
SSL证书由受信任的数字证书颁发机构CA颁发

### 过程

SSL握手协商加密组件

1. 客户端发送一个报文把自身支持的SSL指定的版本，加密组件（其中有加密算法等）的发给服务端
2. 服务端收到后将自身支持的这些东西与之比对，如果没有相同的则断开连接，有相同的从中选择一种加密组件并和公开密钥一起以证书的形式发过去
3. 客户端收到服务端的证书后，首先检查证书的有效性，检查通过之后，浏览器就会生成一个随机密码串，并用服务端给的公钥进行加密，发给服务端
4. 服务端收到后用私钥解密报文，获取到里面的随机密码串。
5. 之后的就是普通的http请求，只不过内容都是用这个密码串进行加密和解密的

总结一句话就是使用**非对称加密算法来交换对称加密算法的密钥**

### 缺点

1. 慢，因为要进行加密解密的处理，消耗网络资源，消耗CPU内存资源；
2. 还要购买证书。