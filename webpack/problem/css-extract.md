# 10. 分离css插件

以css配置示例,less与sass同理

## 1. 使用旧版的ExtractTextPlugin插件
### 安装
```npm
npm install extract-text-webpack-plugin@next --save-dev
```

### 在webpack.config.js中配置
```js
const extractTextPlugin=require('extract-text-webpack-plugin')

module.exports={
    //...code
    module:{
       rules:[{
            test:/\.css$/,
            use:extractTextPlugin.extract({
                fallback:"style-loader",
                use:['css-loader'],
                publicPath:"../"
            })
        }]
    },
    plugins:[
        new extractTextPlugin("./css/[name].css")//输出路径
    ]
}
```
如果还使用了根据css自动加前缀loader
```js
const extractTextPlugin=require('extract-text-webpack-plugin')

module.exports={
    //...code
    module:{
       rules:[{
            test:/\.css$/,
            use:extractTextPlugin.extract({
                fallback: "style-loader",
                use: ['css-loader', {
                        loader: 'postcss-loader',
                        options: {
                            plugins: [require('autoprefixer')]
                        }
                    }]
                publicPath: '../'
            })
        }]
    },
    plugins:[
        new extractTextPlugin("./css/[name].css")//输出路径
    ]
}
```
### 使用方法
配置完成后运行webpack打包即可

---
## 2.使用新版mini-css-extract-plugin 插件

### 安装
```npm
npm install mini-css-extract-plugin --save-dev
```
### 在webpack.config.js中配置
```js
const miniCssPlugin=require('mini-css-extract-plugin');
module.exports={
    module:{
        rules:[
            {
                test:/\.css$/,
                use: [{
                    loader: miniCssPlugin.loader,
                    options: {
                        publicPath: '../'
                    }
                }, 'css-loader']
            }
        ]
    },
    plugins:[
        new miniCssPlugin({
            filename:'./css/[name].css'
        })
    ]
}

```