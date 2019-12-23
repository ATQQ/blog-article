# 2. 加载CSS

### 1. 安装css-loader 与 style-loader
```npm
npm install style-loader css-loader --save-dev
```

### 2. 在webpack.config.js中配置
```js
module.exports={
    //...code
    module:{
        rules:[
            {
                //使用正则表达式,匹配以.css结尾的文件
                test:/\.css$/,
                //顺序不能颠倒
                use: ['style-loader','css-loader']
            }
        ]
    }
}
```
### 3. 使用方法

index.js
```js
import "./../css/index.css"
```