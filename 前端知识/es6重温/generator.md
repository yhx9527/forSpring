# Generator

generator是一个状态机，返回一个遍历器，用来遍历状态

```
function* helloWorldGenerator() {
	console.log('123')
  yield 'hello'; //状态1
  let a = yield 'world'; //状态2
  console.log(a)
  return 'ending'; //状态3
}

var hw = helloWorldGenerator();
hw.next()
//{value: 'hello', done:false}
```

**调用后函数不执行**，而是返回一个遍历器

- next方法的参数，**作为上一个yield的返回值**

```
hw.next()
hw.next('123')
//则a等于123
```

**这就使我们可以向函数内部输入值来控制函数**

第一次的next就可以输入值，则需要外包一层

```
function wrapper(generatorFunction) {
  return function (...args) {
    let generatorObject = generatorFunction(...args);
    generatorObject.next();
    return generatorObject;
  };
}

const wrapped = wrapper(function* () {
  console.log(`First input: ${yield}`);
  return 'DONE';
});

wrapped().next('hello!')
// First input: hello!
```

- Generate.prototype.throw()

抛出异常

```
var g = function* () {
  try {
    yield;
  } catch (e) {
    console.log('内部捕获', e);
  }
};

var i = g();
i.next();

try {
  i.throw('a');
  i.throw('b');
} catch (e) {
  console.log('外部捕获', e);
}
// 内部捕获 a
// 外部捕获 b
```

一旦generate函数里面抛出异常没被内部捕获，则下次next()时就结束了，value为undefined

- Generate.prototype.return()

终结generate函数，若有try…finally,则立刻进入finally代码块,return的参数作为最后一次的value

```
function* numbers () {
  yield 1;
  try {
    yield 2;
    yield 3;
  } finally {
    yield 4;
    yield 5;
  }
  yield 6;
}
var g = numbers();
g.next() // { value: 1, done: false }
g.next() // { value: 2, done: false }
g.return(7) // { value: 4, done: false }
g.next() // { value: 5, done: false }
g.next() // { value: 7, done: true }
```



- yield* 后面跟迭代器对象

用于在一个generate函数中执行另一个generate函数

```
let delegatedIterator = (function* () {
  yield 'Hello!';
  yield 'Bye!';
}());

let delegatingIterator = (function* () {
  yield 'Greetings!';
  yield* delegatedIterator;
  yield 'Ok, bye.';
}());

for(let value of delegatingIterator) {
  console.log(value);
}
// "Greetings!
// "Hello!"
// "Bye!"
// "Ok, bye."
```

```
function* gen(){
  yield* ["a", "b", "c"];
  yield* "123"
}

gen().next() // { value:"a", done:false }
```



##### 应用

- 实现状态机

```
//clock有两个状态tick和tock，需要轮流切换
var clock=function*(){
  while(true){
    console.log('tick');
    yield;
    console.log('tock');
    yield;
  }	
}
```

- generate用于网络请求

```
function* main(){
	var result=yield request("www");
	var resp=JSON.parse(result);
	console.log(resp);
}
function request(url){
	makeAjaxCall(url, function(response){
		it.next(response);
	})
}
var it=main();
it.next();
```

