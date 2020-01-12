# css/html

#### 1. css权重及引入方式

```
!important 最高
内联样式 权重1000
id选择器 权重100
类选择器，伪类选择器，属性选择器 权重10
标签选择器，伪元素选择器 权重1
子选择器，相邻选择器 权重0
```

```
引入方式
1. 内联方式，直接在style属性添加css
<div style="background: red"></div>
2. 嵌入方式， 在<style>标签
<style>
	.content {
		background: red;
	}
</syle>
3. 链接方式, 使用link, 会同html文件一起加载
<link rel="stylesheet" type="text/css" href="style.css">
4. 导入方式, 使用css规则@import，会等页面全部下载完毕后再加载
<style>
	@import url(style.css);
</style>
```

**样式优先级**：行内样式(内联方式)> (内部样式(嵌入)，外部样式（链接，导入）) 采用就近原则

#### 2. a标签全部作用

```
1. 超链接，跳转页面
2. 锚点， 可用于页面定位 
<a href="#target"></a> 可定位到id为target的位置
```

#### 3. 用css画三角形

```
把元素的宽度和高度都设为0，然后设置边框样式，其中一个有颜色，其他透明
.tri{
	width: 0;
  height: 0;
  border-top: 40px solid transparent;
  border-left: 40px solid transparent;
  border-right: 40px solid transparent;
  border-bottom: 40px solid red;
 }
```



#### 延伸：html文件

```
<!DOCTYPE html>
<html>
    <head>
        <meta http-equiv="content-type" content="txt/html; charset=utf-8" />
        <title>test</title>
    </head>
    <body>
        
    </body>
</html>
```

#### 4. 水平垂直居中

- 行内元素的水平居中

```
设置其块级父元素的text-align
#container{
	text-align: center;
}
```

- 块级元素的水平居中

```
设置其左右margin为auto
#center{
	margin: 0 atuo;
	width: 50px;
}
```

- 多个块级元素的水平居中

```
1. 将水平排列的块级元素设display:inline-block，然后父级元素设置text-align:center
#container{
	text-align: center;
}
#center {
	display: inline-block;
}
2. 利用flex布局,只设置父级容器即可
#container {
	display: flex;
	justify-content: flex;
}
```



- 已知元素高度

```
1. 使用绝对定位和margin负值
#container {
	position: relative;
}
#center{
	width: 100px;
	height: 100px;
	position: absolute;
	top: 50%;
	left: 50%
	margin: -50px 0 0 -50px;
}
2. 绝对定位和四个方向置为0
#container {
	position: relative;
}
#center {
	postion: absolute;
	height: 100px;
	widht: 100px;
	margin: auto;
	top:0;
	left:0;
	right:0;
	bottom:0;
}
```

- 未知高度

```
1. 当要居中的为inline或inline-block时，可如下设置父级容器
#container{
	display: table-cell;
	text-align: center;
	vertical-align: middle;
}
2. 使用transform
#container {
	postion: relative;
}
#center {
	position: absolute;
	top: 50%;
	left: 50%;
	trnasform: translate(-50%, -50%);
}
3. flex布局
#container {
	display: flex;
	justify-content: center;
	align-items: center;
}
```



#### 5. 元素种类的划分

- 按占有空间分

```
行内元素
1. 元素一行放置，放不下才在下一行
2. 不能设置宽高
3. 不能设置水平的margin和padding

块级元素
独占一行，宽高，间距都可设置
```

- 按元素本身特点划分

```
替换元素
1. 元素需要根据标签和属性，才能决定显示的内容，如img，input；
2. 替换元素是可设置宽高的，因此虽然img是行内元素，但可设置宽高

不可替换元素
```



#### 6. 盒模型及其理解

```
标准盒子模型(content-box)：宽高=content的宽高

IE盒子模型(border-box): 宽高=content+padding+boder的总宽高

可通过box-sizing进行切换
现在流行css框架几乎都采用border-box
```



#### 7. 定位方式及其区别

```
static
正常布局，top,right,bottom, left, z-index无效

absolute
相对于最近已定位的祖先来定位，若没有的话则相对于最初的包含块,脱离文档流

relative
相对于元素在文档中的初始位置，没有脱离文档流

fixed
相对于视口进行定位，若祖先元素有下面情况，则会使该元素相对于祖先元素定位
1. transform属性不为none
2. perspective属性不为none
3. filter属性不为none

sticky
可设置left,right,top,bottom之一为阈值，如果小于阈值为relative定位，超过阈值为fixed定位。
可用于实现吸顶效果，但有兼容问题
```



#### 延伸：层叠上下文

```
html元素的三维概念，相对于电脑屏幕有一个z轴，元素按照优先级顺序堆叠摆放

创建层叠上下文
1. z-index
2. transform不为none
3. position:fixed
4. opacity小于1的元素
5. filter不为none
6. perspective不为none

```



#### 8. margin塌陷以及合并

```
对于元素之间没有被非空内容，padding，border这些分隔开的，
相邻元素，上面元素设置了margin-bottom，下面元素设置了margin-top，合并时会选择外边距较大的
对于包含元素，父元素和他的相邻的子元素同时设置了垂直方向margin的话，也是会发生重叠,合并时选择最大者
```



#### 延伸： BFC和IFC

```
BFC,块级格式化上下文，是页面上的一个渲染区域，里面只有块级盒子，规定了他们如何布局，BFC内部的元素不受外面影响

布局规则
1. 盒子在垂直方向上一个个摆放
2. 同一个BFC上的盒子垂直的margin会叠加
3. BFC不会与浮动元素重叠
4. 计算BFC高度时，浮动元素也算在内

触发条件
1. float不为none
2. position为fixed和absoulte
3. display为inline-block，flex，table-cell
4. overflow：hidden

应用
1. 解决margin叠加问题
使用一个形成bfc的标签来包裹
2. 清除浮动
使用一个形成bfc的标签来包裹
3. 实现两栏布局
因为BFC区域不与浮动元素相重叠
```

```
IFC, 内联格式化上下文，用于规定内联盒子的格式化规则

当一个块元素只包含内联级别元素时就会生成IFC
根据它的布局规则可用来实现文本垂直水平居中
```



#### 9.浮动模型及清除浮动

```
float：left, right
被移除出文档流，向左或向右平移，直到遇到容器边框或另一个浮动元素
浮动后可使元素在一行显示，并可设置宽高，类似inline-block

清除浮动(把浮动元素拽回文档流)
1. 结尾加一个空标签,设置clear:both
2. overflow：hidden形成BFC清除浮动
3. 使用父元素的after伪元素代替空标签，设置clear:both,dipaly:block
```



#### 10.display

```
指定元素的显示类型
display:
none,inline,block,inline-block,list-item,table,flex

```



#### 11. 圣杯和双飞翼

```
都是三栏布局的方式
圣杯利用了margin负值, 左右填充到容器的padding中
<div class="container">
	<div class="main"></div>
	<div class="left"></div>
	<div class="right"></div>
</div>
<style>
div{
	height:50px;
}
.container{
	padding-left: 150px;
  padding-right: 100px;
}
.main{
	float:left;
	width:100%;
	background: black;
}
.left{
	float: left;
	width: 150px;
	margin-left: -100%;
	position:relative;
	left: -150px;
	background: red;
}
.right{
	float: left;
	width: 100px;
	margin-left: -100px;
	position:relative;
	left: 100px;
	background: green;
}
</style>


```

```
双飞翼
<div>
	<div class="container">
		<div class="main">
	</div>
	<div class="left"></div>
	<div class="right"></div>
</div>

<style>
div{
	height: 50px;
}
.container {
	float: left;
	width: 100%;
}
.main{
	margin-left: 150px;
	margin-right: 100px;
	background: black;
}
.left {
	float: left;
	width: 150px;
	margin-left: -100%;
	black:red;
}
.right {
	float: left;
	width: 100px;
	margin-left: -100px;
	black:green;
}
</style>
```



#### 11.flex布局

```
flex: flex-grow flex-shrink flex-basis
flex-grow 如何分配剩余空间，初始值为0
flex-shrink 如何缩小，初始值为1
flex-basis flex-items初始大小

flex-direction: row, column 摆放方向
flex-wrap: wrap,nowrap 换行

justify-content 子元素在水平方向上的摆放
align-items: 子元素在垂直方向上的摆放
order 子元素如何排列
```

```
flex实现三栏布局

<div class="container">
	<div class="main"></div>
	<div class="left"></div>
	<div class="right"></div>
</div>

<style>
	div{
		height: 50px;
	}
	.container {
		display: flex;
	}
	.main {
		flex-grow:1;
		order:1;
		background: black;
	}
	.left {
		flex: 0 0 150px;
		order: 0;
		background: red;
	}
	.right {
		flex: 0 0 100px;
		order: 2;
		background: green;
	}
</style>
```



#### 12. px， em, rem, %, vw,vh

```
px: 像素
em: 以父元素的font-size作为参考物
rem: 以根元素的font-size作为参考物
%：以父元素的大小设置比率
vw: 相对视口的宽度(兼容问题)
vh: 相对视口的高度(兼容问题)
```

#### 延伸：padding，margin设置%，是基于父元素的宽度

**因此可利用该特性生成固定宽高比**

```
<div class="container">
	<div class="inner">
	</div>
</div>

.container{
	width:100px;
}
.inner {
	width: 100%;
	height:0;
	padding-top: 75%;
	background: red;
}
```



#### 13. sass，less

```
都是css预处理器，添加了诸如变量，函数，混合等功能让css更具维护性和扩展性
less基于js运行
sass基于ruby
```



#### 14. 媒体查询

```
用在响应式设计里面，为不同的媒介规定不同的样式

@media(max-width: 700px){
	样式
}
```



#### 15. H5的语义化作用及语义化标签

```
作用：
1. 更好呈现内容结构和代码结构，更具可读性，便于维护
2. 有利于SEO

标签
header--标题
nav-- 导航
article--文章
section -- 节/段
aside --侧边栏
footer--页脚
```

#### 延伸: SEO

```
SEO，搜索引擎优化
利用搜索引擎的搜索规则，提高网站在搜索引擎内的排名，这个手段就叫SEO

实现
从爬虫爬取的三个基本过程来看如何做到SEO优化
1. 页面抓取
可以主动将页面提交给搜索引擎的接口，让其收录
对于单页应用的话，做服务端渲染
url层级不要太深，在页面里增加导航，让页面层次分明
2. 分析入库
搜索引擎对于已经收录的信息可能不再重复收录
对于网站的内容，应要及时的更新，如果是高质量的原创更好
3. 检索排序
搜索引擎会对页面进行打标签之类的，所以我们可以适当增加页面的关键词密度，增加网站链接，使其互投权重，安装搜索引擎提供的统计代码，以便于我们对网站不断优化，提高用户留存率
```



#### 16. websocket

```
是一个双向通信的协议，同时也有对应的API
建立在TCP的，数据轻量，既可以发送文本也可以发送二进制数据，没有同源限制
```

#### 17. webworker，serviceworker

```
webworker是浏览器提供的使js具有多线程的解决方案
webworker会新开一个线程，我们可将复杂的计算放在该线程中执行，js主线程和该线程通信通过postmessage

缺点
worker中不能访问dom，拥有自己的环境，有兼容问题
```

```
serviceworker,基于webworker实现的，可以被用来做本地缓存或请求转发
```

#### 延伸： web应用缓存方案

- 浏览器请求头的强缓存和协商缓存

- h5提供了APP cache解决静态文件存储的问题
- localstorage
- indexedDB
- serviceworker

#### 延伸： http强缓存和协商缓存

```
浏览器加载资源的步骤
看是否命中强缓存，进行协商缓存，直接从服务器端加载

强缓存，有expires和cache-control设置资源过期时间，cache-control优先于expires
没命中强缓存，就会进行协商缓存，浏览器发送请求查看是否资源更改，若返回304则进行使用已有的缓存，否则就会请求新的资源
协商缓存通过last-modified，if-modified-since和etag，if-none-match两对字段进行控制
```



#### 18. css3新特性

```
动画 transition，transform，animation
边框 border-radius，box-shadow，border-image
文字效果 text-shadow
渐变效果 linear-gradient
```



#### 19.html5新特性

```
语义化标签
拖拽API
音视频API
画布API
localstorage
sessionstorage

```



#### 20. 如何实现响应式布局

```
响应式：通过检测视口分辨率，来展现不同的布局和内容
自适应：需要开发多套界面， 通过检测视口分辨率，来返回不同的页面
```

- 媒体查询
- 使用百分比
- 使用rem
- 使用vw/vh



