# Set

```
const s = new Set(arr);
s.size;
s.add(value);
s.has(value);
s.clear();
//遍历
s.keys();
s.values(); //迭代器用到的函数
s.entries();
s.forEach(callback, thisArg);

//交集
let intersect = new Set([...set1].filter(value=>set2.has(value)))
//并集
let union = new Set([...set1, ...set2]);
//差集
let differenct = new Set([...set1].filter(value=>!set2.has(value)));
```



# WeakSet

```js
const ws = new WeakSet(arr);
ws.add(obj);
ws.has(obj);
ws.delete(obj);
```

存储对象，弱引用，随时可能被垃圾回收

不可被遍历

# Map

map只能通过他的方法进行设值，而不能通过类似mp['a']来映射

```
const m=new Map([[1,2], [2,3]]);
m.set(obj, 'abc');
m.get(obj);
m.has(obj);
m.delete(obj);
m.size;
//遍历
m.keys();
m.values();
m.entries();
m.forEach(callback, thisArg);
```



# WeakMap

键值只能为对象，且为弱引用

不可被枚举

```
const wm = new WeakMap();
wm.set(obj,123);
w.get(obj);
w.delete(obj);
w.has(obj);
```

