# async

async为generate函数的语法糖，内置执行器，返回一个promise

async返回一个promise对象，async函数return值最为then取到的参数，异常可被catch捕获到

```
async function f() {
  throw new Error('出错了');
}

f().then(
  v => console.log(v),
  e => console.log(e)
)
```

async返回的promise对象得等函数中await都执行完状态才会变化

await后面跟promise对象或含有then函数的对象(当成promise处理)，若不是则直接返回对应值

```
async function f() {
  // 等同于
  // return 123;
  return await 123;
}

f().then(v => console.log(v))
// 123
```

await出现异常，则同样可被catch捕获

```
async function f() {
  await Promise.reject('出错了');
}

f()
.then(v => console.log(v))
.catch(e => console.log(e))
// 出错了
```

使用try…catch防止出现异常时函数停止执行

```
async function f() {
  try {
    await Promise.reject('出错了');
  } catch(e) {
  }
  return await Promise.resolve('hello world');
}

f()
.then(v => console.log(v))
// hello world

async function f() {
  await Promise.reject('出错了')
    .catch(e => console.log(e));
  return await Promise.resolve('hello world');
}

f()
.then(v => console.log(v))
```



#### 注意点

- 为防止await后面出现rejected终止函数，可使用下面两种方式

```
async function myFunction() {
  try {
    await somethingThatReturnsAPromise();
  } catch (err) {
    console.log(err);
  }
}

// 另一种写法

async function myFunction() {
  await somethingThatReturnsAPromise()
  .catch(function (err) {
    console.log(err);
  });
}
```

- 如果不是继发关系的，应并发执行

```
// 写法一
let [foo, bar] = await Promise.all([getFoo(), getBar()]);

// 写法二
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bar = await barPromise;
```

#### async实现原理

generate函数+自动执行器

```
spawn(function*(){}) //自动执行器，接收generate作为参数

function spwan(genF){
	return new Promise((resolve, reject)=>{
		const gen=genf();
		function step(nextF){
			let next;
			try{
				next=nextF();
			}catch(e){
				return reject(e);
			}
			if(next.done){
				return resolve(next.value);
			}
			Promise.resolve(next.value).then(data=>{
				step(()=>gen.next(data)),
				e=>step(()=>gen.throw(e));
			})
		}
		step(()=>gen.next());
	})
	
}
```



#### 依次执行写法对比

- Promise

```
function chainPromise(data, animations){
	let ret=null;
	let p=Promise.resolve();
	for(let anim of animations){
		p=p.then(val=>{
			ret=val;
			return anim(data);
		})
	}
	return p.catch(e=>{}).then(()=>ret)
}
```

```
promiseArr.reduce((chain, cur)=>{
	chain.then(()=>cur)
}, Promise.resolve())
```



- generate

```
function chainAnimationsGenerator(elem, animations) {

  return spawn(function*() {
    let ret = null;
    try {
      for(let anim of animations) {
        ret = yield anim(elem);
      }
    } catch(e) {
      /* 忽略错误，继续执行 */
    }
    return ret;
  });

}
```

- async

```
async function chainAnimationsAsync(elem, animations) {
  let ret = null;
  try {
    for(let anim of animations) {
      ret = await anim(elem);
    }
  } catch(e) {
    /* 忽略错误，继续执行 */
  }
  return ret;
}
```

**继发操作**

```
async function logInOrder(urls) {
  for (const url of urls) {
    const response = await new Promise((resolve)=>{
    	setTimeout(resolve,1000,1)
    });

  }
}
```

**并发操作**

通过map，按顺序依次调用回调函数

```
async function logInOrder(urls) {
  const bing = urls.map(async url=>await new Promise((resolve)=>{
    	setTimeout(resolve,1000,1)
    }))
  for(let b of bing){
  	b.then(console.log)
  }
}
```



#### 顶层await (提案中)

