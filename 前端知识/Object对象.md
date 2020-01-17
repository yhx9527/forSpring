# Object对象

### 实例属性

- **hasOwnProperty**()

用于判断某个对象自身是否含有指定属性

会忽略掉那些从原型链上继承的属性

- **toString**

```
toString把数据类型转换成string类型

每个对象都有toString方法，来源于Object.prototype;
toString方法在自定义对象中未被覆盖，就会返回"[object type]",type是对象的类型
因此可通过Object.prototype.toString.call()来检测对象类型
```

- **valueOf**

```
valueOf把数据类型转换成原始类型

返回对象的原始值
比如对象的话返回对象本身，数字的话返回数字值，函数返回函数本身，字符串返回字符串值
```

**valueOf和toString优先级**

```
有运算符时，valueOf优先于toString
调用valueOf无法运算时调用toString
```

- Object.create

```
以某个对象为原型创建一个新对象
Object.create(obj, descriptor)

属性描述符props
```

#### 属性描述符对象

```
descriptor
数据描述符和存取描述符
{
	configurable,//默认false，属性可配置，即可悲删除
	enumerable, //默认false，可枚举
	value,//默认undefined
	writable,//默认false
	get,//默认undefined
	set//默认undefined
}
```

- Object.defineProperty(obj, prop, descriptor)

返回这个对象

```
给一对象添加或修改属性
```



- Object.assign(target, …sourecs)

返回目标对象

```
将多个对象可枚举的自身属性拷贝到目标对象

```



#### 遍历对象

```
Object.keys,Object.values,Object.entries
```



