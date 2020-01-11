# javascirpt部分

#### 1. 原始值和引用值类型及区别

```
原始值，是存储在栈中的简单数据段(占据空间固定，查询较快)，也就是它们的值直接存储在变量访问的位置。包括null，undefined，boolean，number，string，symbol这些基本类型
```

```
引用值，是存储在堆中的对象（大小可变，查询速度较慢），表示引用值的变量是一个指针，指向存储着对象的内存地址。
当查询引用类型的变量时，先从栈中读取内存地址，再通过地址找到堆中值
```

习题

```
var a = { name: '前端开发' }
var b = a;
a = null;
//b为{ name: '前端开发' }，因为把栈内存中的a的指向改成了null，而在堆内存中的对象没有印象
```



#### 引申1：堆内存和栈内存

```
与垃圾回收机制有关，为的是使程序运行占用内存最小
	当一个方法执行时，都会有自己的内存栈，这个方法定义的变量会放到这个栈中，当方法结束，内存栈也跟着销毁了
	如果在程序中创建一个对象，对象保存在堆内存中，以便反复利用，当一个方法结束，这个对象如果还在被引用，不会立马销毁，直到没有再被引用为止，垃圾回收机制才会回收掉
```

**注意闭包中的变量保存在堆内存中，因此即使释放了函数仍然可引用到里面的变量**

#### 引申2: js的垃圾回收机制

```
1.不再被使用的引用计数法
当没有指向该对象的引用时，这个对象就可以被清除了。
会出现循环引用的问题，两个对象相互引用，导致无法被回收，引起内存泄漏
2.最常用标记清除算法
清除那些从根部（全局对象）出发无法到达的对象
 步骤1)创建roots列表，这个就是保存着全局变量
    2）递归的搜索所有对象，并标记为可达
    3）对于未被标记的内存，予以回收
对于局部变量很容易判断是否已使用结束，而全局变量很难判断，因此要尽量避免使用全局变量
```

易错习题

```
var a = {n: 1};
var b = a;
a.x = a = {n: 2};

a.x 	// 这时 a.x 的值是undefined
b.x 	// 这时 b.x 的值是{n: 2}
//a为{n:2}
//b为{n:1, x:{n:2}}
```

**检查内存泄漏**：

1. 浏览器可打开开发者工具选择memory，模拟操作，查看内存占用情况
2. 可使用node的process.memoryUsage查看heapUsed属性

**产生内存泄漏的原因**

- 意外的全局变量

如未使用var进行声明，就会创建一个全局变量

所以要使用严格模式避免这个，'use strict'

- dom节点的引用
- 闭包

#### js内存机制

```
js的内存空间分为栈，堆，池
栈存放变量，堆存放复杂对象，池存放常量
v8引擎中堆内存的js对象还分为新生代和老生代
新生代存活周期短，如临时变量，字符串
老生代，生命力顽强，多次垃圾回收仍可存活
```

**从内存角度上看null和undefined**

```
给一个全局变量赋null，表示将这个变量的指针对象和值都清空，js会回收掉
若给一个对象的属性赋值null，则会给这个属性分配一个空的内存，值为null

给一个全局变量赋undefined，表示将这个对象的值清空
如果给一个对象的属性赋undefined表示该值为空值
```



#### 2. 判断数据类型

- typeof

```
可用于显示number，boolean，string，undefined，symbol，object，function
对于null显示的是object
```

- instanceof

```
arr instanceof Array
//即右边函数的原型是否出现在左边对象的原型链中
```

- Object.prototype.toString.call

```
Object.prototype.toString.call(arr) ==="[Object Array]"
```

- Constructor

```
arr.constructor == Array
```



#### 3. 类数组和数组的区别及转换

- 定义

```
类数组指拥有若干索引属性和一个length属性的对象，典型的就是arguments
可以向数组一样读写，但没有数组的那些方法
可以通过call或apply来使用数组的方法
Array.prototype.join.call(arrayLike,'&');
```

- 转成数组

```
Array.from(arrayLike)
[].slice.call(arrayLike)
Array.prototype.splice.call(arrayLike,0)
Array.prototype.concat.apply([],arrayLike)
var arr=[...arguments] //解构
```

- arguments

```
arguments.callee //表示当前执行的函数
arguments.caller //表示调用该函数的函数
```



#### 4. 数组常见的api

```
[].filter(callback)  //返回通过筛选的数组
[].map(callback) //返回一个 元素经过函数变化 后形成的新数组
[].reduce(callback) //返回函数累计处理的结果
eg.[0, 1, 2, 3, 4].reduce((accumulator, currentValue, currentIndex, array) => { return accumulator + currentValue; }, 10 );

[].every(callback)  //对每个元素进行测试，返回一个布尔值
[].some(callback)  //只要一个通过即可，返回boolean
[].fill() //填充值，起始，结束
[].concat(arr) //返回连接好的新的数组
[].flat(层次)// 数组扁平化
[].forEach(callback);
[].join();
[].slice(begin, end);//返回一个新数组
[].splice(start, 删除的数目, 新添的元素)//会裁剪原数组


```

**新鲜易错题**

```
["1", "2", "3"].map(parseInt);
//[1, NaN, NaN]
//相当于
/*  first iteration (index is 0): */ parseInt("1", 0); // 1
/* second iteration (index is 1): */ parseInt("2", 1); // NaN
/*  third iteration (index is 2): */ parseInt("3", 2); // NaN
```

#### 延伸：reduce的应用

- 二维数组扁平化

```
const flattened=[[0,1],[2,3],[3,4]].reduce((arr,cur)=>acc.concat(cur),[]);
```

- 使用reduce实现map

```
Array.prototype.mapUsingReduce=function(callback, thisArg){
	return this.reduce((mappedArray, currentValue, index, array)=>{
	mappedArray[index]=callback.call(thisArg, currentValue, index, array);
	return mappedArray;
	},[])
}
```

- 按顺序执行promise

```
function p1(a) {
  return new Promise((resolve, reject) => {
    resolve(a * 5);
  });
}
...类似的有p2, p3, p4
const promiseArr=[p1, p2, p3, p4];
function runPromiseInSequence(arr, input){
	return arr.reduce((promiseChain, currentFunction)=promiseChain.then(currentFunction),Promise.resolve(input))
}

runPromiseSequence(promiseArr,10).
then(console.log);
```

- 计算数组中元素出现个数

```
var names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];
var countedNames=names.reduce((allNames, name)=>{
	if(name in allNames){
		allNames[name]++
	}else{
		allNames[name]=1;
	}
}, {})
```

- 数组去重

```
myArray.reduce((acc, current)=>{
	if(acc.indexOf(current)===-1){
		acc.push(current);
	}
	return acc;
},[])
```

```
Array.from(new Set(myArray))
```



#### 5.bind,call, apply的区别

```
call,apply都是用来改变this的指向，同时执行调用的函数，两者传参方式不同
call接受参数列表
apply接受参数数组

bind也是改变this的指向，但它会返回一个新的函数，他就是一种柯里化
```



#### 延伸1: call的实现

```
Function.prototype.myCall=function(context){
	let ctx=context || window;
	ctx.fn=this;
	let args = [...arguments].slice(1);
	let result=context.fn(...args);
	delete ctx.fn;
	return result;
}
```



#### 延伸2: apply的实现

```
Function.prototype.myApply=function(context){
	let ctx=context || window;
	ctx.fn=this;
	let result;
	if(arguments[1]){
		result=ctx.fn(...arguments[1]);
	} else {
		result=ctx.fn();
	}
	delete ctx.fn;
	return result;
}
```



#### 延伸3 ： bind的实现

```
Function.prototype.myBind=function(context){
	//if(typeof this!=='function'){
		//throw new TypeError('Error');
	//}
	let _this=this;
	let args=[...arguments].slice(1);
	return function F(){
		if(this instanceOf F){
			return new _this(...args, ...arguments);
		}
		return _this.apply(context, args.concat([...arguments]));
	}
}
```

#### 延伸4： 函数柯里化

```
将一个多元函数，转化成接收单一参数的函数，以便参数复用，延迟执行
```

- 将一个函数转成柯里函数

```
function curry(fn, args){
	let length=fn.length;
	args=args || [];
	return function(){
		let _args=args.slice(0).concat([...arguments]);
		if(_args.length<length){
			return curry.call(this, fn, _args);
		} else {
			return fn.apply(this, _args);
		}
	}
}
var fn=curry(function(a,b,c){
    console.log(a,b,c);
})
fn("a")("b")("c") 

```



#### 柯里化面试题

 实现一个add方法，使计算结果能够满足如下预期：

```javascript
add(1)(2)(3) = 6;
add(1, 2, 3)(4) = 10;
add(1)(2)(3)(4)(5) = 15;
```

**tip**：重写toString方法,直接将函数参与其他运算时，会默认调用toString方法

```
function fn() { return 20 }
console.log(fn + 10);     // 输出结果 function fn() { return 20 }10

fn.toString = function() { return 30 }
console.log(fn + 10); // 40
```



```
function add(){
	let args=[...arguments];
	//闭包保存参数
	let _adder=function(){
		args.push(...arguments);
		return _adder;
	}
	//重写toString
	_adder.toString=function(){
		return args.reduce((a,b)=>a+b);
	}
	
	return _adder;
}
```



#### 6. new的原理

```
1. 新生成一个空对像
2. 将该对象的隐式原型链接到构造函数的显示原型
3. 将构造函数中的this指向该对象，执行构造函数体
4. 返回实例时进行判断
若构造函数没有返回值或返回的值类型，则返回this
若构造函数返回引用类型，则返回这个
```

```
function myNew(){
	let constructor=[].shift.call(arguments);
	if(typeof constructor !== "function"){
		 throw new Error('请传入构造函数');
	}
	let 	   obj=Object.create(constructor.prototype);
	let temp=constructor.apply(obj, arguments);
  return typeof temp ==="object" ? temp : obj;
}
```



#### 7. 正确判断this

```
1.函数通过对象调用，this指向该对象
2.函数直接调用，this指向window
3.构造函数中调用，this指向实例调用
4.函数通过call，apply调用，this指向传入的第一个参数
5.ES6中箭头函数是没有this的，它里面使用的this是父执行上下文的
```

#### 延伸：执行上下文

```
即代码运行的环境，可以把它看成一个对象，它们以栈的形式存储
能产生执行上下文的只有三种
1. 全局代码，产生的是全局上下文，只有一个
2. 函数代码，每次调用都产生一个新的上下文环境
3. eval函数里的代码
```

```
执行上下文对象的三个必要属性有：变量对象，作用域链，this对象

变量对象包括arguments对象，函数内部的变量声明，函数声明
建立过程1)arguments对象 2)函数声明，将函数名变量对像的一个属性 3）变量声明
需注意，函数声明优先于变量声明，若变量和函数重名则将跳过

作用域链包括了当前的变量对象以及父执行上下文的变量对象，因此可通过作用域链访问到上级上下文的变量
```



#### 8.闭包及其作用

```
函数a中返回一个函数b，函数b中使用了函数a的变量，则函数b就叫闭包，用于间接访问函数内部的变量，同时将该变量保留在内存中
```

**经典题**

```
for ( var i=1; i<=5; i++) {
	setTimeout( function timer() {
		console.log( i );
	}, i*1000 );
}
使之每隔一秒输出1，2，3，4，5
```

```
法一：使用闭包
for ( var i=1; i<=5; i++) {
	(function(j){
		setTimeout( function timer() {
			console.log( j );
		}, j*1000 );
	})(i);
}

法二：使用setTimeout的第三个参数
for ( var i=1; i<=5; i++) {
	setTimeout( function timer(j) {
		console.log( j );
	}, i*1000，i);
}

法三：使用let创建块级作用域
for ( let i=1; i<=5; i++) {
	setTimeout( function timer() {
		console.log( i );
	}, i*1000 );
}
原理：let使得每一次循环都会重新声明变量i，随后的每个循环都会使用上一个循环结束时的值来初始化这个变量i

法四：使用try，catch
for ( var i=1; i<=5; i++) {
	try{
		throw(i);
	}catch(j){
		setTimeout( function timer() {
			console.log( j );
		}, i*1000 );
	}
}
catch后面就是一个块级作用域，接收来自try抛出的变量并保存，实现类似闭包的效果
```

**块级作用域（不局限于函数，有{}就形成一个新的作用域）**：实现的就是块里面可访问外面的变量，但外面的访问不到块里面的变量

#### 延伸：var，let， const

```
变量生命周期：声明(作用域注册一个变量)，初始化（分配内存，初始化为undefined），赋值

var:语句执行前，已完成声明和初始化，存在变量提升，没有块级作用域
let：先完成了声明，没有初始化，如果在变量之前就访问，则将报错，这就是let的暂时性死区；当解析到let的那一行才会进行初始化
const：与let类似，有暂时性死区，同时const声明的变量是不能被重新赋值。(对于引用类型来说就是不能更改变量指向的内存地址，但可修改对象的属性)
```



#### 8.原型和原型链

```
每个函数都有prototype属性，他是个对象，里面有constructor属性指向这个函数
每个对象都有__proto__属性，指向它的构造函数的prototype
__proto__将对象连接了起来组成了原型链，使得对象可访问不属于它的属性

特殊的是
1）引擎首先创建了Object.prototype, 因此Object.prototype.__proto__===null以表示终止
2）接着创建了Function.prototype,它是个函数，但它没有prototype，因为引擎认为不需要
3）接着才有function Function(),至于Function.__proto__===Function.prototype，是为了和其他函数保持原型链的一致，所以才将它们相互链接的

```



#### 9. 继承的实现方式及比较

- 原型链继承

```
function SuperType(){
	this.colors=['red','yellow']
}额
function SubType(){}
SubType.prototype = new SuperType()
```

**缺点**：原型对象被所有实例共享，创建子构造函数实例时不能给父构造函数传参

- 借用构造函数继承

```
function SuperType(name){
	this.name=name
}
function SubType(){
	SubType.call(this, 'yhx');
	this.age=23;
}
```

解决了引用类型被共享以及可以向超类传参

- 组合继承

```
function SuperType(name){
	this.name=name;
}
SuperType.prototype.say = function(){}
function SubType(){
	SuperType.call(this, 'yhx');
	this.age=29
}
SubType.prototype=new SuperType('');
SubType.prototype.constructor = SubType;
```

既有实例自己的属性，同时又有公有的方法

**ES5继承特点**:先创建子类的实例对象的this，然后再将父类的方法添加到this上

- ES6,使用class

**ES6继承特点**：先将父类实例对象的属性和方法加到this上(因此必须先调用super方法),再用子类的构造函数修改this

```
class ColorPoint extends Point{
	constructor(x,y,color){
		super(x,y);
		this.color=color;
	}
}
```

**super**

```
1. 可以做函数使用，表示父类构造函数，但返回的是子类的实例。只能用在子类的construcctor函数中
相当于A.prototype.constructor.call(this)
new.target表示当前正在执行的函数
2. 可以做对象，在普通方法中指向父类的原型，静态方法中指向父类
```

**extends**

```
两条继承链
1. 作为对象，子类的原型是父类
B.__proto__=A;
2. 作为构造函数，子类的原型对象的__proto__指向父类的原型对象
B.prototype.__proto__=A.prototype

class MyArray extends Array{}
var arr = MyArray.of(3)
通过第一点使MyArray可以调用属于Array的方法
```



#### 延伸：原型和实例的判定方法

```
1. instanceof
2. isPrototypeOf()
3. Object.getPrototypeOf();
```



#### 10. 深拷贝和浅拷贝

```
都是针对引用类型，浅拷贝只实现一层的拷贝，深拷贝是完全拷贝
```

- 浅拷贝实现

```
1. Object.assign
let b=Object.assign({}, a)
2. 展开运算符
let b={...a}
```

- 深拷贝的实现

```
1. JSON.parse(JSON.stringify(obj));
缺点：会忽略undefined, symbol,不能序列化函数，不能解决循环引用的对象
2. 使用MessageChannel,这会创建一个消息通道(可解决undefined和循环引用的对象)
function structuralClone(obj){
	return new Promise(resolve=>{
		const {port1, port2}=new MessageChannel();
		port2.onmessage = ev =>resolve(ev.data);
		port1.postMessage(obj);
	})
}
var obj={'a':1, 'b':{'c':'b'}}
(async () => {
  const clone = await structuralClone(obj)
})()
```



#### 11. 防抖和节流

```
两者都是用于降低函数执行频率的，区别在于防抖会重新计时，使得在一定时间内只执行一次；而节流不会，因此节流会每隔一段时间执行一次
```

```
防抖简单实现, 缺少立即执行的选项
const debounce=(func, wait=50)=>{
	let timer;
	return function(...args){
		if(timer) clearTimeout(timer);
		timer = setTimeout(()=>{
			func.apply(this, args)
		}, wait)
	}
}
```

```
防抖增加立即执行选项
const debounce=(func, wait=50, immediate=true)=>{
	let timer,context, args;
	const later = ()=>setTimeout(()=>{
		if(!immediate){
			func.apply(context, args);
		}
	}, wait)
	return function(...params){
		if(timer) {
			clearTimeout(timer);
			timer = later();
		} else {
			timer = later();
			if(immediate){
				func.apply(this, params);
			} else {
				context = this;
				args=params;
			}
		}
	}
}
```

```
节流实现，使用时间戳
const throttle=(func, wait=50)=>{
	let previous=0;
	return function(){
		let now=+new Date();
		if(now-previous>wait){
			func.apply(this, arguments);
			previous=now;
		}
	}
}
使用计时器实现
const throttle=(func, wait=50)=>{
	let timer;
	return function(){
		if(!timer){
			timer=setTimeout(()=>{
				timer=null;
				func.apply(this, arguments);
			}, wait)
		}
	}
}
```



#### 12. dom的常见操作

- 节点查找

```
document.getElementById('id')
document.getElementsByClassName('class属性')
document.getElementsByTagName()
document.getElementsByName()
document/element.querySelector('css选择器')
document/element.querySelectorAll('css选择器')
document.documentElement; 获取html
document.body //获取body
document.all[''];//获取页面中所有元素集合
```

- 新建节点

```
document.createElement()
document.createAttribute()
document.createTextNode()
document.createComment();
document.createDocumentFragment();//创建文档片段
```

- 添加新节点

```
element.setAttribute()
element.setAttributeNode()
parent.appendChild();
parent.insertBefore();
parent.removeChild();
```



#### 13. Array.sort()方法和实现机制

```
默认排序：首先会将每个数组项转成字符串，然后按照unicode编码对其进行排序
传入回调函数：返回值大于0则进行交换
arr.sort(function(a,b){
	return a-b;
})
v8引擎：数组长度小于等于 22 的用插入排序，其它的用快速排序（快速排序中子数组长度小于10的使用插入排序）
```



#### 14. ajax的请求过程

```
ajax是一种异步请求数据，来局部更新网页的技术
核心在于XMLHttpRequest API
```

- 步骤

```
1. 创建XMLHttpRequest对象
const xmlhttp=new XMLHttpRequest();
2. 连接服务器
xmlhttp.open('GET', 'demo.php'， true) //请求方法，url，async(同步/异步)
对于post请求要设置请求头
xmlhttp.setRequestHeader("content-type","application/x-www-form-urlencoded");
3. 发送请求
xmlhttp.send(); 
对于post请求可能要使用字符串参数
xmlhttp.send("name=yhx");
4. 服务器作出响应
responseText为字符串形式的响应数据
responseXML为XML格式的响应数据
5. 接收响应数据
对于同步的直接在send后面，使用xmlhttp.responseText即可
对于异步的要在onreadystatechange回调中监听请求状态变化(xmlhttp.readyState)
为4时表示请求已完成

```

**ajax存在跨域问题**

#### 延伸：Fetch

```
是HTML5的一个异步通信的API，原生支持promise
```

- 调用方式

```
1. Promise fetch(String url, [, Object options])
传入url和配置信息
2. Promise fetch(Request req, [, Object options])
传入request对象和配置信息
```



#### 15. String, Math方法

- String

```
str.replace(regexp|substr, newSubStr|function)//字符串替换，返回替换后的新串
对于functon->(match, p1,p2,...) return 替换的内容
其中match表示匹配到的子串，p1,p2表示匹配到的第几个括号里的内容；

str.charAt(index);//返回指定位置的字符
str.indexOf(searchValue, fromIndex可选);//返回字符出现的第一个位置
str.slice(beginIndex[, endIndex]);//返回子串，不改变原字符串
str.split([separator[, limit]]);//拆分字符串为数组
str.substring(indexStart[, indexEnd]);//返回子串
str.trim()；//两端删除空白符
str.toLowerCase();//转成小写
str.toUpperCase();//转成大写
```

- Math

```
Math.abs()
Math.ceil()
Math.floor()
Math.random()
Math.round()
Math.max(value1[,value2,...]);//返回一组数中的最大值
Math.min(value1[,value2,...])
```



#### 16. addEventListener和onClick的区别

```
onClick事件同一时间只能指向唯一对象，多次指定会被覆盖
addEventListener可以给一个事件注册多个listener
addEventListener可以控制触发的阶段（捕获/冒泡），对于相同的事件处理器只会触发一次
```



#### 17. new和Object.create的区别

```
Object.create创建的对象的原型可以指定，new的只能是构造函数
```



#### 18.BOM

####  1. location对象

```
浏览器提供的原生对象，可用于操作url
location.href //整个url
location.host //主机
location.hash //从#开始
location.origin //url协议，主机名，端口
location.search //从？开始

location.assign(url)//跳转到新的url
location.replace(url)//跳转到新的地址，但无法回腿
location.reload() //刷新
```

#### 延伸：url编码/解码

```
encodeURI() //转整个的
decodeURI()

encodeURIComponent() //转片段，元字符(如/，=)也将被转码
decodeURIComponent()
```

#### 2. window对象

```
window.name
window.top //最顶层的窗口
window.frames //框架对象数组

window.alert()
window.open()

```

#### 3. history对象

```
history.back() //后退
history.forward() //前进
history.go(num)
```



#### 19. 浏览器从输入url到页面渲染的整个流程

- DNS解析

将域名转成ip地址

- 建立TCP连接，进行TCP三次握手
- 发送http请求， 如果是https的话进行TLS握手
- 请求服务器时，可能还会经过负责负载均衡的服务器，将请求转发到目标服务器，然后响应请求
- 浏览器收到请求，判断状态码，如果是200则继续解析，如果是400或500则报错，如果是300的会进行重定向
- 浏览器开始解析文件，首先根据html构建dom树，如果有css则构建cssom树
- dom树和cssom树构建完成后开始生成渲染树
- 浏览器根据渲染树调用GPU进行绘制，将内容显示在屏幕上



#### 20. 同源策略，跨域及解决方法和原理

```
浏览器为例安全考虑，从而有同源策略
两个网页同源，指的是他们的协议，域名和端口号要相同
如果不同的就会出现跨域，从而导致
1) ajax请求不能发送
2）dom无法获得
3) cookie, localstorage, IndexDB无法读取
```

**跨域请求的解决方法**

- JSONP

```
本质利用了script,img,iframe标签不受同源策略的限制，可以从不同域加载并执行资源的特性，来实现数据跨域传输
JSONP由两部分组成：回调函数和数据
需要和服务端约定好一个函数名，服务端返回一段js代码，里面调用约定好的回调函数，并传入响应数据，当网页收到这段js代码后，执行这个回调函数。

客户端
<script type="text/javascript">
	function dosomething(jsondata){
		//处理jsondata
	}
</script>
利用script标签进行请求
<script src="http://example.com/data.php?callback=dosomething"></script>

服务端返回的js代码
dosomething(['data'])

客户端即可执行
```

**缺点**：只支持get请求

- CORS

```
跨域资源共享CORS是一个新的W3C标准，新增了一组http首部字段来进行跨域规定.浏览器在请求前会先发一个OPTION方法的预检请求来获知服务端是否允许跨域

header字段（服务端在响应中进行添加）
Access-Control-Allow-Origin
Access-Control-Allow-Methods
Access-Control-Max-Age
...
```

**不同源的iframe相互通信**

```
使用postMessage API

window.postMessage('内容'， 'orgin(协议+域名+端口)')
window.addEventListener('message', function(event) {
  console.log(event.data);
},false);

event.source//发送消息的窗口
event.origin//消息发向的网址
event.data //消息内容
```



#### 21. 浏览器的回流和重绘

```
重绘：更改节点的外观而不影响布局
回流：布局或几何属性改变，回流必定重绘，成本更高,所以要减少回流

方法
1. 使用tanslate替代top(回流)
2. 使用visibility(重绘)替换display:none（回流）
3. 将dom离线后(display:none)再修改
4. 不要把dom节点属性放在循环里当变量
5. 不使用table布局,因为一个小改动就会导致整个table重新布局
6. 动画可使用requestAnimationFrame
7. 将频繁的动画变为图层
```

#### 延伸1: 图层

```
普通文档流就可以看成一个图层，不同的图层之间渲染互不影响
生成新图层：
1. 3d变换：tranlate3d
2. video,iframe标签
3. position：fixed
```



#### 延伸2: requestAnimationFrame

```
window.requestAnimationFrame(callback)
在浏览器下次重绘之前会调用指定的回调函数，重绘频率与浏览器屏幕刷新频率一致
如果要持续更新的话，就要接着调用requestAnimationFrame
```



#### 22. EventLoop

```
js引擎在执行代码时，会将整个script看成一个宏任务，在执行内部代码时如果遇到同步任务就进入主线程等到它执行完成，遇到异步任务就会把它放入事件表注册，当事件满足执行条件的时候就把它放入任务队列，当主线程空闲时就会依次清空任务队列，然后异步任务又分为微任务和宏任务，当主线程空闲时会先清空微任务列表中的任务，然后再执行宏任务的一个任务

js是单线程的，执行时有一个工作栈和任务队列
每次同步任务放入工作栈中，将异步回调放入任务队列中
当同步任务都执行结束，才会取出异步回调进行执行
其中任务队列又分为宏任务和微任务，一开始执行的同步的代码其实就是一个宏任务，执行结束后，会先把执行过程中产生的所有的微任务都先执行完，然后再接着执行下一个宏任务，这样循环下去，就是eventLoop

微任务
promise
mutationObserver
Object.observe
prcess.nextTick

宏任务
setTimeout
setInterval
setImmediate
I/O
UI rendering
script
```



#### 23. ==和===

```
===:严格相等，会比较两个值的类型和值
==: 抽象相等，比较时会先进行类型转换，再比较值
```

**有点意思**

```
[]==![] //true
```



#### 24. setTimeout误差

```
setTimeout是一个宏任务，它的定时指的的什么时候将其放入宏任务队列中，至于执行的话需要它前面的同步任务和微任务，宏任务都执行完，才轮到它，所以就产生了误差
```



#### 延伸： setTimeout模拟setInterval

```
setInterval缺点：
1. 某些间隔会被跳过，因为它会检查上次的定时器是否在队列中，如果有，则跳过这次的,不会重复添加
2. 可能会有多个定时器会连续执行
```

```
setTimeout(function test(){
	setTimeout(test, interval)
}, interval)
```



