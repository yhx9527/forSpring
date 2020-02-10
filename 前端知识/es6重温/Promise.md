# Promise

异步编程的一种解决方案，提供一个容器，保存未来才会结束的事件

#### 特点

- 三种状态,一旦变化就不再改变

```
pending（进行中）
fulfilled（已成功）
rejected（已失败）
```

- 一旦新建立即执行，内部错误不抛出

#### 使用

```
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

- promise.then()添加状态改变的回调函数，返回一个promise，可链式调用

- promise.catch()//发生错误时的回调函数，**promise的异常处理**

**promise出现错误不会影响外部代码**

- promise.finally()

- Promise.all([p1,p2,p3])

当所有的promise都是fulfilled时，最终才是fulfilled，有一个rejected就变成了rejected

- Promise.race([p1,p2,p3])

哪个快是哪个

- Promise.allSettled([p1, p2,p3])

**es2020**

当所有promise都完成之后才结束

```
const promises = [ fetch('index.html'), fetch('https://does-not-exist/') ];
const results = await Promise.allSettled(promises);
// [
//    { status: 'fulfilled', value: 42 },
//    { status: 'rejected', reason: -1 }
// ]

// 过滤出成功的请求
const successfulPromises = results.filter(p => p.status === 'fulfilled');

// 过滤出失败的请求，并输出原因
const errors = results
  .filter(p => p.status === 'rejected')
  .map(p => p.reason);
```

- Promise.any([p1,p2,p3]) 提案中

有一个变成fulfilled就会fulfilled，所有的都变成rejected才会是rejected

- Promise.resolve(xxx)

```
1. xxx为promise实例时，返回这个实例
2. 参数是个具有then方法的对象时，执行then方法
3. 参数不是上述时，返回一个resolved的promise对象，并将其作为返回值
```

- Promise.reject()

```
参数会原封不动返回
```

**generate流程管理**

```
function getFoo () {
  return new Promise(function (resolve, reject){
    resolve('foo');
  });
}

const g = function* () {
  try {
    const foo = yield getFoo();
    console.log(foo);
  } catch (e) {
    console.log(e);
  }
};

function run (generator) {
  const it = generator();

  function go(result) {
  	console.log('bbb',result)
    if (result.done) return result.value;

    return result.value.then(function (value) {
    console.log('aaa',value)
      return go(it.next(value));
    }, function (error) {
      return go(it.throw(error));
    });
  }

  go(it.next());
}

run(g);
```

#### 同步函数，异步函数都变成异步

- async

```
const f = () => console.log('now');
(async () => f())()
.then(...)
.catch(...)
```

- new promise

```
const f = () => console.log('now');
(()=>new Promise(resolve=>resolve(f())))()
console.log('next')
```

```
Promise.resolve(f()).then()
console.log('next')
```



- Promise.try()  提案中

```
const f = () => console.log('now');
Promise.try(f);
```

