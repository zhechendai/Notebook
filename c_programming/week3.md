<!--
 * @Date: 2020-07-07 11:18:08
 * @Author: Dai Zhechen
 * @Github: https://github.com/zhechendai
 * @LastEditTime: 2020-07-07 14:51:07
 * @Copyright ©️ 2020 Dai Zhechen. All Rights Reserved.
--> 

循环
====

循环
----

eg. 数数几位数
* 程序要读入一个4位以下（含4位）的正整数。然后输出这个正整数的位数，如：
* 输入：352，输出：3

>程序写的是步骤，而不是关系

```c
int x;
int n = 1;
scanf("%d", &x);

if(x > 999){
    n = 4;
}else if(x > 99){
    n = 3;
}else if(x > 9){
    n = 2;
}

printf("%d\n", n);
```

* 判断大于时，从高处往下判断，就不用判断上限了
* 判断小于时，从低处往上判断，就不用判断下限了

？如果是任意正整数怎么判断位数

>让计算机去数数，从右边开始数一位划一位，通过`/10`的方法。如果使用if，则又会没完没了的写无数个 if语句。此时需要引入循环语句，如

```c
int x;
int n = 0;

scanf("%d", &x);

n ++;
x /= 10;
while(x > 0){
    n ++;
    x /=10;
}

printf("%d\n", n);
```

>计算机中int能表示的整数有限，无法表达超过十位数，2^31-1= 2 147 483 647

while循环

```c
// if 语句
if( x > 0){
    x /= 10;
    n++;
}
// while循环语句类似if语句
// 不同得是if语句是一次性的，但while语句会循环执行，直到条件不满足
while( x > 0){
    x /= 10; // 这一部分称之为循环体
    n++;     //
}
```

<img src="https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/while.png" width="200" height="400">

>需要避免死循环，并需要有能使条件不成的的情况存在

* 当条件满足时，不断重复循环体内的语句
* 循环执行之前判断是否继续循环，所以有可能循环一次也没有被执行
* 条件成立是循环继续的条件

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/12.52.20.png)

>右边代码无法判断0的位数  
>调试时可以在程序适当的地方插入`printf`来输出变量

do-while循环

```c
do
{
    <循环体语句>
}while(<循环条件>)； //必须要加入分号，没有相当于这句话没有结束
```

<img src="https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/dowhile.png" width="200" height="300">

do-while 和 while的区别
* do-while在循环结束的时候才来判断条件，也就是循环无论如何都会被执行一遍
* while先判断条件，当条件满足时，再执行循环，所以while可能一边都不会执行

循环应用
----

eg. 计算log函数

```c
int x;
int ret = 0;  //也可以 ret = -1

scanf("%d", &x);
int t = x;

while(x > 1){  // x > 0 这取决于你对这个问题的解决思路的不同
    x /=2;
    ret++;
}
printf("log2 of %d is %d.\n", t, ret);
```

eg. 计数循环

```c
int count = 100;
while (count >= 0){           // 这个循环需要执行几次             101次
    count--;                  // 循环停下来的时候有没有输出最后的0  有
    printf("%d\n", count);    // 循环结束后count的值是多少        -1
}
printf("launch\n";) 
```
> 如果要模拟运行一个很大次数的循环，可以模拟较少的循环次数，然后做出推断。

eg. 算平均数
![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/2.01.23.png)

```c
int number;
int sum = 0;
int count = 0;

scanf("%d", &number);
while(number != -1){
    sum +=number;
    count ++;
    scanf("%d", &number);
}
printf("the avarage is %f.\n", 1.0*sum/count);
```

eg. 猜数游戏

```c
#include <stdio.h>
#include <stdlib.h>  // 多的这两行是为了
#include <time.h>    // srand(time(0))和 int number = rand()

int main()
{
    srand(time(0));
    int number = rand()%100+1; //限制随机数范围是1-100
    int count = 0;
    int input = 0;

    printf("我已经想好了一个1到100之间的数");
    do{        // 在这里需要无论如何都进入循环，所以使用do-while比while更合适
        printf("请猜这个数：");
        scanf("%d", &input);
        count ++;
        if(input > number){
            printf("你猜的数大了");
        }else if(input < number){
            printf("你猜的数小了");
        }
    }while(input != number);

    printf("恭喜！你用了%d次就猜到了答案。\n", count);

    return 0;
}
```

eg. 整数逆序

```c
int x;
int digit;
int ret = 0;

scanf("%d", &x);
while(x > 0){
    digit = x%10;
    ret = ret*10 + digit;
    x /=10;
}

printf("%d\n", ret);  // 这种情况下输入700输出为7，如果要输出007？如下
```
```c
int x;
int digit;

scanf("%d", &x);
while(x > 0){
    digit = x%10;
    printf("%d\n",digit);  // 将每一次获得的个位按序打印
    x /=10;
}
```
