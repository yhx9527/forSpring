# VUE

#### 1. vue的双向绑定

```
Object.defineProperty(obj, prop, descriptor)
通过利用了Object.defineProperty劫持传进来的数据，在getter的时候订阅重新编译模版的消息，然后通过js监听元素的事件，可以是input，keyup等等，将新的值重新赋值给被劫持的数据，这样就触发了setter，再在setter函数中去发布重新编译模版的消息
```

```
数据双向绑定
<input id="input" type="text"/>
<div id="text"></div>

let input=document.getElementById("input")
let text=document.getElementById("text")
let data={value:""}
Object.defineProperty(data, "value", {
	get:function(){
		return input.value
	},
	set:function(val){
		text.innerHTML=val;
		input.value=val
	}
})
input.onkeyup=function(e){
	data.value=e.target.value;
}
```

#### 延伸：使用Object.defineProperty的缺陷，为什么用proxy进行替代

```
1. VUE因为考虑到性能问题所以没有使用Object.defineProperty对数组下标进行监控
2. Object.defineProperty只能劫持对象的属性，而proxy可以劫持整个对象，并返回一个新对象
3. Proxy不仅可以代理对象，可以代理数组，还可以代理动态增加的属性
```



#### 2. v-model原理

```
v-model是一个语法糖，帮我添加了value属性和input事件，监听用户输入以更新数据
<input v-model="sth"/>
相当于<input :value="sth" @input="sth=$event.target.value"/>
```



#### 延伸：v-model和vuex是否冲突

```
vuex的文档对此进行了描述，是冲突了，但是可以解决
如利用计算属性的setter函数进行vuex的commit
```



#### 3. v-for中使用key的作用

```
在更新组件时判断两个节点是否相同，相同则复用，不相同则删除旧的创建新的
```



#### 4. 理解MVVM

```
由view，model和viewModel三个内容组成
以前使用jquery，如果要刷新ui，就需要获取到dom，再更新ui，这就导致了业务和页面强耦合
在mvvm中，ui是通过数据来驱动，ui上的改变也会改动相关的数据，这样就让我们只需关心业务中数据如何流转，而不需要在直接和页面打交道，viewmodel帮我们干了这件事。
```



#### 5. 单页路由原理

````
1. hash模式
url中#后面的哈希值发生变化不会向服务器发请求，而我们可以通过hashchange事件来监听url，从而实现页面跳转
2. history模式
需要服务端配合重定向
监听popstate事件，解析url
````



#### 6. 单向数据流

```
数据总由父组件传递给子组件，子组件无权修改父组件的数据
方法
父组件可通过v-bind绑定属性向子组件传递数据，子组件通过props来接收
```

#### 延伸：组件通信

````
1. 父组件传递给子组件的数据，子组件通过props接收
2. 子组件通过事件触发方式，向父组件传数据
3. 新建一个vue实例作为中央事件总线，实现任意组件间的通信
4. 使用refs，父组件可通过refs直接访问父祖件
5. $parent和$children来访问
6. Vuex
7. $attrs,包含了不在props中的父组件的属性
$listeners包含了父组件绑定的事件
8. provide，祖先组件提供变量；
子孙组件通过inject获取到这个变量
````

#### 7. vue首页白屏

```
原因：单页应用的html是通过js生成的，首屏需要加载很大的js文件，当网页差的时候就会产生一定程度的白屏

解决方法
1. 减少模块打包体积，分包加载
2. 服务端渲染，在服务端事先拼接好首页需要的html
3. 首页加loading或骨架屏，优化体验
```

#### 延伸：骨架屏

```
页面的大致框架
生成方法
1. 设计个图片作为骨架屏
2. 写个组件作为骨架屏
	通过vue的预渲染将骨架屏组件渲染成html片段，插入到首页模版的挂载点，当前端渲染完成时，会将骨架屏内容替换成真正的页面内容
```

#### 8. Vue的生命周期

- beforeCreate

完成实例初始化，但还没进行数据绑定及dom节点挂载，即data和$el为undefined

- created

已完成数据绑定，属性和方法的运算，watch/event事件回调。还没挂载dom节点，即data有值，$el为undefined

- beforeMount

对template模版进行编译解析，生成虚拟dom,还没渲染

- mounted

已将虚拟dom转成了真实的dom，并挂载到页面上了

- beforeUpdate

数据更新时，新旧虚拟dom通过diff算法得出补丁，在打补丁之前

- updated

通过patch给真实的dom打上补丁，更新完毕，页面相关dom已发生变化

- beforeDestroy

实例销毁之前调用

- destroyed

vue实例销毁之后调用，实例的所有东西被解绑，子实例也被销毁，dom还存在



#### 9.对vue项目的优化

- 代码层面的优化

```
1. v-if和v-show
v-show使用于频繁切换的场景
2. 合理使用computed和watch
computed有缓存特性
3. v-for遍历时添加key
4. 长列表优化，对于不会改变的数据，进行Object.freeze冻结，防止vue劫持数据
5. 组件销毁时，通过addEventListener绑定的事件要进行销毁
6. 图片的懒加载
7.路由懒加载
```

- web层面的优化

```
1. 浏览器的缓存
2. 使用cdn
```

 [性能.md](../前端知识/性能.md) 



#### vue3.0新特性

```
1. 使用typescript进行编写
2. 摇树优化：也就是只导入用到的API
3. 更轻量更快
```

