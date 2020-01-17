# webpack

- 入口(entry)
- 输出(output)
- loader
- 插件(plugins)
- 模式

```
//__dirname  执行的js文件的绝对路径

const path = require('path'); const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装

module.exports = { entry: './path/to/my/entry/file.js', output: { path: path.resolve(__dirname, 'dist'), filename: 'my-first-webpack.bundle.js' }, module: { rules: [ { test: /\.txt$/, use: 'raw-loader' } ] }, plugins: [ new HtmlWebpackPlugin({template: './src/index.html'}) ], mode: 'production' };
```



### 入口

```
entry: '路径'
entry: {
   app: '应用程序起始入口',
   vendors: '第三方库入口'
}
//属性名是随意的
```

### 输出

```
output:{
    filename: '编译之后的文件名',  //输出打包的文件，最后只要在html中引入它就可以了
    path: '存放路径'
    publickPath: ''  // publicPath 指定资源文件引用的目录
}

// 多入口
output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
```

### 模式

```
mode: development
// mode: production
```

### loader

loader 可以将文件从不同的语言（如 TypeScript）转换为 JavaScript，或将内联图像转换为 data URL

```
module: {
    rules: [
            {
                test: /\\.css$/,
                use: [
                    'style-loader',
                    'css-loader'
                ]
            }
        ]
  }
```

在 module 对象的 rules 属性中可以指定一系列的 loaders，每一个 loader 都必须包含 test 和 use 两个选工页。这段配置的意思是说，当 webpack 编译过程中遇到 require（）或 import 语句导入一个后缀 名为.css 的文件时， 先将它通过 css-loader 转换， 再通过 style-loader 转换，然后继续打包。 use 选 项的值可以是数组或字符串， 如果是数组， 它的编译顺序就是从后往前

css 是通过 JavaScript 动态创建＜style>标签来写入的，这意味着样式代码都已经编 译在了 main.js 文件里，但在实际业务中 ， 可能并不希望这样做， 因为项目大了样式会很多，都放 在 JS 里太占体积， 还不能做缓存。这时就要用到 webpack 最后一个重要的概念一一插件（Plugins）

### plugins

是一个有`[apply](<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply>)` 属性的 JavaScript 对象

```
const pluginName = 'ConsoleLogOnBuildWebpackPlugin';class ConsoleLogOnBuildWebpackPlugin {apply(compiler) {compiler.hooks.run.tap(pluginName, compilation =>{console.log("webpack 构建过程开始！");});}}

//往plugins传入插件的实例plugins: [new webpack.optimize.UglifyJsPlugin(),new HtmlWebpackPlugin({template: './src/index.html'})]
```

### 配置

- 通过 `require(...)` 导入其他文件
- 通过 `require(...)` 使用 npm 的工具函数
- 使用 JavaScript 控制流表达式，例如 `?:` 操作符
- 对常用值使用常量或变量
- 编写并执行函数来生成部分配置

**导出一个对象的形式**

```
module.exports = {
    
}
```

**导出一个函数**

```
//env为环境对象//argv对象描述webpack选项module.exports = function(env, argv) {return {mode: env.production ? 'production' : 'development',devtool: env.production ? 'source-maps' : 'eval',plugins: [new webpack.optimize.UglifyJsPlugin({compress: argv['optimize-minimize'] // 只有传入 -p 或 --optimize-minimize})]};
```

**导出一个promise**

```
module.exports = () => {return new Promise((resolve, reject) => {setTimeout(() => {resolve({entry: './app.js',/* ... */});}, 5000);});};
```

**导出一个数组包含多个配置对象**

```
module.exports = [{output: {filename: './dist-amd.js',libraryTarget: 'amd'},entry: './app.js',mode: 'production',}, {output: {filename: './dist-commonjs.js',libraryTarget: 'commonjs'},entry: './app.js',mode: 'production',}];
```

### 模块解析

webpack使用enhance-resolve这个包来解析文件路径

- 绝对路径无需再解析

  import '/home/me/file';import 'C:\\Users\\me\\file';

- 相对路径，需要进行解析为绝对路径

  import '../src/file1';import './file2';

**可通过配置webpack中的resolve来进行路径解析**

[Untitled](https://www.notion.so/7a18b61e78ff46a0853a9beeb65d155a)

**resolveLoader用于解析webpack的loader包**

选项与resolve相同

```
resolveLoader: {
    moduleExtensions: [ '-loader' ]
  }
```

上述实现了`example-loader`省略了 `-loader`，也就是说只使用 `example`，则可以使用此选项来实现

### webpack构建

有三种代码类型

1. 你或你的团队编写的源码。
2. 你的源码会依赖的任何第三方的 library 或 “vendor” 代码。
3. webpack 的 runtime 和 *manifest*，管理所有模块的交互。

runtime 包含：在模块交互时，连接模块所需的加载和解析逻辑。包括浏览器中的已加载模块的连接，以及懒加载模块的执行逻辑。

Manifest是会保留所有模块的详细要点的数据集合。

### target

webpack 能够为多种环境或 *target* 构建编译

如

```
target: 'node' //编译为类 Node.js 环境可用（使用 Node.js require 加载 chunk）
target: 'web'  //编译为类浏览器环境里可用（默认）
```

### webpack-dev-server

热更新功能通过建立一个websocket连接来实时响应代码修改

### 常见插件

`[clean-webpack-plugin](<https://www.npmjs.com/package/clean-webpack-plugin>)` 用于在每次构建时清理dist文件夹

`[HtmlWebpackPlugin](<https://webpack.docschina.org/plugins/html-webpack-plugin>)`用于生成新的index.html文件替换旧的

`extract-text-webpack-plugin`将散落的某种文件如css集中起来生成一个main.css文件

## webpack运行流程

Webpack 的运行流程是一个串行的过程，从启动到结束会依次执行以下流程。 • 初始化参数： 从配置文件和 Shell 语句中读取与合并参数，得出最终的参数。在这个过程中还会执行配置 文件中的插件实例化语句 new Plugin() • 开始编译：用上一步得到的参数初始化 Compiler 对象，Compiler 负责文件监听和启动编译。在 Compiler 实例中包含了完整的 Webpack 配置，全局只有一个 Compiler 实例,加载所有配置的插件，依次调用插件的 apply 方法，让插件可以监听后续的所有事件节点．同时向插件传入 compiler 实例的引用，以方便插件通过 compiler 调用 Webpack 提供的 AP! 通 过执行对象的 run 方法开始执行编译。 • 确定入口 ： 根据配置中的 entry 找出所有入口文件。

•编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该 模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理。 • 完成模块编译： 在经过第 4 步使用 Loader 翻译完所有模块后， 得到了每个模块被 翻译后的最终内容及它们之间的依赖关系。 • 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk, 再将每个 Chunk 转换成一个单独的文件加入输出列表中，这是可以修改输出内容 的最后机会。 • 输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，将文件的内 容写入文件系统中。

在以上过程中， Webpack 会在特定的时间点广播特定的事件，插件在监听到感兴趣的 事件后会执行特定的逻辑，井且插件可以调用 Webpack 提供的 API 改变 Webpack 的运行 结果。

## 自定义webpack相关

### 1. 命令相关

通过向 npm run build 命令和你的参数之间添加两个中横线，可以将自定义参数传递给 webpack

例如：npm run build -- --colors。

### 2. loader的配置使用

```
module: {
    // 关于模块配置
    rules: [
      // 模块规则（配置 loader、解析器等选项）
      {
        test: /\\.jsx?$/,
        include: [
          path.resolve(__dirname, "app")   //这些路径下的js需要被该loader处理
        ],
        exclude: [
          path.resolve(__dirname, "app/demo-files")  //这些路径下的js不需要被该loader处理
        ],
        // 这里是匹配条件，每个选项都接收一个正则表达式或字符串
        // test 和 include 具有相同的作用，都是必须匹配选项
        // exclude 是必不匹配选项（优先于 test 和 include）
        // 最佳实践：
        // - 只在 test 和 文件名匹配 中使用正则表达式
        // - 在 include 和 exclude 中使用绝对路径数组
        // - 尽量避免 exclude，更倾向于使用 include
        issuer: { test, include, exclude },
        // issuer 条件（导入源）
        enforce: "pre",
        enforce: "post",
        // 标识应用这些规则，即使规则覆盖（高级选项）
        loader: "babel-loader",
        // 应该应用的 loader，它相对上下文解析
        // 为了更清晰，`-loader` 后缀在 webpack 2 中不再是可选的
        // 查看 webpack 1 升级指南。
        options: {
          presets: ["es2015"]
        },
        // loader 的可选项
      }
	]
}
```

### 3. 自定义loader

一个 Loader 的职责是单一的， 只需要完成一种转换。 如果一 个源文件需要经历多步转换才能正常使用，就通过多个 Loader 去转换。 在调用多个 Loader 去转换一个文件时，每个 Loader 都会链式地顺序执行。第 1 个 Loader 将会拿到需处理的原 内容，上一个 Loader 处理后的结果会被传给下一个 Loader 接着处理，最后的 Loader 将处理 后的最终结果返回给 Webpack。 所以，在开发一个 Loader 时，请保持其职责的单一性，我们只需关心输入和输出 。

Webpack 是运行在 Node. 上的，一个 Loader 其实就是一个 Node.js 模块，这个模块需 要导出一个函数。这个导出的函数的工作就是获得处理前的原内容，对原内容执行处理后， 返回处理后的内容。

由于 Loader 运行在 Node.js 中，所以我们可以调用任意 Node.js 自带的 API，或者安装 第三方模块进行调用

使用loader:

- 对于npm包，直接使用名称, webpack会到node_modules目录下查找
- 对于本地编写的loader，可以通过path.resolve()来引入使用
- npm link的方式

> Npm link 专门用于开发和调试本地的 Npm 模块，能做到在不发布模块的情况下， 将本 地的一个正在开发的模块的源码链接到项目的 node modules 目录下，让项目可以直接使 用本地的 Npm 模块。由于是通过软链接的方式实现的，编辑了本地的 Npm 模块的代码，所 以在项目中也能使用到编辑后的代码。 完成 Npm link 的步骤如下： • 确保正在开发的本地 Npm 模块（也就是正在开发的 Loader） 的 package. json 己 经正确配置好： • 在本地的 Npm 模块根目录下执行 npm link，将本地模块注册到全局： • 在项目根目录下执行 npm link loader-name，将第 2 步注册到全局的本地 Npm 模块链接到项目的 node moduels 下，其中的 loader-name 是指在第 1 步的 package.json 文件中配置的模块名称。

- resolveLoader

webpack增加配置

表示Webpack 会先去 node_modules 项目下寻找 Loader，如果找不到， 则再去build目录下寻找。

```
resolveLoader: {
      modules: ['node_modules', 'build/']
  }
```

可以在loader中通过loaderUtils来获取传给loader的配置

```
//webpack配置
module: {
      rules: [
          {
              test: /\\.js$/,
              use: [
                {
                    loader: path.resolve(__dirname, 'build/simpleLoader.js'),
                    options: {
                        args: 'yhx'
                    }
                }
              ]
          }
      ]
  }
//simpleLoader
const loaderUtils = require('loader-utils')
module.exports = function(source) {
    const options = loaderUtils.getOptions(this)
    console.log(options)
    return source
}
```

如果loader中需要返回原内容之外的东西，则使用this.callback与webpack进行通信

```
module. exports = function (source) { 
//通过 this .callback 告诉 Webpack 返回的结果 
this.callback(null, source, sourceMaps); 
//当我们使用 this.callback 返回内容时，该 Loader 必须返回 undefined, 
//以让 Webpack 知道该 Loader 返回的结果在 this.callback 中，而不是 return 中 return; 
//其中的 this.callback 是 Webpack 向 Loader 注入的 API，以方便 Loader 和 Webpack 之间通信。 //this.callback 的详细使用方法如下：
this.callback( 
//当无法转换原 内容时，为 Webpack 返回一个 Error 
err: Error | null,
//原内容转换后的内容
 content: string |  Buffer, 
//用于通过转换后的内容得出原内容的 Source Map，以方便调试 
sourceMap?: SourceMap, 
//如果本次转换为原内容生成了 AST 语法树，则可以将这个 AST 返回，以方便之后需要 AST 的 Loader 复用该 AST，避免重复生成 AST，提升性能 
abstractSyntaxTree?: AST
);
```

异步loader

```
module.exports = function(source) {
    const callback = this.async();
    setTimeout(function() {
        console.log('等待')
        callback(null, source)
    }, 3000)
}
```

loader获取二进制数据

```
module.exports = function(source) {
    console.log('二进制', source)
    return source
}
module.exports.raw = true //通过 exports.raw 属性告诉 Webpack 该 Loader 是否需要二进制数据
```

可通过this.cacheable(false)让webpack不缓存某个loader处理后的结果,默认是缓存的

在某些情况下，有些转换操作需要大量的计算，非常耗时，如果每次构建都重新执行重 复的转换操作，则构建将会变得非常缓慢。为此， Webpack 会默认缓存所有 Loader 的处理 结果，也就是说在需要被处理的文件或者其依赖的文件没有发生变化时，是不会重新调用对 应的 Loader 去执行转换操作的。

loader的其他API,详见



### 4. 自定义plugin

Webpack 通过 Plugin 机制让其更灵活，以适应各种应用场景。在 Webpack 运行的生命 周期中会广播许多事件， Plugin 可以监昕这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。

**plugin工作机制**

Webpack 启动后，在读取配置的过程中会先执行 实例化插件数组中的插件，初 始化一个 插件并获得其实例。在初始化 compiler 对象后，再调用 插件实例的apply (compiler ）为插件实例传入 compiler 对象。插件实例在获取到 compiler 对象后， 就可以通过 compiler. plug in （事件名称，回调函数）监听到 Webpack 广播的事件， 并且可以通过 compiler 对象去操作 Webpack。

**关于compiler和compilation**

- Compiler 对象包含了 Webpack 环境的所有配置信息，包含 options、 loaders、 plugins 等信息。这个对象在 Webpack 启动时被实例化，它是全局唯一的，可以简单地将 它理解为 Webpack 实例
- Compilation对象包含了当前的模块资源、编译生成资源、变化的文件等。当 Webpack 以开发模式运行时，每当检测到一个文件发生变化，便有一次新的 Compilation 被 创建。 Compilation 对象也提供了很多事件回调供插件进行扩展。通过 Compilation也能读取到 Compiler 对象。

Compiler 和 Compilation 的区别在于： Compiler代表了整个 Webpack 从启动到关闭的生 命周期，而 Compilation 只代表一次新的编译。

它们是 Plugin 和 Webpack 之间的桥梁

Webpack 在运行的过程中会广播事件，插件只需要监听它所关心的事件，就能加入这条生产 线中，去改变生产线的运作。 Webpack 的事件流机制保证了插件的有序性，使得整个系统的 扩展性良好。

我们自己可以自定义事件

```
/** 广播事件 食 event-name 为事件名称，注意不要和现有的事件重名 * params 为附带的参数 */
compiler. apply ('event-name',params); 
/** 监昕名称为 event-name 的事件，当 event-name 事件发生时，函数就会被执行。 ＊同时函数中的 params 参数为广播事件时附带的参数。 */ 
compiler.plugin ('event-name', function (params) { };

//webpack现有事件
compiler.plugin('emit', function(compilation, callback){
//处理完毕后执行 callback 以通知 Webpack 
//如果不执行 callback，运行流程将会一直卡在这里而不往后执行	
	callback();
})
```

**Plugin事件**

- emit

emit 事件发生时，代表源文件的转换和组装己经完成，在这里可以读取到最终将输出 的资源、代码块、模块及其依赖，并且可以修改输出资源的内容，emit 事件是修改 Webpack 输出资源的最后时机。

**常用API**

- 读取输出资源，代码块，模块及依赖

  class Plugin { apply(complier) { compiler. plugin ('emit', function (compilation, callback) { // compilation.chunks 存放所有代码块，是一个数组 compilation.chunks.forEach(function (chunk) { //chunk 代表一个代码块 //代码块由多个模块组成，通过 chunk.forEachModule 能读取代码块的每个模块 chunk.forEachModule(function (module) { // module 代表一个模块 [//module.fileDependencies](http://module.fileDependencies) 存放当前模块的所有依赖的文件路径，是一个数组 module.fileDependencies.forEach (function (filepath) { }); }); //Webpack 会根据 Chunk 生成输出的文件资源，每个 Chunk 都对应一个及以上的输出文件 //例如在 Chunk 中包含 css 模块并且使用了 ExtractTextPlugin 时，该 Chunk 就会生成 . j s 和 . css 两个文件 chunk.files.forEach (function (filename) { [//compilation.assets](http://compilation.assets) 存放当前即将输出的所有资源 //调用一个输出资源的 source （）方法能获取输出资源的内容 let source = compilation.assets[filename] .source( ); }) ; }); //这是一个异步事件，要记得调用 callback 来通知 Webpack 本次事件监听处理结束 ／／ 如果忘记了调用 callback，则 Webpack 将一直卡在这里而不会往后执行 callback() } }

- 监听文件的变化

在开发插件时经常需要知道是哪个文件发生的变化导致了新的 Compilation，为此可以使用 如下代码：当依赖的文件发生变化时会触发 watch-run 事件

```
compiler.plugin ('watch-run',(watching, callback) => { 
//获取发生变化的文件列表 
	const changedFiles = watching.compiler.watchFileSystem.watcher.mtimes; 
//changedFIles 格式为键值对，键为发生变化的文件路径。 
	if (changedFiles[filePath]) ! == undefined) { 
		//filePath 对应的文件发生了变化
	}
	callback () ; 
});
```

- 修改输出资源

在emit事件中进行修改

- 判断 Webpack 使用了哪些插件

可通过compiler.options.plugins来获取