# 9. 打包HTML文件

### 1. 安装 html-webpack-plugin 
```npm
npm install html-webpack-plugin --save-dev
```

### 2. 在webpack.config.js中配置
```js
const htmlWebpackPlugin=require('html-webpack-plugin')

module.exports={
    //...code
    plugins:[
        new htmlWebpackPlugin({
            template:"./src/index.html",
            filename:"index.html",//生成的文件名
            minify:{
                minimize:true,//是否打包为最小值
                removeAttrbuteQuotes:true,//去除引号
                removeComments:true,//去掉注释
                collapseWhitespace:true,//去掉空格
                minifyCss:true,//压缩css
                removeEmptyElements:false,//清理内容为空的元素
            },
            chunks: ['base', 'index'], //引入对应的js(对应(entry)中的入口文件)
            hash:true//引入产出的资源时加上哈希避免缓存
        })
    ]
}
```
### 3. 使用方法
直接启动webpack进行项目打包即可