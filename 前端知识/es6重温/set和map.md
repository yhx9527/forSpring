# Set和Map

### Set

- 新建

```
let s=new Set()
s.add(1)

new Set([1,2,3])
```

**去除数组,字符串重复**

```
[...new Set(array)]
Array.from(new Set(array))
[...new Set('ababc')].join('')
```

- 操作

```
set.size

set.add(value)
set.delete(value)
set.has(value)
set.clear()
```

- 遍历

```
//配合for...of
set.keys()
set.values()//此为默认遍历器生成函数
set.entries()
set.forEach()


for (let item of set.values()) {
  console.log(item);
}
// red
// green
// blue

let set = new Set(['red', 'green', 'blue']);

for (let x of set) {
  console.log(x);
}
// red
// green
// blue
```

**扩展运算符…**内部使用了for…of循环



**set实现并，交，差**

```
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

let union=new Set([...a,...b])

let interset=new 
Set([...a].filter(x=>b.has(x)))

let difference=new Set([...a].filter(x=>!b.has(x)))
```



### WeakSet

成员只能为对象，对象为弱引用，时刻可能被回收，不可被遍历

```
let ws=new WeakSet()
ws=new WeakSet([[1],[2]])

ws.add(value)
ws.delete(value)
ws.has(value)
```

- 用途

可用于存储dom节点，不用担心节点从文档移除时引起内存泄漏



### Map

对象只能使用字符串当键值，而map的键值可以是各种类型,map的键跟内存地址绑定

- 新建

```
let m=new Map()
m.set(o, '1')
m.get(o)
let m1=new Map([[k1,v1],[k2,v2]])
```

- 操作

```
map.set(k,v).set(k1,v1)
map.size
map.get(k)
map.has(k)
map.delete(k)
map.clear()
```

- 遍历

```
map.keys()
map.values()
map.entries() //此为默认遍历器生成函数
map.forEach()

map[Symbol.iterator] === map.entries
```

**Map与其他数据结构互换**

```
Map转为数组
[...map]
数组转为map
new Map([[k1,v1],[k2,v2]])
map转为对象
Object.fromEntries(map)
对象转为map
new Map(Object.entries(obj))
```



### WeakMap

键值只能为对象，对象为弱引用，时刻可能被回收，不可被遍历

```
let wm=new WeakMap()
wm.set(k1,v1)
wm.get(k1)

wm=new WeakMap([[k1, 'foo'], [k2, 'bar']])

wm.has(k1)
wm.delete(k1)
```

- 用途

可用于需要dom节点作为键名的场合