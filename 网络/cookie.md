# cookie

进行会话管理的一个东西

#### 特点

- 不可跨域

1. 每个cookie都会绑定单一的域名

2. 不同域是不能相互设置cookie的

3. 同一域下的二级域名也不可读取或使用各自cookie

4. `client.com` 不能向 `a.client.com` 设置cookie, 而 `a.client.com` 可以向 `client.com` 设置cookie

读取cookie情况同上

5. 普通请求(浏览器输入url,使用的是httprequest对象)中只会带上同域的cookie，如果是ajax请求（使用的是xhr对象）默认不会带cookie，只有设置了credentials，且服务端设置响应头Access-Control-Allow-Credentials: true，才会带上同域的cookie

#### 属性

http-only设为true就不能通过js脚本来获取cookie值



#### 注意事项

- 有数量(50条)和长度限制(每条4k)
- 不保存机密信息
- 可设置http-only，让js脚本访问不到cookie