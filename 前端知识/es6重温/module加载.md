# module加载

#### script标签异步加载

defer：渲染后执行

async: 下载后执行

```
<script src="path/to/myModule.js" defer></script>
<script src="path/to/myModule.js" async></script>
```

**加载es6模块**

默认开启defer

```
<script type="module" src="./foo.js"></script>
```

#### es6和commonjs

- 差异

1. CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用(**在真正执行时再顺着引用找到这个值**)。

2. CommonJS 模块是运行时加载(生成一个对象)，ES6 模块是编译时输出接口(静态定义)。
3. es6模块中，顶层this指向undefined，commonjs的顶层this指向当前模块

**commonjs模块加载原理**：第一加载时执行这个模块，并生成一个对象，之后的取值都是在这个对象的exports上取，除非清除缓存，否则不会在执行这个模块了

```
{
  id: '...',
  exports: { ... },
  loaded: true,
  ...
}
```

出现循环加载时，会停下当前的脚本a，返回到上个脚本b执行，当这个脚本b执行完之后，才会回到a继续执行

#### nodejs中

`.mjs`文件总是以 ES6 模块加载，`.cjs`文件总是以 CommonJS 模块加载，`.js`文件的加载取决于`package.json`里面`type`字段的设置。