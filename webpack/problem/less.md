# 3. 加载less

### 1.安装    less-loader 与   less
```npm
npm install less-loader less --save-dev
```

### 2.配置webpack.config.js
```js
module.exports={
    //...code
    module:{
        rules:[
            {
                //匹配规则
                test:/\.less$/,
                //loader加载顺序 不能颠倒
                use: ['style-loader', 'css-loader','less-loader']
            }
        ]
    }
}
```

### 3. 使用方法

index.js
```js
import "./../less/index.less"
```