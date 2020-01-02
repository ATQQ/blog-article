# 3. 嵌入式软件技术复习
## 简述题(作业1)
嵌入式软件技术基础-作业1

> 1.查阅资料回答嵌入式操作系统有哪几类，并分别举例说明

分时系统
* Unix

顺序系统
* Dos

全能操作系统(Rich OS):
* linux
* Android
* IOS
* Symbian
  
实时操作系统
* VxWorks
* 嵌入式linux
* Windows CE
* FreeRTOS
* RT-Threads
* uC/OS-II
>2、查阅资料回答如下（嵌入式）处理器有什么特点？并分别举例说明

* 稳定性强
* 环境适应性强
* 体积小
* 集成功能较多
* 支持实时多任务
* 具有存储区保护功能
* 功耗低
  
举例:Power PC、ARM

>3、简述嵌入式系统开发调试方式有哪几种，如何针对这几种方式搭建开发环境？

* 模拟器方式
* 交叉调试
* 非交叉调试
* 在线仿真器方式
* 监控器方式
* 在线调试器方式
  
>4、常见的嵌入式系统开发方法有哪几种？并比较这几种开发方法的优缺点

* 面向硬件开发
  * 优点：可以对程序进行实时仿真和测试，可以直接针对硬件进行调试
  * 缺点：需要购买硬件调试工具（如ICE），调试时必需有目标系统
* 面向操作系统开发
  * 优点：不需要购买硬件调试工具，节省开发成本。最为重要的是，通过使用模拟器和仿真器，可以在没有目标系统的情况下完成相应的开发工作，便于多人同时进行开发工作 
  * 缺点：只能基于操作系统进行应用程序和驱动程序的开发
* 通过vmware虚拟机:对于低配置的PC不太友好
* 通过仿真软件:无法达到真机100%的效果
* 通过安装真机环境:对新手不友好,需要一定时间去熟悉

## linux下C开发(作业2)
>1.请在linux环境下编写一个程序，使用结构定义
```cpp 
struct student{
    int no; /* 学号 */
    char name[64]; /* 名称 */
    float  height; /* 身高 */
};
```
>要求创建3个学生定义，并给三个学生的三个成员都赋上值，写一个show函数依次把所有学生信息显示，要求用vi编辑,并用gcc在Linux下编译，测试通过。提示：创建一个工程student目录，将结构体类型、show()函数原型写到show.h，将show()定义写到show.c里，main()写到main.c 里，*.h文件放在include目录下，*.c 放到source目录下。

show.h
```cpp
struct student{
	int no;
	char name[64];
	float height;
};
void show(const struct student stu);
```
show.c
```cpp
#include<stdio.h>
#include"./show.h"
void show(const struct student stu){
	printf("%d--%s--%f\n",stu.no,stu.name,stu.height);
}
```
main.c
```cpp
#include<stdio.h>
#include"./show.h"
int main(){
	struct student stu={1,"小明",170.2};
	show(stu);
	return 0;
}
```

## gdb调试(实验1)
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

## gcc 编译生成动态库/静态库
### 1.静态库生成
```bash
# 生成hello.o文件
gcc -c -I ../include hello.c
# 生成镜态库libhello.a
ar -rcs libhello.a hello.o
# 调用静态库生成可执行文件main
gcc main.c -static -I ../include -L ../lib -lhello  -o main
```
### 2.动态库生成
```bash
# 生成动态库文件libhello.so
gcc hello.c -I../include -fPIC -shared -o libhello.so
# 调用动态库编译连接生成可执行程序main
gcc main.c -I ../include -L ../lib -lhello -o main
```

## makefile(作业3/实验2)
demo1目录结构
```text
├── include
│   └── hello.h
├── lib
│   ├── hello.c
│   ├── hello.o
│   ├── libhello.a
│   ├── libhello.so
│   └── Makefile
└── src
    ├── main
    ├── main.c
    └── Makefile
```
静态库生成Makefile
```MakeFile
libhello.a:hello.o
	ar -rcs libhello.a hello.o
hello.o:hello.c
	gcc -c -I../include hello.c
```
调用静态库makefile生成可执行文件Makefile
```MakeFile
main:main.c libhello.a
	 gcc main.c -static -I ../include -L ../lib -lhello  -o main 

libhello.a:
	cd ./../lib && make libhello.a
```

demo2目录结构
```text
├── hellofirst
│   ├── hellofirst.c
│   ├── hellofirst.h
│   ├── hellofirst.h.gch
│   ├── hellofirst.o
│   └── Makefile
├── hellosecond
│   ├── hellosecond.c
│   ├── hellosecond.h
│   └── Makefile
├── include
│   ├── hellofirst.h
│   └── hellosecond.h
├── lib
│   ├── libhellofirst.a
│   └── libhellosecond.so
├── main
├── Makefile
└── twohellos.c
```
静态库
```Makefile
libhellofirst.a:hello.o
	ar -rcs ../lib/libhellofirst.a *.o

hello.o:hellofirst.c
	gcc -c hellofirst.c
clean:
	rm *.o
```
动态库
```makefile
main:hellosecond.c
	gcc hellosecond.c -fPIC -shared -o ../lib/libhellosecond.so
```
根目录Makefile调用 子目录Makefile
```Makefile
main:twohellos.c libhellofirst.a libhellosecond.so
	gcc twohellos.c -L ./lib/  -lhellofirst -lhellosecond -o main
libhellofirst.a:
	cd hellofirst && make
libhellosecond.so:
	cd hellosecond && make
clean:
	rm ./hellofirst/*.o ./lib/*.a ./lib/*.so
```
## shell编程题目
>试编写一个Shell程序，该程序能接收用户从键盘输入的50个整数，然后求出其总和、最大值及最小值。

```bash
sum=0
max=0
min=0
index=0
read nums
for num in $nums
do 
    # 最值初始化
    if [ $index -eq 0 ]
    then
        max=$num
        min=$num
    else
        # 判断当前值是否最大值
        if [ $max -lt $num ]
        then
            max=$num
        fi
        # 判断当前值是否为最小值
        if [ $min -gt $num ]
        then
            min=$num
        fi
    fi
    # 求和
    sum=`expr $sum + $num`

    # index++
    index=`expr $index + 1`
done
# 输出结果
echo "max:$max"
echo "min:$min"
echo "sum:$sum"
```

>判断一个文件是不是字符设备文件，如果是则将其拷贝到/dev目录下

```bash
read filepath
if [ -c $filepath ]
then
    mv $filepath /dev
else
    echo "no"
fi
```

>实现自动在/home目录下添加50个账号的功能，账号名称为stud1至stud50

```bash
i=1
while [ $i -le 50 ]
do
    # echo "stud$i"
    useradd "stud$i"
    i=`expr $i + 1`
done
```

## 概念
>总线结构
* 哈佛结构
* 冯*诺依曼结构

>CISC&RISC
* 复杂指令集计算机(Complex Instruction Set Computer)
* 精简指令集计算机(Reduced Instruction Set Computer)

>大端存储与小端存储
0x12345678

* 大端存储:字或半字的高位对应内存地址中的低地址
* 小端存储:高位数对应高地址

>存储器管理单元MMU
* 将虚地址转换成物理地址。
* 对存储器访问权限的控制。

>交叉编译

把在宿主机(开发环境)上编写的高级语言程序编译成可以运行在目标机(生产环境))上的代码，即在宿主机上能够编译生成另一种CPU（嵌入式微处理器）上的二进制程序。 

>嵌入式系统的开发模式 
* 面向硬件
* 面向操作系统

>什么是嵌入式系统？嵌入式系统主要的特点是什么？

嵌入式系统是以应用为中心，以计算机技术为基础，软件硬件可裁减，适于应用系统对功能、可靠性、成本、体积、功耗严格要求的专用计算机系统。 

* 稳定性强
* 环境适应性强
* 体积小
* 集成功能较多
* 支持实时多任务
* 具有存储区保护功能
* 功耗低

>嵌入式处理器主要有哪几类？分别有什么特点？

* 嵌入式微处理器（MPU）：运算器、控制器
* 嵌入式微控制器（MCU）：片内ROM、RAM、总线、I/O口、计数器、看门狗、AD、DA、Flash 
* 数字信号处理器（DSP）：哈佛结构，适用于FFT变换、谱分析、数字滤波等操作，用于音频、视频处理
* 片上系统（SOC）：USB、GPRS、GPS、IEEE1394、蓝牙，可靠性强、开发时间短
---
* 嵌入式微处理器（MPU）：Am186/88、386EX
* 嵌入式微控制器（MCU）：8051、P51XA
* 数字信号处理器（DSP）：TMS320系列、DSP56200系列
* 片上系统（SOC）：M-core、CC2430

> BootLoader
 
* 在嵌入式系统中，没有像BIOS那样的固件程序，整个系统的加载启动完全由BootLoader完成。
* BootLoader是操作系统内核之前运行的一段小程序。
通过这段小程序，我们可以初始化硬件设备、建立内存空间的映射，从而将系统的软硬件环境设置成一个合适的状态，然后加载操作系统内核并运行。
* 特点
  * 严重地依赖于硬件而实现。
* 分类
  * 按照功能复杂程度(是否含有Monitor)划分；
  * 按照处理器体系结构划分

