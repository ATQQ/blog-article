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