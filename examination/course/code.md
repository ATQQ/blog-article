>进去临界区,离开临界区函数

```cpp
#include "windows.h"

CRITICAL_SECTION  _critical

/*初始化，最先调用的函数。没什么好说的，一般windows编程都有类似初始化的方法*/
InitializeCriticalSection(& _critical) 

/*释放资源，确定不使用_critical时调用，一般在程序退出的时候调用。如果以后还要用_critical，则要重新调用InitializeCriticalSection
*/
DeleteCriticalSection(& _critical) 

/*
把代码保护起来。调用此函数后，他以后的资源其他线程就不能访问了。
*/
EnterCriticalSection（& _critical）

/*
离开临界区，表示其他线程能够进来了。注意EnterCritical和LeaveCrticalSection必须是成对出现的!当然除非你是想故意死锁！
*/
LeaveCriticalSection(& _critical)
```

>优先级反转代码(编程必考)

```cpp
/**
* 创建并启动所有任务
*/
void TaskStartCreateTasks(void)
{
    INT8U i;

    for (i = 0; i < N_TASKS; i++) // Create N_TASKS identical tasks
    {
        TaskData[i] = i;
    }
    // Each task will pass its own id
    OSTaskCreate(Task0, (void *)&TaskData[0], &TaskStk[0][TASK_STK_SIZE - 1], 5);
    OSTaskCreate(Task1, (void *)&TaskData[1], &TaskStk[1][TASK_STK_SIZE - 1], 6);
    OSTaskCreate(Task2, (void *)&TaskData[2], &TaskStk[2][TASK_STK_SIZE - 1], 7);
}

/**
* 任务 TA0 
*/
void Task0(void *pdata)
{
    INT8U err;
    INT8U id;

    id = *(int *)pdata;
    for (;;)
    {

        printf("Task_%d waitting for an EVENT\n\r", id);
        OSTimeDly(200); /* Delay 200 clock tick */
        printf("Task_%d's EVENT CAME!\n\r", id);
        printf("Task_%d trying to GET MUTEX\n\r", id);
        OSSemPend(mutex, 0, &err); /* Acquire mutex */
        switch (err)
        {
        case OS_NO_ERR:
            printf("Task_%d GOT mutex.\n\r", id);
            break;
        default:
            printf("Task_%d CANNOT get mutex, then SUSPENDED.\n\r", id);
        }
        OSTimeDly(200); /* Delay 200 clock tick */
        printf("Task_%d RELEASE mutex\n\r", id);
        OSSemPost(mutex); /* Release mutex */
    }
}

/**
* TASK1
*/
void Task1(void *pdata)
{
    INT8U id;
    id = *(int *)pdata;
    for (;;)
    {
        printf("Task_%d waitting for an EVEBT\n\r", id);
        OSTimeDly(100); /* Delay 100 clock tick */
        printf("Task_%d's EVENT CAME!\n\r", id);
        OSTimeDly(100);
    }
}

/*
* Task2
*/
void Task2(void *pdata)
{
    INT8U err;
    INT8U id;
    id = *(int *)pdata;
    for (;;)
    {
        printf("Task_%d trying to GET MUTEX\n\r", id);
        OSSemPend(mutex, 0, &err); /* Acquire mutex */
        switch (err)
        {
        case OS_NO_ERR:
            printf("Task_%d GOT mutex.\n\r", id);
            OSTimeDly(200); /* Delay 100 clock tick */
            break;
        default:
            printf("Task_%d CANNOT get mutex, then SUSPENDED.\n\r", id);
            OSTimeDly(200); /* Delay 100 clock tick */
            break;
        }
        printf("Task_%d RELEASE mutex\n\r", id);
        OSSemPost(mutex); /* Release mutex */
    }
}
```
pdf31页实验指导书

>系统调用实现原理，串惨

* 概念:是操作系统提供给用户程序调用的一组特殊接口。用户程序可以通过这组特殊接口来获得操作系统内核提供的服务。
* 系统调用并非直接和程序员或系统管理员打交道，它仅仅是一个通过软中断机制向内核提交请求，获取内核服务的接口。
* Linux会有6个寄存器使用来传递这些参数：eax (存放系统调用号)、 ebx、ecx、edx、esi及edi来存放这些额外的参数（以字母递增的顺序）。


>信号与共享内存代码

### 信号
```cpp
#include <signal.h>
#include <stdio.h>
void ouch(int sig){
    printf("\nOUCH! - I got signal %d\n", sig);
    (void) signal(SIGINT, SIG_DFL);
}

int main(){
    (void) signal(SIGINT, ouch);
    while(1)  {
        printf("Hello World!\n");
        sleep(1);
    }
    return 0;
}
```

### 示例
server.c
```cpp
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
#include<sys/types.h>
#include<sys/ipc.h>
#include<sys/shm.h>
#include<errno.h>
#define SHMSZ 27

int main(){
    char c;
    int shmid;
    key_t key;
    char *shm,*s;

    key=5678;
    shmid=shmget(key,SHMSZ,0666|IPC_CREAT);
    if(shmid<0){
        printf("shm create error");
        exit(1);
    }
    shm=(char *)shmat(shmid,NULL,0);
    if(shm==(char *)-1){
        printf("shm at error");
        exit(1);
    }
    s=shm;
    for(c='a';c<'z';c++){
        *s++=c;
    }
    *s=NULL;
    while(*shm !='*'){
        sleep(1);
    }
    shmdt(shm);
    exit(0);
    return 0;
}
```

user.c
```cpp
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
#include<sys/types.h>
#include<sys/ipc.h>
#include<sys/shm.h>
#include<errno.h>
#define SHMSZ 27

int main(){
    char c;
    int shmid;
    key_t key;
    char *shm,*s;

    key=5678;
    shmid=shmget(key,SHMSZ,0666|IPC_CREAT);
    if(shmid<0){
        printf("shm create error");
        exit(1);
    }
    shm=(char *)shmat(shmid,NULL,0);
    if(shm==(char *)-1){
        printf("shm at error");
        exit(1);
    }
    for(s=shm;*s!=NULL;s++){
        putchar(*s);
    }
        putchar('\n');
    *s='*';
    shmdt(shm);
    exit(0);
    return 0;
}

```

## shell编程题
>1.数组排序

c++代码
```cpp
int nums[8]={1,2,1,3,4,2,5,6};
int length=8;
for(int i=0;i<length;i++){
    for(int j=i+1;j<length;j++){
        if(nums[j]<nums[i]){
            int t=nums[j];
            nums[j]=nums[i];
            nums[i]=t;
        }
    }
}
for(int i=0;i<length;i++){
    cout<<nums[i]<<" ";
}
```
shell代码
```bash
i=0    #外层循环计数
j=0+$i #内层循环计数
t=0    #用于两数字交换临时变量
nums=(1 2 1 3 4 2 5 6) # 用于测试的数组
length=${#nums[*]} # 数组长度

# 外层循环开始
while [ $i -lt $length ]
do
    
    # 内层循环开始
    j=$(($i + 1))
    while [ $j -lt $length ]
    do
        # 从小到大排序
        if [ ${nums[$j]} -lt ${nums[$i]} ]
        then
            t=${nums[$i]}           
            nums[$i]=${nums[$j]}
            nums[$j]=$t
        fi
        
        # j++
        j=$(($j + 1))
    done

    # i++
    i=$(($i + 1))
done

# 输出结果
i=0
while [ $i -lt $length ]
do
    echo -e "${nums[$i]} \c"
    # i++
    i=$(($i + 1))
done
```

>2.数组求和

```bash
nums=(3 2 3 4 5)
sum=0
for v in ${nums[@]}
do
    sum=$(($sum + $v))
done
echo $sum
```

>3.求最值
```bash
read -a nums
max=${nums[0]}
for v in ${nums[@]}
do
    if [ $max -lt $v ]
    then
        max=$v
    fi
done
echo $max
```

>4.水仙花数(100-999).水仙花数是指一个 3 位数，它的每个位上的数字的 3次幂之和等于它本身

c++代码
```cpp
int i=100;
while(i<=999){
    int sum=0;
    int temp=i;
    int k=0;
    while(temp!=0){
        k=temp%10;
        sum=sum+k*k*k;
        temp=temp/10;
    }
    if(sum==i){
        cout<<sum<<" ";
    }
    i++;
}
```
shell 代码
```bash
# @author sugar
# time  2020年 01月 01日 星期三 22:33:59 CST
# 水仙花数

i=100
# 外层循环遍历每个数字(100 - 999)
while [ $i -le 999 ]
do
    declare -i sum=0    #存放3个位数和的临时值
    declare -i temp=$i  #当前待判断的值
    declare -i k=0      #临时存放每个位数的值
    while [ temp -ne 0 ]
    do
        k=$(($temp % 10))
        temp=$(($temp/10))
        sum=$(($sum+$k*$k*$k))
    done
    # 如果相等即为水仙花数
    if [ $sum -eq $i ]
    then
        echo  -e "$sum \c"
    fi
    # i++
    i=$(($i + 1))
done
```