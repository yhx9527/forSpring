# Reflect

与proxy对象的方法相对应，完成原生行为之后再做额外操作

```
Proxy(target, {
  set: function(target, name, value, receiver) {
    var success = Reflect.set(target, name, value, receiver);
    if (success) {
      console.log('property ' + name + ' on ' + target + ' set to ' + value);
    }
    return success;
  }
});
```

#### 静态方法

- Reflect.apply(target, thisArg, args)
- Reflect.construct(target, args)
- Reflect.get(target, name, receiver)

对于部署了getter的name属性，this绑定的是receiver

```
var myObject = {
  foo: 1,
  bar: 2,
  get baz() {
    return this.foo + this.bar;
  },
};

var myReceiverObject = {
  foo: 4,
  bar: 4,
};

Reflect.get(myObject, 'baz', myReceiverObject) // 8
```



- Reflect.set(target, name, value, receiver)

对于部署了setter的name属性，this绑定的是receiver

- Reflect.defineProperty(target, name, desc)
- Reflect.deleteProperty(target, name)
- Reflect.has(target, name)
- Reflect.ownKeys(target)
- Reflect.isExtensible(target)
- Reflect.preventExtensions(target)
- Reflect.getOwnPropertyDescriptor(target, name)
- Reflect.getPrototypeOf(target)
- Reflect.setPrototypeOf(target, prototype)

**观察者模式**

```
const queueObservers=new Set()
const observe=fn=>queueObservers.add(fn);
const observable=obj=>new Proxy(obj,{set})
function set(target, key, vlaue,receiver){
	const result=Reflect.set(target, key, vlaue,receiver);
	queueObservers.forEach(fn=>fn());
	return result;
}

const person = observable({
  name: '张三',
  age: 20
});

function print() {
  console.log(`${person.name}, ${person.age}`)
}

observe(print);
person.name = '李四';
// 输出
// 李四, 20
```

