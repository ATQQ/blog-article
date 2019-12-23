# 6. 图片文件处理

### 1. 安装 file-loader html-loader
```npm
npm install file-loader html-loader --save-dev
```
其中html-loader生效需配合   html-webpack-plugin (分离html插件)才能生效
### 2. 在webpack.config.js中配置
```js
module.exports={
    //...code
    module:{
        rules:[
            {   //处理样式表中引入的图片
                test: /\.(png|jpg|gif|jpeg)$/,
                loader: 'file-loader',
                options: {
                    name: '[hash].[ext]',
                    outputPath: './img',
                    esModule: false
                }
            },
            {
                //处理html内联的图片
                test: /\.(html)$/,
                loader: 'html-loader',
                options: {
                    attrs: ['img:src', 'img:data-src']
                }
            }
        ]
    }
}
```
### 3. 使用方法

index.css
```css
#bgImg {
    width: 10em;
    height: 10em;
    background: url("./../imgs/favicon.png");
}
```
index.js
```js
import "./xxx/css/index.css"
```