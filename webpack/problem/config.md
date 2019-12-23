# 1. 文档学习记录

## 安装webpack4
```npm
① 本地安装
npm install -save-dev webpack
npm install -save-dev webpack-cli
② 全局安装
npm install -global webpack webpack-cli
```

## 配置文件
* wabpack.config.js (不指定文件时,默认使用)
* webpack.config.dev.js
  * 开发环境使用
* webpack.config.build.js
  * 生产环境使用
## npm 启动命令
```js
    "scripts": {
        "dev": "webpack-dev-server  --config=config/webpack.config.dev.js",
        "dev:dist": "webpack --mode development --config=config/webpack.config.dev.js",
        "build": "webpack --mode production  --config=config/webpack.config.build.js"
    },
```

## 开发环境配置文件
---
## mode (模式)
* <details>
  <summary>development   --  开发环境</summary>

    会将 DefinePlugin 中 process.env.NODE_ENV 的值  设置为 development。启用 NamedChunksPlugin 和     NamedModulesPlugin。
  </details> 
* <details>
  <summary>productiom    --  生产环境 **(默认值)**</summary>

   会将 DefinePlugin 中 process.env.NODE_ENV 的值设置为 production。启用 FlagDependencyUsagePlugin, FlagIncludedChunksPlugin, ModuleConcatenationPlugin, NoEmitOnErrorsPlugin, OccurrenceOrderPlugin, SideEffectsFlagPlugin 和 TerserPlugin。
  </details> 

* none -- 不设置可以在npm启动命令中添加参数
  * 退出任何默认优化选项
 
<details>
<summary>mode: development</summary>

```js
module.exports = {
+ mode: 'development'
- devtool: 'eval',
- cache: true,
- performance: {
-   hints: false
- },
- output: {
-   pathinfo: true
- },
- optimization: {
-   namedModules: true,
-   namedChunks: true,
-   nodeEnv: 'development',
-   flagIncludedChunks: false,
-   occurrenceOrder: false,
-   sideEffects: false,
-   usedExports: false,
-   concatenateModules: false,
-   splitChunks: {
-     hidePathInfo: false,
-     minSize: 10000,
-     maxAsyncRequests: Infinity,
-     maxInitialRequests: Infinity,
-   },
-   noEmitOnErrors: false,
-   checkWasmTypes: false,
-   minimize: false,
- },
- plugins: [
-   new webpack.NamedModulesPlugin(),
-   new webpack.NamedChunksPlugin(),
-   new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("development") }),
- ]
}
```
</details>

<details>
<summary>mode: production</summary>

```js
module.exports = {
+  mode: 'production',
- performance: {
-   hints: 'warning'
- },
- output: {
-   pathinfo: false
- },
- optimization: {
-   namedModules: false,
-   namedChunks: false,
-   nodeEnv: 'production',
-   flagIncludedChunks: true,
-   occurrenceOrder: true,
-   sideEffects: true,
-   usedExports: true,
-   concatenateModules: true,
-   splitChunks: {
-     hidePathInfo: true,
-     minSize: 30000,
-     maxAsyncRequests: 5,
-     maxInitialRequests: 3,
-   },
-   noEmitOnErrors: true,
-   checkWasmTypes: true,
-   minimize: true,
- },
- plugins: [
-   new TerserPlugin(/* ... */),
-   new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("production") }),
-   new webpack.optimize.ModuleConcatenationPlugin(),
-   new webpack.NoEmitOnErrorsPlugin()
- ]
}
```
</details>

<details>
<summary>mode: none</summary>

```js
module.exports = {
+ mode: 'none',
- performance: {
-  hints: false
- },
- optimization: {
-   flagIncludedChunks: false,
-   occurrenceOrder: false,
-   sideEffects: false,
-   usedExports: false,
-   concatenateModules: false,
-   splitChunks: {
-     hidePathInfo: false,
-     minSize: 10000,
-     maxAsyncRequests: Infinity,
-     maxInitialRequests: Infinity,
-   },
-   noEmitOnErrors: false,
-   checkWasmTypes: false,
-   minimize: false,
- },
- plugins: []
}9910
```
</details>

### 配置方法1
```js
module.exports={
    mode:"development"
}
```
### 方法2
```cli
webpack --mode=production
```
### 根据环境选择打包方式(配置文件)
```js
let config={
    entry:'./index.js',
    // ...code
}

module.exports=(env,argv)=>{
    let {mode}=argv;//获取当前的打包模式
    if(mode==='production'){
        config.devtool='source-map';
    }

    if(mode==='development'){
        config.devtool='xxx'
    }
    return config;
}
```
---
## entry (入口文件)
 默认  --  ./src/index.js(不指定的情况下)

### 单入口
```js
  module.exports={
      entry:'./src/index.js'
  }
```
等价于
```js
module.exports={
    entry:{
        main:'./src/index.js'
    }
}
```

### 参数为数组时
>向 entry 属性传入文件路径数组时，将创建出一个多主入口chunk。在需要要一次注入多个依赖文件，并且将它们的依赖导向到一个 chunk
```js
module.exports={
    entry:{
        main:['index.js','second.js']
    }
}
```

### 多入口(对象语法)
>entry:{[entryName:String]:String|Array<String\>}
```js
module.exports={
    entry:{
        index:'index.js',
        second:['s1.js','s2.js']
    }
}
```

### 多页面使用场景
推荐:一个HTML文档一个入口文件
```js
module.export={
    entry:{
        pageone:'./src/pageone/index.js',
        pagetwo:'./src/pagetwo/index.js'
    }
}
```

## output(输出配置)
>控制 webpack 如何向硬盘写入编译文件。即使存在多个 entry 起点，但只指定一个 output 配置。

### 基本用法
* filename:打包后的文件名   默认:main.js
* path:打包后的路径         默认:./dist/
* 


会在打包后的dist目录中生成bundle.js文件
```js
module.export={
    output={
        filename:'bundle.js'
    }
}
```

### 占位符
* [ext]：资源扩展名
* [name]：资源的基本名称
* [path]：资源相对于context的路径
* [hash]：内容的哈希值
### 多入口时
使用文件名做占位符
```js
import path=require('path');
module.export={
    entry:{
        index:'index.js',
        second:'second.js'
    },
    output:{
        filename:'[name].js',//使用占位符
        path:path.join(__dirname,'dist/js')
    }
}
```

### 高级用法
```js
module.export={
    output:{
        path:'/home/xxx/[hash]',
        publicPath:'http://img.cdn.xxx.com/dir/[hash]'
    }
}
```

### 动态设置
```js
__webpack_public_path=myRuntimePublicPath
```

---
## resolve(解析)
>配置模块如何解析。例如，当在 ES2015 中调用 import 'lodash'，resolve 选项能够对 webpack 查找 'lodash' 的方式去做修改（查看模块）。

### resolve.alias
创建 import或require别名

```js
module.exports={
    resolve:{
        alias:{
            utils:path.resolve(__dirname,'src/util/')
        }
    }
}
```
导入
```js
import jsonUtil from 'utils/jsonUtil'
```
### resolve.modules
解析时搜索的目录
```js
module.exports={
    resolve:{
        modules:[
            "node_modlues"
        ]
    }
}
```

## loader
### css-loader与style-loader
加载CSS
#### install
```npm
npm run i style-loader css-loader -D 
```
#### 使用
```js
module.exports={
    module:{
        rules:[
             {
                  test:/\.css$/,//正则表达式,以.css结尾的文件
                 use: ['style-loader','css-loader']//顺序不能颠倒
            }
        ]
    }
    
}
```
### 配置方法
#### 1.configuration
>module.rules 允许你在 webpack 配置中指定多个 loader。 这种方式是展示 loader 的一种简明方式，并且有助于使代码变得简洁和易于维护。同时让你对各个 loader 有个全局概览：
loader 从右到左地取值(evaluate)/执行(execute)。在下面的示例中，从 sass-loader 开始执行，然后继续执行 css-loader，最后以 style-loader 为结束。查看 loader 功能 了解有关 loader 顺序的更多信息。

* test: 用于标识出应该被对应的 loader 进行转换的某个或某些文件。
* use:  表示进行转换时，应该使用哪个 loader。
```js
module.exports={
    module:{
        rules:[
         {
            test:/\.css$/,
            use: [
                { loader: 'style-loader' },
                 {
                    loader: 'css-loader',
                    options: {
                        modules: true
                    }
             },
             { loader: 'sass-loader' }
            ]
          }
     ]
    }
    
}
```
#### [2. inline(内联)](https://webpack.docschina.org/concepts/loaders/#%E5%86%85%E8%81%94-inline-)
#### [3. cli(脚手架)](https://webpack.docschina.org/concepts/loaders/#cli)

## plugin(插件)

插件则可以用于执行范围更广的任务。包括：打包优化，资源管理，注入环境变量。
>想要使用一个插件，你只需要 require() 它，然后把它添加到 plugins 数组中。多数插件可以通过选项(option)自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 new 操作符来创建它的一个实例。

>[插件列表](https://webpack.docschina.org/plugins)
### 例子
#### html-webpack-plugin
```npm
npm i html-webpack-plugin -D
```

```js
// 打包html
const htmlWebpackPlugin = require('html-webpack-plugin');
module.exports={
    plugins: [
        new htmlWebpackPlugin({
            filename: 'index.html',
            minify: {
                removeAttributeQuotes: false,//去掉参数双引号
                removeComments: false, //去掉注释
                collapseWhitespace: false //去掉空白
            },
            chunks: ['base', 'index'], //引入对应的js
            template: 'src/index.html'
        })
    ]
}
```



## configuration(配置)

### 基本配置

```js
module.exports={
    mode:'development',
    entry:'./src/index.js',
    output:{
        path:path.join(__dirname,'dist'),
        filename:'index.js'
    }
}
```

### 常用详细配置(部分待完善))

```js
module.exports={
    mode:"development",//可选development|production|none
    entry:'./src/index.js',//String|Array|Object
    output:{//输出配置
        //string
        //所有输出文件的目标路径
        //必须是绝对路径
        path:path.resolve(__dirname,'dist'),
        //string
        //[name].js 用于多入口(entry)
        //[chunkhash].js    用于长效缓存
        filename:'main.js',
        //输出解析文件的目录
        publicPath:'/assets/',//string
        //在生成代码时，引入相关的模块、导出、请求等有帮助的路径信息。
        pathInfo:true,
        //附加分块的文件名模板
        chunkFilename:"[id].js",
    },
    module:{
        rules:[//模块规则
            {  
                //规范只在文件名和test中使用正则
                //include和exclude使用绝对路径
                //避免使用exclude
                test:/\.jsx$/,//匹配条件
                include:[//必须匹配
                    path.resolve(__dirname,'app')
                ],
                exclude:[//必须不匹配
                    path.resolve(_dirname,'app/demo-files')
                ],
                issuer:[test,include,exlude],//导入源
                loader: 'xxx-loader',
                options: {
                    name: '[hash].[ext]',
                    outputPath: './img',
                    esModule: false//解决html中出现export default
                }
            }
        ]
    },
    //解析模块请求的选项
    //不适用于loader的解析
    resolve:{
        //用于查找模块的目录
        modules:[
        "node_modules",
        path.resolve(__dirname,'app')
        ],
        //使用的扩展名
        extensions:['.js','jsx','json','.css'],
        //模块别名
        alias:{
            moduleName:"module/path/file",
        }
    }
}
```