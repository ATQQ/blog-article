# 单例模式

## 箭头函数写法
```js
const singleton = function () { };
singleton.getInstance = (() => {
    let instance = null;
    return () => {
        if (!instance) {
            instance = new singleton();
        }
        return instance;
    }
})()
const s1 = singleton.getInstance();
const s2 = singleton.getInstance();

console.log(s1 === s2);//true
```

## function写法
```js
singleton.getInstance=(function(){
    let instance = null;
    return function(){
        if(!instance){
            instance = new singleton(); 
        }
        return instance;
    }
})();

const s1 = singleton.getInstance();
const s2 = singleton.getInstance();

console.log(s1===s2);
```

## class写法
```js
class Singleton{
    static getInstance(){
        if(!Singleton.instance){
            Singleton.instance = new Singleton();
        }
        return Singleton.instance;
    }
}

let s1 = Singleton.getInstance();
let s2 = Singleton.getInstance();
console.log(s1 === s2);//true
```

## 实例1
实现一个 Storage

**①Storage--class实现**
```js
class Storage {
    static getInstance() {
        if (!Storage.instance) {

            Storage.instance = new Storage();
        }

        return Storage.instance;
    }

    setItem(key, value) {
        return localStorage.setItem(key, value);
    }

    getItem(key) {
        return localStorage.getItem(key);
    }
}

//调用
let s1 = Storage.getInstance();
s1.setItem("a", 666);
let s2 = Storage.getInstance();
console.log(s2.getItem("a"));
```

**②闭包实现**
```js
// 基础的StorageBase类，把getItem和setItem方法放在它的原型链上
function storageBase() {
    storageBase.prototype.getItem = function(key) {
        return localStorage.getItem(key);
    }

    storageBase.prototype.setItem = function(key, value) {
        return localStorage.setItem(key, value);
    }
}

const Storage = (function() {
    let instance = null;
    return function() {
        if (!instance) {
            instance = new storageBase();
        }
        return instance;
    }
})()

let s3 = new Storage();
console.log(s3.getItem('a'));
```

## 实例2
实现一个全局唯一的Modal弹框

index.html
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        #my-modal {
            height: 200px;
            width: 200px;
            line-height: 200px;
            position: fixed;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            border: 1px solid black;
            text-align: center;
        }
    </style>
</head>

<body>
    <button id="open" type="button">打开</button>
    <button id="close" type="button">关闭</button>
</body>
<script src="./modal.js"></script>
<script>
    document.getElementById('open').addEventListener('click', function() {
        let modal = Modal.getInstance();
        modal.style.display = 'block';
    })

    document.getElementById('close').addEventListener('click', function() {
        let modal = Modal.getInstance();
        modal.style.display = 'none';
    })
</script>
</html>
```

modal.js
```js
class Modal {
    static getInstance() {
        if (!Modal.instance) {
            let modal = document.createElement('div');
            modal.innerHTML = '全局弹窗';
            Modal.instance = modal;
            modal.id = 'my-modal';
            modal.style.display = 'none';
            document.body.appendChild(modal);
        }
        return Modal.instance;
    }
}
```