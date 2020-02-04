# Module语法

- **CommonJS运行时加载，将模块生成一个对象，再从对象上提取方法**

- **ES6模块是编译时加载，用哪几个方法就提出几个**

#### export命令

```
// profile.js
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;
```

```
// profile.js
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export { firstName as name1, lastName, year };
```

- 可通过as进行重命名
- 使用export规定的对外接口必须和模块内部变量建立一一对应关系

```
// 报错
export 1;

// 报错
var m = 1;
export m;
// 报错
function f() {}
export f;
//上述都只是输出值
```

- **export输出的，是动态更新的**

```
export var foo = 'bar';
setTimeout(() => foo = 'baz', 500);
```

**而commonjs输出的是值的缓存，不存在动态更新**

#### import

```
import { lastName as surname } from './profile.js';
```

- import输出的只读的，不能改写
- import具有提升效果

```
foo();

import { foo } from 'my_module';

```

- import是静态执行的，故不能使用表达式，变量和if

```
// 报错
import { 'f' + 'oo' } from 'my_module';

// 报错
let module = 'my_module';
import { foo } from module;

// 报错
if (x === 1) {
  import { foo } from 'module1';
} else {
  import { foo } from 'module2';
}
```

- import会执行所加载的模块，但对同一模块不会重复执行



#### export default

不用知道函数名就得到默认导出的模块,且自己命名

```
export default function () {
  console.log('foo');
}
import customName from './export-default';
```

- 一个模块只有一个默认输出
- export default输出的是一个defaullt的变量，后面不能跟变量声明语句
- Export default 输出的应是匿名函数，匿名类

```
// 正确
var a = 1;
export default a;
export default 42;
export default function (obj) {
  // ···
}
export default class { ... }

// 错误
export default var a = 1;
```

#### export和import复合写法

```
export { foo, bar } from 'my_module';

export { es6 as default } from './someModule';

// 等同于
import { es6 } from './someModule';
export default es6;
```

#### import(), 动态加载模块（es2020提案）

返回一个promise对象