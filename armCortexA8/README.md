# 嵌入式系统给结构及原理
## 选择题
1. 一幅1024×768的彩色图像,其数据量在2.25 MB左右,若图像数据没有经过压缩处理,则图像中每像素是使用<code>24</code>二进制位表示的。

## 问答题
1. 形形色色的嵌入式系统默默无闻地工作在我们身边,为我们的生活增加了无穷的乐趣。请列举你所熟悉的5个嵌入式应用系统,并说明嵌入式系统同PC系统相比具有哪些特点?  
   * 手机,mp3,路由器,打印机,相机,冰箱,洗衣机等;
   * 特点
     * 功耗低,体积小,专用性强
     * 软件一般固化在存储器芯片中,以提高执行速度和系统可靠性
     * 操作系统和应用集成在一起
     * 软件代码质量高
     * 嵌入式系统开发需要专门的开发工具与环境
     * 系统内核小
     * 专用性强
     * 系统精简
     * 高实时性
     * 多任务的操作系统


2. Boot Loader在嵌入式系统中主要起什么作用?完成哪些主要的工作?

   * Boot Loader 是在操作系统内核运行之前运行的一段小程序。功能：通过这段小程序，我们可以初始化硬件设备、建立内存空间的映射表，从而将系统的软硬件环境带到一个合适的状态，以便为最终调用操作系统内 
   核准备好正确的环境。
   * 工作
     1. 硬件设备初始化
     2. 加载Bootloader 的stage2 准备ARM空间
     3. 拷贝Bootloader的stage2到RAM空间中
     4. 设置堆栈
     5. 跳转到stage2的C入口点
     6. stage2包括
        1. 初始化本阶段要使用的硬件设备
        2. 检测系统内存映射
        3. 将内核映像和根文件系统映像从flash设备上复制到RAM中
        4. 设置启动参数
        5. 调用启动内核
3. 在对接口编程中,我们常常会用到这样的语句:#define GPDOCON (*(volatile unsigned long *)0xE0200A0)
请问:volatile在此处的含义是什么?能否删除volatile,为什么?
   * 阻止编译器优化
   * 不能删除,即优化器在用到这个变量时必须每次都小心的重新读取这个变量的值,而不是使用保存在寄存器中的备份
   * 当计算机需要一个数值的时候，会先把内存中的值读取到寄存器，然后下次在使用该值的时候就直接读取寄存器中的值了。加上volatile之后，程序就会在每
   * 不能,这是为了防止某些原因硬件会改变其值。

4. 当异常产生,处理器进入一个异常程序,进入和退出异常时需要的操作有哪些?   
* 进入异常
   1. 把断点处下一指令的地址保存到R14寄存器中
   2. 把CPSR的值复制到SPSR中,以保存断电处的状态
   3. 根据异常模式,吧CPSR的模式位M[4:0]设置成对应的值
   4. 自动使PC指向相关的异常向量,从该向量地址读取一条指令进行执行
* 退出异常
  1. 将保存在R14 的值送回到PC中
  2. 将SPSR的值送回到CPSR中
  3. 对中断禁止标志进行清楚

## 其它
1、MRS指令
* 格式:MRS{条件}   通用寄存器，程序状态寄存器（CPSR或SPSR）
* 作用:将程序状态寄存器的内容传送到通用寄存器中

2、MSR指令
* 格式:MSR{条件}   程序状态寄存器（CPSR或SPSR）_<域>，操作数 



---
## 部分ARM指令

### mvn指令
与mov指令用法差不多，唯一的区别是：它赋值的时候，先按位取反

---
### str
**字**数据存储指令 (32位)

---
### strb
**字节**数据存储指令(8位)

---
### strh
**半字**存储指令(16位)

### ldr
**字**数据加载指令(32位)


## 程序设计题
1. 将256字节数据从源数据区0x8000复制到目标数据区0x8200，复制时，以1个字为单位进行。编写相应的ARM汇编程序。

```c
#include<stdio.h>
extern void asm_strcpy(const char *str,char *des);
int main(){
   char origin[10]="abcdef";
   char d[10];
   asm_strcpy(d,origin);
   return 0;
}
```

```ARM
AREA ASMFILE,CODE,READONLY
EXPORT ASM_STRCPY
 LOOP
   LDR R4,[R0],#1
   CMP R4,#0
   BEQ OVER
   STR R4,[R1],#1
   B LOOP
 OVER
   MOV PC,LR
END
```


1. 编写ARM汇编程序，实现1+2+3+.......+100，相加的和放到0x300000处。

```ARM
AREA EXAMPLE,CODE,READONLY
 SUM DCD 0
ENTRY CODE32
 LDR R0,#100
 LDR R1,#0
LOOP
 ADD R1,R1,R0
 SUBS R0,R0,#1
 BCC LOOP
 LDR R0,SUM
 STR R1,[R0]
END
```

```ARM
AREA EXAMPLE,CODE,READONLY
  SUM DCD 0X300000
 ENTRY
   LDR R0.#100
   LDR R1,#0
LOOP
   ADD R1,R1,R0
   SUBS R0,R0,#1
   BCC LOOP
   LDR R0,SUM
   STR R1,[R0]
END
```


1. 采用冒泡排序法对buf缓冲区中存放的10个字节数据进行从大到小排序并放回原处。
```arm
AREA example,CODE,READONLY
   ENTRY
start
   mov r11,#9
outer
   str r11,[sp]
   ldr r0,=buf
   ldr r12,[sp]
   cmp r12,#0
   beq L1
inner
   ldrb r1,[r0]
   ldrb r2,[r0,#1]
   cmp r1,r2
   swpccb r2,r2,[r0]
   strccb r1,[r0,#1]
   add r0,r0,#1
   subs r12,r12,#1
   bne inner
   subs r11,r11,#1
   bne outer
L1
   mov r0,#0x18
   ldr r1,=0x20026
   swi 0xAB
buf
   DCB 12,23,8,2,45,17,53,78,29,36
END
```
4. 利用ARM汇编与C语言的互相调用，编程实现：某电影评分网站的打分记录中，4个用户给5部电影打分（所打的分值只能是1、2、3、4以及5，若未打分，则用0表示）。请求出每部电影的平均分，结果保留2位小数。
```arm

```
5. 已知数组array中有50个字节数据，统计所有数据中二进制‘1’ 的个数，如果是奇数则R7为1，如果是偶数则R7为0。结果存放于R6中。

```arm
AREA EXAMPLE,CODE,READONLY
ENTRY
 LDR R1,=0X99999
 LDR R2,=0X99999
 LDR R0,=0X00
LOOP
 CMP R1,0
 BEQ END
 SUB R1,R1,#1
 AND R2,R2,R1
 MOV R1,R2

 ADD R0,#1
 B LOOP
END
```

```ARM
AREA EXAMPLE.CODE,READONLY
 ENTRY
   LDR R1,0X99999
   LDR R2,0X99999
   LDR R0,0X00
 LOOP 

```