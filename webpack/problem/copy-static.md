# 13. 拷贝静态资源

处理不需要使用webpack统一打包处理或webpack不支持的文件

## 安装
```npm
npm install copy-webpack-plugin --save-dev
```

## 配置
```js
const copyWebpackPlugin=require('copy-webpack-plugin');

module.exports={
    plugins:[
        new copyWebpackPlugin([
        {
            from:__dirname+"/public/jquery.js",
            to:__dirname+"/build/jquery.js"
        },
        {
            from:__dirname+"/public/index.js",
            to:__dirname+"/build/index.js"
        }
        ])
    ]
}
```