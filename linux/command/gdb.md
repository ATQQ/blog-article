# 9. gdb调试器的基本用法

## gdb调试
>编译链接生成可执行程序，在程序编译链接的过程中注意加上 -g 选项。
```bash
gcc -g test.c -o main
```
gdb调试生成的可执行程序
```bash
gdb main
```
### gdb命令
* 显示源代码
  ```bash
  l
  # 或者
  list
  ```
  ![图片](http://img.cdn.sugarat.top/mdImg/MTU3NzkzMDU3MzQzMA==577930573430)
* 设置断点
  ```bash
  b lineNumber
  # 或者
  break lineNumber
  ```
  ![图片](http://img.cdn.sugarat.top/mdImg/MTU3NzkzMDY3Mjg0NQ==577930672845)
* 查看断点信息
  ```bash
  info break
  ```
  ![图片](http://img.cdn.sugarat.top/mdImg/MTU3NzkzMTE5MzcxMQ==577931193711)
* 执行程序
  ```bash
  r
  # 或者
  run
  ```
  ![图片](http://img.cdn.sugarat.top/mdImg/MTU3NzkzMTE2NDc1NA==577931164754)
* 单步执行（两种）
  ```bash
  n
  # 或者
  next
  ```
  ![图片](http://img.cdn.sugarat.top/mdImg/MTU3NzkzMTI3MzMxMg==577931273313)
* 继续运行
  ```bash
  c
  # 或者
  continue
  ```
  ![图片](http://img.cdn.sugarat.top/mdImg/MTU3NzkzMTUyMDE4MA==577931520180)

* 查看变量值
  ```bash
  p varName
  # 或者
  print varName
  ```
  ![图片](http://img.cdn.sugarat.top/mdImg/MTU3NzkzMDg4NDM4NQ==577930884385)
* 修改变量值
  ```bash
  set varName=newValue
  ```
  ![图片](http://img.cdn.sugarat.top/mdImg/MTU3NzkzMjQ4MjkwNg==577932482906)
* 查看函数调用堆栈情况
  ```bash
  bt 
  ```
  ![图片](http://img.cdn.sugarat.top/mdImg/MTU3NzkzMTgzNzg1Mg==577931837852)

* 监控指定变量的值
  ```bash
  watch varName
  ```
  ![图片](http://img.cdn.sugarat.top/mdImg/MTU3Nzk1ODkwNjI1Mg==577958906252)
* 查看寄存器值
  ```bash
  info registers
  ```
  ![图片](http://img.cdn.sugarat.top/mdImg/MTU3NzkzMjI3MDU3Mw==577932270573)
* 显示反汇编
  ```bash
  disassemble functionName
  ```
  ![图片](http://img.cdn.sugarat.top/mdImg/MTU3NzkzMjIwNjY0MA==577932206640)
* 运行程序，直到当前函数结束
  ```bash
  finish
  ```
* 退出gdb
  ```bash
  quit
  ```
