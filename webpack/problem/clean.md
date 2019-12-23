# 12. 清理旧打包文件插件
当我们修改带hash的文件并进行打包时，每打包一次就会生成一个新的文件，而旧的文件并 没有删除。为了解决这种情况，我们可以使用clean-webpack-plugin 在打包之前将文件先清除，之后再打包出最新的文件

## 安装
```npm
npm install clean-webpack-plugin --save-dev
```

## 配置
```js
const {CleanWebpackPlugin}=require('clean-webpack-plugin');

module.exports={
    plugins=[
        new CleanWebpackPlugin()
    ]
}
```

## 使用
配置完成,在打包时自动生效