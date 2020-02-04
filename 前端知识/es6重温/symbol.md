# Symbol

表示独一无二的值

```
let a=Symbol('1')
不能用点运算符
obj[a]=123
```

- Symbol.prototype.description

```
const sym = Symbol('foo');
sym.description // "foo"

```

- 遍历

```
Object.getOwnPropertySymbols(obj)

Reflect.ownKeys(obj)//可获取到自身所有属性
```

- Symbol.for()

在全局环境（包括不同iframe，service worker）中先搜索，搜索不到再新建

```
Symbol.for("bar") === Symbol.for("bar")
// true
```

**与symbol()的区别**

symbol.for会注册到全局环境中，以供搜索，symbol不会

- Symbol.keyFor()

```
返回已登记的symbol的名字
let s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"
```



### 内置的symbol值

- Symbol.hasInstance

```
instanceof使用的
class MyClass {
  [Symbol.hasInstance](foo) {
    return foo instanceof Array;
  }
}

[1, 2, 3] instanceof new MyClass() // true
```

- Symbol.isConcatSpreadable

```
表示使用concat时，对象是否可以展开
对于类数组对象，当该属性设置为true时就可以展开了
let obj = {length: 2, 0: 'c', 1: 'd'};
['a', 'b'].concat(obj, 'e') // ['a', 'b', obj, 'e']

obj[Symbol.isConcatSpreadable] = true;
['a', 'b'].concat(obj, 'e') // ['a', 'b', 'c', 'd', 'e']
```

- Symbol.species

创建衍生对象时，会使用到该属性

```
class MyArray extends Array {
  static get [Symbol.species]() { return Array; }
}

const a = new MyArray();
//创建衍生对象时，会使用到该属性
const b = a.map(x => x);

b instanceof MyArray // false
b instanceof Array // true
```

- Symbol.match

```
class MyMatcher {
  [Symbol.match](string) {
    return 'hello world'.indexOf(string);
  }
}

'e'.match(new MyMatcher()) // 1
```

- Symbol.replace
- Symbol.search
- Symbol.split
- Symbol.iterator
- Symbol.toPrimitive

上述指向一个方法

- Symbol.toStringTag
- Symbol.unscopables

上述指向一个get拦截的方法