# 7. 字体文件处理

### 1. 安装 file-loader 
```npm
npm install file-loader --save-dev
```

### 2. 在webpack.config.js中配置
```js
module.exports={
    //...code
    module:{
        rules:[
           { //字体文件
                test: /\.(eot|svg|ttf|woff|woff2)$/,
                loader: 'file-loader',
                options: {
                    name: "[name].[ext]",
                    outputPath: './fonts'
                }
            }
        ]
    }
}
```
### 3. 使用方法
直接启动webpack进行项目打包即可