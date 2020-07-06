<!--
 * @Date: 2020-07-06 17:46:06
 * @Author: Dai Zhechen
 * @Github: https://github.com/zhechendai
 * @LastEditTime: 2020-07-07 01:33:06
 * @Copyright ©️ 2020 Dai Zhechen. All Rights Reserved.
--> 

判断
====

判断
----

在time interval程序中我们之前使用全部化成分钟来计算，为了避免出现负数。如果使用小时分钟同时计算，出现负数时我们可以使用判断，来将负数转变成正数，同时小时数减1。

示例程序：

```c
#include <stdio.h>
int main()
{
    int hour1, minute1;
    int hour2, minute2;

    scanf("%d %d %d %d", &hour1, &minute1, &hour2, &minute2);

    int ih = hour2-hour1;
    int im = minute2-minute1;

    if(im < 0){
        im = 60+im;
        ih --;
    }

    printf("The time interval is %d hour %d minutes.\n", ih, im);

    return 0;
}
```

如果

```c
if(条件成立){
    ...
}
```

判断的条件

* `if()`括号中填写得是运算，用来计算两个值之间的关系，所以叫做关系运算

关系运算符

>`=`表示赋值，`==`表示相等关系

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_24183D31AA24-1.jpeg)

关系运算的结果
* 当两个值的关系符合关系运算符的预期时，关系运算的结果为整数1，否则为0。（有且只有这两个值），例如

```c
printf("%d\n", 5==3);
printf("%d\n", 5>3);
printf("%d\n", 5<=3);

//其输出结果为
// 0 不成立
// 1 成立
// 0 不成立
```

优先级
*  所有的运算符的优先级比算数的低，但是比赋值运算的高（比赋值低，就无法做赋值）
```c
7 >= 3+4;
// 返回1，先计算3+4
int r = a>0;
```
* 判断是否相等的==和!=的优先级又比其他低，连续的关系运算是从左到右进行的。

eg. 找零计算器
* 见GitHub

否则`else`
![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_2F972955BF55-1.jpeg)

if语句
* `if()` `else()`后可以不加`{}`,这时，紧跟在`)`后缩进的语句为条件成立时的执行语句
* `if()`语句一行结束的时候没有表示结束的`;`一整个语句块称为if语句，例如

```c
if(total > amount)
    total += amount+10;
// 这一整个才是完整的if语句
```

为什么强调if和else后面要用{}
* 不加只能只有一条有效语句，加了可以有多条有效语句。要修改时也很方便
* 加上{}可以让人更明确if条件成立时应该执行哪条或者哪几条语句，以及不成立的时候执行else后面的哪些语句。

错题整理

* ？以下语句是否可以通过编译：`if ( 1<=n<=10 );`，是 
* 以下语句是否表示n属于[1,10]：`if ( 1<=n<=10 );` 否，n是一个输入量


分支
----

嵌套的if-else
* 当if的条件满足或者不满足的时候要执行的语句也可以是一条if或者if-else语句，这就是嵌套的if语句
![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_3C4805032638-1.jpeg)

else的匹配
* else总是和最近的那个if匹配

注意点
* 在if或者else后面总是用{}
* 即使只有一条语句的时候

级联的if-else if
* eg. 分段函数

```c
if(x < 0){
    f = -1;
}else if(x == 0){
    f = 0;
}else{
    f = 2*x;
}
```

<img src="https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_2E25A739F215-1.jpeg" width="300" height="250"/>

级联的if-else if

```c
if(exp1)
    stl;
else if(exp2)
    st2;
else
    st3;
// 可以添加多级 else if
```

单一出口
* 代码使用的重复性
* 输出可控

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_24AFF562F057-1.jpeg)

if语句常见错误
* 忘了带括号{}
* if后面的分号：不进入if内的内容
* 错误使用==和=：if（）内只读两个值0和非0，手动输入1，如`if(1)`则会进入if语句
* 使人困惑的else

多路分支
* switch-case
![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_F090C43E9F9E-1.jpeg)
>switch-case能直接找到条件对应的是哪一个case，而不用像if-else一样一级一级往下找

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_11E6E7E4FF67-1.jpeg)

switch语句可以看作是一种基于计算的表达。case是入口，如果没有break，会进入下一个case直到遇到break为止，或者switch结束为止。

问题
----

if语句中如果条件为闭区间如何输入？

```c
// 第一种
if(0 >= a <=10){
    ...
}
// 这种方法实践之后不行

// 第二种，描述两段区间之外，从而得到闭区间
if(a < 0){

}else if(a > 10){

}else{
    ...
}
// 这种方法实测可行
```

