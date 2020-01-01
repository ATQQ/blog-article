# 4. 嵌入式操作系统复习

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