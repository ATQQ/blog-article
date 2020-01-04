# 8. gcc的基本用法

## 基本用法
>gcc [option] [filename]

### 单文件示例
将main.c编译成可执行文件main
```bash
gcc -o main main.c 
```
### 多文件示例
将m1.c m2.c m3.c 编译成可执行文件main
```bash
gcc m1.c m2.c m3.c -o main
```
## option参数说明
> -o out_filename
* 输出文件名称指定为out_filename
* 默认:源文件名a.out

> -g

在可执行文件中包含符号调试工具所必须得调试信息

> -I dirname

将dirname所代表的目录加入到程序**头文件**目录列表中

> -L driname

将dirname所代表的目录加入到程序**库文件**目录列表中

> -l libname

指定目标文件链接时需要的库文件名

> -v 

查看gcc版本号

> -On 

优化代码
* O0:对程序的编译、链接不进行优化
* O1:同时减小代码的长度和执行时间
* O2:包括-O的功能，以及指令调度等调整工作
* O3:包括-O2的功能，以及循环展开等工作

## 函数库
* 程序员的角度，函数库实际上就是头文件（.h）和库文件（.so或.a）的集合
* Linux下函数默认将头文件放在/usr/include/目录下，库文件放到/usr/lib目录下
* Windows下静态库是以lib为后缀名，动态库是以dll为后缀名
* Linux下静态库以.a为后缀名，动态库是以.so为后缀名

## 库文件链接方式
### 使用完整库文件名称
```bash
gcc -o main hello.o libmy.a
gcc -o main hello.o libmy.so
``` 

### 使用-l参数
优先使用动态库
```bash
gcc main.c -lmy -o main
```
带有--static使用静态库
```bash
# 连接到 libmy.a
gcc main.c -static -lmy -o main
```

### 指定库目录
```bash
gcc main.c –L /home/sugar/lib –lmy –o main
```
* 一条gcc语句可以包含多个-L参数
* 在编译目标文件时使用-L无效
* 标准库无需使用-L参数

## 创建静态链接库

1. 编写 mylib.h
   ```cpp
   void test();
   ```
2. 编写 mylib.c
   ```cpp
   #include<stdio.h>
   void test(){
       printf("hello world");
   }
   ```
3. 生成目标文件 mylib.o
   ```bash
   gcc -c mylib.c
   ```
   ![图片](http://img.cdn.sugarat.top/mdImg/MTU3Nzk1NTM2NDYwMQ==577955364601)

4. 归档生成 libmy.a
   * >格式: ar –rc lib[name].a libname.o 
    
    ```bash
    ar -rc libmy.a mylib.o
    ```
    ![图片](http://img.cdn.sugarat.top/mdImg/MTU3Nzk1NTYyNjE1Mw==577955626153)

5. 编写测试程序 main.c
   ```cpp
   #include<mylib.h>
   int main(){
       test();
       return 0;
   }
   ```

6. 编译生成 main.o
   ```bash
   gcc -c main.c -o main.o -I ./
   ```
   ![图片](http://img.cdn.sugarat.top/mdImg/MTU3Nzk1NjAwNjc5OQ==577956006799)

7. 最后一步链接生成可执行程序
   ```bash
   gcc -L ./ -lmy main.o -o main
   ```
   ![图片](http://img.cdn.sugarat.top/mdImg/MTU3Nzk1NjQ0MDQ4NQ==577956440485)

8. 执行生成的可执行文件main
   ```bash
   ./main
   ```
   ![图片](http://img.cdn.sugarat.top/mdImg/MTU3Nzk1NjUwNTgxOQ==577956505819)

9. 最终完成的目录结构
```text
.
├── libmy.a     最终生成的静态库
├── main        可执行程序
├── main.c      测试代码
├── main.o      测试代码的编译结果
├── mylib.c     静态库的定义代码
├── mylib.h     静态库的头文件
└── mylib.o     静态库的编译结果

0 directories, 7 files
```

## 使用上面示例创建动态链接库
```bash
gcc -fpic -shared -o libmy.so mylib.c
```
![图片](http://img.cdn.sugarat.top/mdImg/MTU3Nzk1NzU1ODAzMQ==577957558031)

### 链接动态库生成可执行程序
```bash
gcc -L ./ -lmy main.c -I ./ -o main2
```
![图片](http://img.cdn.sugarat.top/mdImg/MTU3Nzk1NzgxNTEwMQ==577957815101)

执行
```bash
./main2
```
如果报错
```error
./main2: error while loading shared libraries: libmy.so: cannot open shared object file: No such file or directory
```
* 原因:使用动态链接库的程序运行时，要做一下设置，否则应用程序会报找不到动态库的错误
* 解决方案:将生成的动态库拷贝至 **/lib** 目录中,再次运行即可
```bash
sudo cp libmy.so /lib/
```
![图片](http://img.cdn.sugarat.top/mdImg/MTU3Nzk1ODA4MDMxMQ==577958080311)