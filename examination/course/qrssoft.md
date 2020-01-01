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