# 11. babel简单使用

## 安装依赖
```npm
npm install babel-loader @babel/core @babel/preset-env --save-dev
```

## 配置
```js
module.exports={
    module:{
        rules:[
            {
                test: /\.js$/,
                exclude: '/node_modules/',//不在指定路径查找文件,
                use: [
                    {
                        loader: 'babel-loader',
                        options:{
                            presets: ["@babel/preset-env"]
                        }
                    }
                ]
            }
        ]
    }
}
```