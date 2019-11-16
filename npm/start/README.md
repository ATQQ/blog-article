# 1. 入门篇

## 创建package.json
```
npm init
```

## 使用默认的
```npm
npm init -y 或
npm init -f
```

## 修改默认配置信息
```npm
npm config set init.author.email "2604395430@qq.com"
npm config set init.author.name "wangshijun"
npm config set init.author.url "http://github.com/wangshijun"
npm config set init.license "MIT"
npm config set init.version "0.1.0"
```

## npm run 运行命令
npm run command

command是scripts脚本中的key
```js
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  }
```
如果不带任何参数执行 npm run，它会列出可执行的所有命令

npm 在执行指定 script 之前会把 node_modules/.bin 加到环境变量 $PATH 的前面，这意味着任何内含可执行文件的 npm 依赖都可以在 npm script 中直接调用，换句话说，你不需要在 npm script 中加上可执行文件的完整路径


## 创建自定义 npm script
---
### demo
eslint代码风格检查工具
1. 用于测试代码
```js
const str = 'some value';

function fn(){
    console.log('some log');
}
```

2. 添加eslint依赖
```
npm install eslint -D
```

3. 初始化 eslint 配置
```npm
./node_modules/.bin/eslint --init
```
根据提示完成风格配置

4. 添加 eslint 命令

```js
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "eslint": "eslint *.js"
    }
```

5. 运行
```npm
npm run eslint
```
---
## 多个npm script 串行
使用<code>&&</code>分割多个指令
一个出错则终止后续命令执行
```js
 "scripts": {
        "eslint": "eslint *.js",
        "serial": "npm run eslint && node index.js && node second.js",
    }
```

## 多个npm并行执行
使用<code>&</code>分割多个指令
```js
"parallel": "npm run eslint & node index.js & node second.js",
```

## 多命令运行
1. 安装依赖
```npm
npm i npm-run-all -D
```
2. 指令

npm-run-all <code>command1</code> <code>command2</code>
```js
"all": "npm-run-all eslint serial parallel"
```
3. 并行执行多条同时运行的指令

npm-run-all <code>--parallel</code> <code>command1</code> <code>command2</code>  
```js
"allparallel": "npm-run-all --parallel eslint serial parallel",
```

## 参数传递
npm run command <code>--</code> paramContent
```js
"eslint":"eslint *.js",
"param": "eslint *.js --fix"
```

```npm
npm run eslint -- --fix
```
等价于
```npm
npm run param
```

## 调整日志输出
### 显示尽可能少的有用信息

使用 --loglevel silent，或者 --silent，或者更简单的 -s 来控制

```npm
npm run test -s
```

### 显示尽可能多的运行时状态
使用 --loglevel verbose，或者 --verbose，或者更简单的 -d 来控制
```npm
npm run test -d
```