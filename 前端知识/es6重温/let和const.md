# let和const

## let

- let只在声明他的代码块内有效

for循环设置循环变量的那部分是一个父作用域，循环体内是一个单独的子作用域

- let不存在变量提升
- let存在暂时性死区，即进入当前作用域，变量已经存在但不可获取直到变量声明的那行代码，才可正常使用变量
- 不允许重复声明



## 块级作用域

es5没有块级作用域，可能导致内层变量覆盖外层变量，可能让循环中的循环变量泄漏为全局变量

es6允许块级作用域的嵌套，内层的可访问外层的，外层的无法读取内层的



## const

- 声明一个只读的常量，变量指向的内存地址保存的数据不可改动
- 其他与let类似



**彻底冻结对象**

```
let constantize = (obj)=>{
	Object.freeze(obj);
	Object.keys(obj).forEach((key, i)=>{
		if(typeof obj[key] === "object"){
			constantize(obj[key]);
		}
	})
}
```



声明一个对象

```
var
function
let
const
import
class
```



## 顶层对象

浏览器：window，self

webworker：self

node：global



es2020引入了globalThis作为顶层对象