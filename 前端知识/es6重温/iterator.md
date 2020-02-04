#Iterator

为数据结构提供统一访问接口

使数据结构成员按某种顺序排列

供for….of进行消费

iterator接口部署在Symbol.iterator中

```
const obj = {
  [Symbol.iterator] : function () {
    return {
      next: function () {
        return {
          value: 1,
          done: true
        };
      }
    };
  }
};
```

- 原生具有iterator接口的

```
Array
Map
Set
String
TypedArray
函数的 arguments 对象
NodeList 对象
```

- 调用iterator的场合

```
解构赋值
扩展运算符
yield*
for...of
Array.from()
Map(), Set(), WeakMap(), WeakSet()（比如new Map([['a',1],['b',2]])）
Promise.all()
Promise.race()
```

- return方法

```
function readLinesSync(file) {
  return {
    [Symbol.iterator]() {
      return {
        next() {
        //必须返回一个对象
          return { done: false };
        },
        return() {
          file.close();
          //必须返回一个对象
          return { done: true };
        }
      };
    },
  };
}
```

当在for…of循环中使用了break或throw时就会触发return方法



**注意！！**

对于普通对象，for…of不能直接使用，必须部署了Iterator接口才可使用

普通对象的遍历，可使用Object.keys(),Object.values(),Object.entries()



### 循环遍历的方法

- for循环
- 数组的forEach，但无法中途退出
- For…in,但它会遍历所有可枚举的属性包括继承来的
- for…of