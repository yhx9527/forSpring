# generate的异步应用

**异步**：任务分两段，先执行一段，然后去执行其他任务，等待准备好之后，再回头执行第二段

**同步**：连续执行



#### Thunk函数

将多参数的函数转成一个只接收回调函数作为参数的单参数函数

```
const Thunk=function(fn){
	return function(...args){
		return function(callback){
			return fn.call(this, ...args, callback);
		}
	}
}
var readFileThunk = Thunk(fs.readFile);
readFileThunk(fileA)(callback);
```

**generate自动执行器**

```
function run(fn) {
  var gen = fn();

  function next(err, data) {
    var result = gen.next(data);
    if (result.done) return;
    result.value(next);
  }

  next();
}

function* g() {
  // ...
  yield thunk函数
  yield readFileThunk('fileA');
}

run(g);
```



**自动执行的关键**

异步操作有结果后要交回执行权

- 回调函数
- Promise在then中交回执行权，即在then中调用next

```
function run(gen){
  var g = gen();

  function next(data){
    var result = g.next(data);
    if (result.done) return result.value;
    result.value.then(function(data){
      next(data);
    });
  }

  next();
}

run(gen);
```

