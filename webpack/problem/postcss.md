# 5. 根据浏览器自动添加css前缀

### 1.安装  [postcss-loader](https://www.webpackjs.com/loaders/postcss-loader/) [autoprefixer](https://www.npmjs.com/package/autoprefixer)
```npm
npm install postcss-loader autoprefixer --save-dev 
```

### 2.配置webpack.config.js
以css示例,less与sass中配置类似
```js
module.exports={
    //...code
    module:{
        rules:[
            {
                //匹配规则
                test:/\.css$/,
                //loader加载顺序 不能颠倒
                use: ['style-loader', 'css-loader',{
                    loader:'postcss-loader',
                    options:{
                        plugins:[
                            require('autoprefixs')
                        ]
                    }
                }]
            }
        ]
    }
}
```

### 3. 修改package.json
增加如下内容
```json
    "browserslist": [
        "ie>=8",
        "Firefox>=20",
        "Safari>=5",
        "Android>=4",
        "Ios>=6",
        "last 4 version"
    ]
```

### 4. 使用方式
通过直接引入配置了postcss-loader的文件
```js
import "xxxx/xx.css"
```