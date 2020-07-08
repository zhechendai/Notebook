<!--
 * @Date: 2020-07-08 09:26:10
 * @Author: Dai Zhechen
 * @Github: https://github.com/zhechendai
 * @LastEditTime: 2020-07-08 15:09:00
 * @Copyright ©️ 2020 Dai Zhechen. All Rights Reserved.
--> 

循环控制
====

第三种循环
----

阶乘（使用while实现）

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/9.31.03.png)

引入for循环

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/9.31.16.png)

`for(循环初始条件；循环继续条件；循环内容每一轮要做的事)`
>`for(count=10; count>0;count--)`  
>三个表达式可以省去一个
>读成：对于一开始的count=10，当count大于0时，重复做循环体，每一轮循环在做完循环体内语句后，使得count--

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/10.07.58.png)

* for循环像一个计数器
* 设定一个计数器并初始化，在计数器达到某一值之前，重复执行循环体
* 每执行一轮循环，计数器进行步进调整，如+1或-1

>求积初始化为1，求和初始化为0

for循环可以改写成while
![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/10.08.55.png)

>for中的每一个表达式都能够省略，例如`for(;条件;)`==`while(条件)`  
>分号不可以省略

如何选择for/ do-while/ while
* 如果有固定次数，用for
* 如果必须执行一次，用do-while
* 其他情况用while


循环控制
----

eg. 判断是否为素数

* `break`跳出循环
* `continue`跳过循环这一轮剩下的语句进入下一轮
* `break`和`continue`只对所在的那一层循环一作用

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/11.00.09.png)

循环的嵌套

循环里面还是循环

eg. 100以内的素数

```c
#include <stdio.h>
int main()
{
    int x;
    for (x = 2; x < 100; x++)
    {
        int i;
        int isPrime = 1;  // is prime
        for ( i = 2; i < x; i++)
        {
            if (x % i == 0)
            {
                isPrime = 0;
                break;
            }    
        }
        if (isPrime == 1)
        {
            printf("%d ", x);
        }    
    }
    printf("\n");

    return 0;
}
```

修改：只输出前50个素数

```c
#include <stdio.h>
int main()
{
    int x;
    // x = 2;                          while 条件
    int cnt = 0;
    // for (x = 2; x < 100; x++)
    // while (cnt < 50)                while条件
    for (x = 2; cnt < 50; x++)       //while可以使用for来替代
    {
        int i;
        int isPrime = 1;  // is prime
        for ( i = 2; i < x; i++)
        {
            if (x % i == 0)
            {
                isPrime = 0;
                break;
            }    
        }
        if (isPrime == 1)
        {
            printf("%d ", x);
            cnt++;
        }
        // x++;                        while条件
    }
    printf("\n");

    return 0;
}
```

离开多重循环

eg. 凑硬币

```c
#include <stdio.h>
int main()
{
    int x;
    printf("please type your price:");
    scanf("%d", &x);
    int one, two, five;

    for ( one = 1; one < x*10; one++)
    {
        for ( two = 1; two < x*10/2; two++)
        {
            for ( five = 1; five < x*10/5; five++)
            {
                if (one + two*2 + five*5 == x*10)
                {
                    printf("可以用%d个一角和%d个两角和%d个五角得到%d元\n", one, two, five, x);
                }
                
            }
            
        }
        
    }
    

    return 0;
}
```
>此时发现输出了所有可能的结果，如果只需要找到一种情况？如下

利用break跳出多重循环（需要在多重下写break）
* `break`和`continue`只对所在的那一层循环一作用

```c
#include <stdio.h>
int main()
{
    int x;
    printf("please type your price:");
    scanf("%d", &x);
    int one, two, five;
    int exit = 0;

    for ( one = 1; one < x*10; one++)
    {
        for ( two = 1; two < x*10/2; two++)
        {
            for ( five = 1; five < x*10/5; five++)
            {
                if (one + two*2 + five*5 == x*10)
                {
                    exit = 1;
                    printf("可以用%d个一角和%d个两角和%d个五角得到%d元\n", one, two, five, x);
                    break;
                } 
            }
            if (exit == 1) break;
        }
        if (exit == 1) break;
    }
    
    return 0;
}
```

发现
* 要用多个break跳出从内到外一级一级for循环(还需要增添一个判断变量exit)，可以利用`goto`跳出循环，直接跳到指定的位置，如下

```c
#include <stdio.h>
int main()
{
    int x;
    printf("please type your price:");
    scanf("%d", &x);
    int one, two, five;
    int exit = 0;

    for ( one = 1; one < x*10; one++)
    {
        for ( two = 1; two < x*10/2; two++)
        {
            for ( five = 1; five < x*10/5; five++)
            {
                if (one + two*2 + five*5 == x*10)
                {
                    printf("可以用%d个一角和%d个两角和%d个五角得到%d元\n", one, two, five, x);
                    goto out; // out相当于一个标记符，可以用任何字符表示
                } 
            }
        }
    }
out: // 注意这里是冒号    goto跳到这个位置
    
    return 0;
}
```

>goto和指针一样，使用不当会使程序难以理解，但是在某些地方还是有用的，比如深层嵌套（以上例子）

循环应用
----

eg. 求和

式子为：
$$f(n)=1+\frac12+\frac13+\frac14+...+\frac1n$$

```c
int n;
int i;
double sum = 0.0;

scanf("%d", &n);
for(i=1; i<=n; i++){
    sum += 1.0/i;
}
printf("%f", sum)
```

如果式子为：
$$f(n)=1-\frac12+\frac13-\frac14+...+\frac1n$$
>需要添加一个变量来控制符号，如

```c
int n;
int i;
double sum = 0.0;
// int sign = 1;
double sign = 1.0; //这样可以不用乘1.0
scanf("%d", &n);
for(i=1; i<=n; i++){
    sum += sign/i;
    sign = -sign;
}
printf("%f\n", sum);
```

eg. 最大公约数

普通枚举法

```c
    int a;
    int b;
    scanf("%d %d", &a, &b);
    int min;
    if (a < b)
    {
        min = a;
    }else
    {
        min = b;
    }
    int i;
    int ret = 0;
    for ( i = 1; i < min; i++)
    {
        if (a%i == 0)
        {
            if (b%i == 0)
            {
                ret = i;
            }
        }    
    }
    printf("%d和%d的最大公约数是%d\n", a, b, ret);
```

辗转相除法

原理：
* 如果b等于0，计算结束，a就是最大公约数；
* 否则，计算a除以b的余数，让a等于b，而b等于那个余数；
* 回到第一步

```c
    int a;
    int b;
    scanf("%d %d", &a, &b);
    int t;
    while (b!=0)
    {
        t = a%b;
        a=b;
        b=t;
    }
    printf("gcd is %d\n", a);
```

eg. 整数分解

```c
#include <stdio.h>
int main()
{
    int x;                   // 这一部分先将整数逆序，防止接下来分解的时候再逆序
    int t = 0;
    scanf("%d", &x);

    do{
        int d = x%10;
        t = t*10+d;
        x /=10;
    } while (x>0);
    x = t;
    
    do{                      // 这一部分用来分解整数
        int d = x%10;
        printf("%d", d);
        if (x >9){           // 这个if语句用来解决输出最后一个空格的问题
            printf(" ");
        } 
        x /=10;
    } while (x>0);
    printf("\n");
    
    return 0;
}
```

发现
* 程序运行顺利
* 存在一个问题：输入例如700时，输出为7，而不是 0 0 7
* 表明该程序不适合末尾有0的数字

改进
* 通过除法来获得最高位，按从左到右
* 怎么通过确定几位数来确定mask是10的几次？使原数/10，每除一次mask*10（存在一个问题，这时mask的位数比原数多一位？使循环范围t>9，这样可以少进一次循环，mask少乘一次10，则位数与原数一样）
* 发现当个位数输入时多出现一个0，这是因为do-while循环，使得mask一定先进入循环乘10，需要换成while循环，这样当原数为个位时，mask为1 

```c
#include <stdio.h>
int main()
{
    int x;
    scanf("%d", &x);
    int t = x;
    int mask = 1;

    // do
    // {
    //     t/=10;
    //     mask *=10;
    // } while(t>9)；

    while(t>9)
    {
        t/=10;
        mask *=10;
    } 
    
    do{
        int d = x/mask;
        printf("%d", d);        // 将每一位除数结果输出
        if (mask >9){           // 这个if语句用来解决输出最后一个空格的问题
            printf(" ");
        } 
        x %=mask;
        mask /=10;

    } while (mask>0);
    printf("\n");
    
    return 0;
}
```

>上述问题中mask也可以表示成10的几次  
>* 指数计算可以使用`pow()`（类似week3_homework_2）  
>* 这时需要使用`#include <math.h>`  
>* pow是浮点运算，更慢，所以我们在上述问题中使用累乘的方法

