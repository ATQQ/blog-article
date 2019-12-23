# 8. 引入jQuery

## 1. 本地文件引入
### 配置
```js
const webpack=require('webpack');

module.exports={
    resolve:{
        alias:{
            //绝对路径
            jQuery:path.resolve(__dirname,'src/js/libs/jquery.min.js')
        }
    },
    plugins:[
        new webpack.ProvidePlugin({
            $:"jQuery"
        })
    ]
}
```
### 使用
index.js
```js
$(document).ready(function() {
    console.log("success");
})
```

---
## 2. 通过npm引入
### 安装
```npm
npm install  jquery --save-dev
```

### 使用
index.js
```js
import $ from 'jquery'

$(document).ready(function() {
    console.log("success");
})
```