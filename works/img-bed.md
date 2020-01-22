# 1. 简单的Web图床

一个[极简的Web图床应用]((https://picker.sugarat.top/)),支持复制粘贴与拖拽上传图片

## 1.开发缘由
日常使用Vs Code编写markdown笔记与博客文章时,在文章中插入图片时发现非常不便
* 使用本地文件编写相对路径---没法直接复制粘贴到其它地方
* 使用第三方的图床---需要登录账号(还是放到自己"口袋"里放心)
* vs code内置插件--- 诸多bug使用不方便
* 喜欢折腾(真实)

## 2.效果预览
抛弃一切花里胡哨的,满足日常使用
### 静图
![图片](http://img.cdn.sugarat.top/mdImg/MTU3OTcwMTM0NDQ1NA==579701344454)

### 动图
![图片](http://img.cdn.sugarat.top/mdImg/MTU3OTcwMTgzNjQwNA==579701836404)

### [点击体验一把](https://picker.sugarat.top/)

> 项目地址:[github](https://github.com/ATQQ/image-bed-qiniu) ,附有详细的食用指南,从0到1

## 3.手把手讲解代码核心部分
### (1)如何获取复制粘贴的图片?
首先创建一个textarea获取粘贴/拖拽的内容,img展示复制/拖拽的图片

```html
<!-- 用于粘贴或拖拽图片 -->
<textarea id="paste-panel"></textarea>
<!-- 用于展示粘贴/拖拽的图片 -->
<img id="img-panel" src="">
```
创建完毕后你可以看到的效果

![图片](http://img.cdn.sugarat.top/mdImg/MTU3OTcwMzA4MTkwOQ==579703081909)

接下来是书写js代码
```js
// 获取到对应的dom
let $textarea = document.getElementById('paste-panel');
let $img = document.getElementById('img-panel');

/**
 * 绑定粘贴事件
 **/
$textarea.addEventListener('paste', function(e) {
    // 组织触发默认的粘贴事件
    e.preventDefault();
    // 获取粘贴板中的内容
    let {
        items
    } = e.clipboardData;

    // 遍历获取到的items
    for (const item of items) {
        // 如果是文件对象且类型为图片
        if (item.kind === 'file' && item.type.includes('image')) {
            // 获取到文件对象
            let imgFile = item.getAsFile()
            // 将文件转成url
            let src = URL.createObjectURL(imgFile)
            // 展示生成的url
            $img.src = src;
            return;
        }
    }
})
```
效果

![图片](http://img.cdn.sugarat.top/mdImg/MTU3OTcwNTgwMjI2Nw==579705802267)

### (2)如何获取拖拽的图片?

基于刚才的html结构,让我们一起来书写js代码

```js
// 禁用默认的拖拽触发的内容(防止浏览器直接打开图片文件)
document.addEventListener('drop', function(e) {
    e.preventDefault()
})
document.addEventListener('dragover', function(e) {
    e.preventDefault()
})

/**
 * 监听文件拖拽相关事件
 **/
// 判断文件是否是拖拽进入了指定区域再释放
let drag = false;

// 拖拽进入
$textarea.addEventListener('dragenter', function(e) {
    drag = true;
})

// 拖拽在区域里移动
$textarea.addEventListener('dragover', function(e) {
    drag = true;
});

// 离开指定的区域
$textarea.addEventListener('dragleave', function(e) {
    drag = false;
})

// 在指定的区域释放
$textarea.addEventListener('drop', function(e) {
    if (drag) {
        // 获取拖拽的文件
        let {
            files
        } = e.dataTransfer;
        for (const file of files) {
            // 如果为图片文件则展示
            if (file.type.includes("image")) {
                // 将文件转成url
                let src = URL.createObjectURL(file)
                    // 展示生成的url
                $img.src = src;
                return;
            }
        }
    }
})
```
效果

![图片](http://img.cdn.sugarat.top/mdImg/MTU3OTcwNjUwNDQ3NQ==579706504475)

关键的两个问题就这样解决了

### (3) 如何上传到七牛云?
> 参考:[qiniu-JavaScript-sdk文档](https://developer.qiniu.com/kodo/sdk/1283/javascript)

书写js方法
```js
import * as qiniu from "qiniu-js";
let Domain = 'bind-host' // 七牛云对象存储空间绑定的域名
let observer = {
    next(res) {
        console.log(res);
        //上传进度
        let { percent } = res.total;
    },
    error(err) {
        console.log(err);
    },
    complete(res) {
        let { key } = res;
        // 完整的图片链接
        let completeUrl = `${Domain}/${key}`;
    }
}

/**
 * 文件上传
 * @param {Blob|File} file
 * @param {String} filename
 **/
function uploadFile(file, filename) {
    // 上传配置
    let putExtra = {
        fname: filename,// 文件名称
        params: {},
        mimeType: null // 文件类型(null:系统自动识别)
    };

    // 上传用户平凭据
    let token = 'xxxxx....';
    // config
    let config = {
        useCdnDomain: true,// 是否使用cdn加速
        region: qiniu.region.z0
        //选择上传域名区域；当为 null 或 undefined 时，
        //自动分析上传域名区域,我是选择的华东所以是z0
        }
    }
    // token 上传身份验证秘钥
    let observable = qiniu.upload(file, filename, token, putExtra, config)

    // 配置回调函数
    observable.subscribe(observer)
}

```

### (4) 如何生成用户上传凭据token?

>参考[qiniu-nodejs-sdk文档](https://developer.qiniu.com/kodo/sdk/1289/nodejs)

书写js
```js
const qiniu = require('qiniu')
const fs = require('fs');

// 七牛账号下的一对有效的Access Key和Secret Key
// 对象存储空间名称 bucket
let accessKey = 'xxxx',
    secretKey = 'xxx',
    bucket = 'xxxx';

//鉴权对象
let mac = new qiniu.auth.digest.Mac(accessKey, secretKey);

let options = {
    scope: bucket,
    expires: 60 * 60 * 24 * 7 //这里设置的7天,token过期时间(s(秒))
};

let putPolicy = new qiniu.rs.PutPolicy(options);
let uploadToken = putPolicy.uploadToken(mac);

// 将获取的token生成写入到token.txt中
fs.writeFileSync("./token.txt", uploadToken);
```
<details>
<summary>图片解释</summary>
1. 对象存储空间名称 bucket
<img src="http://img.cdn.sugarat.top/mdImg/MTU3Nzc2MjM3NDI3Mw==577762374273">
2.Access Key和Secret Key
<img src="http://img.cdn.sugarat.top/mdImg/MTU3Nzc2MjUwMzA3Ng==577762503076">
<img src="http://img.cdn.sugarat.top/mdImg/MTU3Nzc2MjU5ODU4NQ==577762598585">
</details> 

书写完成后运行token.js

```npm
node token.js
```

同级目录下生成token.txt文件,里面的内容即为所需的用户凭据

![图片](http://img.cdn.sugarat.top/mdImg/MTU3Nzc2MjcyMTM3MQ==577762721371)

### 综上
利用上述所提到的知识即可搭建出这个简易的基于七牛云的web图床

## 4. 最后附上参考资料链接与项目github地址
#### 参考文档
> [1. qiniu-JavaScript-sdk文档](https://developer.qiniu.com/kodo/sdk/1283/javascript)<br>
> [2. qiniu-nodejs-sdk文档](https://developer.qiniu.com/kodo/sdk/1289/nodejs)

#### 项目地址
> 我的[Github](https://github.com/ATQQ/image-bed-qiniu)<br>
> 项目地址:[https://github.com/ATQQ/image-bed-qiniu](https://github.com/ATQQ/image-bed-qiniu)<br>
> 体验地址:[https://picker.sugarat.top/](https://picker.sugarat.top/)