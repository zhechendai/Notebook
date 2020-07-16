<!--
 * @Date: 2020-07-16 12:09:34
 * @Author: Dai Zhechen
 * @Github: https://github.com/zhechendai
 * @LastEditTime: 2020-07-16 16:03:00
 * @Copyright ©️ 2020 Dai Zhechen. All Rights Reserved.
--> 

结构类型
====

枚举
----

* 常量符号化
  * 用符号而不是具体的数字来表示程序中的数字
* 枚举
  * 用枚举而不是定义独立的const int变量
  * `enum COLOR {RED, YELLOW, GREEN};`

* 枚举是一种用户定义的数据类型，它用关键字**enum**以如下语法来声明：
  * `enum 枚举类型名字 {名字0, ..., 名字n};`
* 枚举类型名字通常并不是真的使用，要用的是在大括号里的名字，因为他们就是常量符号，它们的类型是int，值则依次从0到n。如，
  * `enum colors {red, yellow, green};`
* 就创建了三个常量，red的值是0，yellow的值是1，而green的值是2。
* 当需要一些可以排列起来的常量时，定义枚举的意义就是给了这些常量值名字

* 枚举量可以作为值
* 枚举类型可以跟上enum作为类型
* 但是实际上是以整数来做内部计算和外部输入输出的

套路：自动技术枚举
* `enum COLOR {RED, YELLOW, GREEN, NumCOLORS};`中的`NumCOLORS`的值正好为3，可以用来表示枚举的个数
* 这样需要遍历所有的枚举量或者需要建立一个用枚举量做下标的数组的时候就很方便了

枚举量
* 声明枚举量的时候可以指定值
  * `enum COLOR {RED=1, YELLOW, GREEN=5};`
  * 此时RED=1，YELLOW=2，GREEN=5

枚举只是int
* 即使给枚举类型的变量赋值不存在的整数值也没有任何warning或error
* 原因是
  * 虽然枚举类型可以当作类型使用，但是实际上很(bu)少(hao)用
  * 如果有意义上排比的名字，用枚举比const int方便
  * 枚举比宏(macro)好，因为枚举有int类型，宏没有类型
* 人们使用C的枚举主要是为了来定义符号量，而不是把它当作枚举类型来做


结构
----

如果想表达的数据比较复杂，不是一个值，如日期。但又希望用一个整体来表达，此时需要使用结构
* 一个结构就是一个复合的数据类型，在里面可以有很多给种类型的它的成员，然后用一个变量来表达 

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/2.05.13.png)

在函数内/外
* 和本地变量一样，在函数内部声明的结构类型只能在函数内部使用
* 所以**通常在函数外部声明结构类型**，这样就可以被多个函数所使用了

声明结构的形式
```c
// 第一种
struct point{
    int x;
    int y;
};
struct point p1,p2;
// p1和p2都是point里面有x和y的值

// 第二种
struct{
    int x;
    int y;
} p1,p2;
// p1和p2都是一种无名结构，里面有x和y

// 第三种
struct point{
    int x;
    int y;
} p1,p2;
// p1和p2都是point里面有x和y的值t
```
* 对于第一和第三种形式，都声明了结构point。
* 但是第二种形式没有声明point，只是定义了两个变量

结构初始化
* `struct data today = {07,16,2020};`
* `struct data thismonth = {.month=7, .year=2014};`和数组一样，没有被赋值到的，为0

结构成员
* 结构和数组有点像
* 数组用[]运算符和下标访问其成员
  * a[0]=10;
* 结构用`.`运算符和名字访问其成员
  * `today.day`

结构运算
* 要访问整个结构，直接用结构变量的名字
* 对于整个结构，可以做赋值、取地址，也可以传递给函数参数
  * `p1=(struct point){5,10}:`相当于`p1.x=5; p1.y=10;`
  * `p1=p2;`相当于`p1.x=p2.x; p1.y=p2.y;`

结构指针
* 和数组不同，结构变量的名字并不是结构变量的地址，必须使用&运算符
  * `struct date *pData = &today;`

结构与函数

结构作为函数参数`int numberofDays(struct date d)`
* 整个结构可以作为参数的值传入函数
* 这时候是在函数内新建一个结构变量，并复制调用者的结构的值
* 也可以返回一个结构
* 这与数组完全不同

输入结构
* **没有直接方式**可以一次scanf一个结构
* 解决方案
  * 在这个输入函数中，完全可以创建一个临时的结构变量，然后把这个结构返回给调用者
  * ![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/2.53.11.png)
  * 也可以把y的地址传给函数，函数的参数类型是指向一个结构的指针。（这样的话，访问结构的成员的方式需要做出调整）
* 不传结构，而传结构的指针

指向结构的指针

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/2.59.22.png)

* 用`->`表示指针所指的结构变量中的成员

结构指针参数

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_BD2E1ADF05A0-1.jpeg)

* 好处是传入传出只是一个指针的大小
* 如果需要保护传入的结构不被函数修改
  * const struct point *p
* 返回传入的指针是一种套路

结构中的结构

结构数组
```c
struct date dates[100];
struct date dates[]={
    {4,5,2005},{2,4,2005}
};
```

结构中的结构
```c
struct dateAndTime{
    struct date sdate;
    struct time stime;
};
```

嵌套的结构

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_200A51AB362D-1.jpeg)

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_200A51AB362D-2.jpeg)

结构中的结构的数组

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/3.16.01.png)

结构和数组的联系与区别
* 数组和结构都是用来存大数据的一个容器，他们都可以用指针来操作，不同的是数组要定义的必须是同一类型的，而结构可以多种类型同时定义
* 数组作为形参是地址传递，而结构体是值的传递。数组中可以存放结构体类型变量，结构体中也可出现数组数据项，它们都是相通的。只是数组存储的是一类型数据。而结构体是不同类型数据的集合。

对结构的探究
* 对结构，我们没有像对数组那样使用sizeof和&这两个工具来探究一下。我们把这个任务留给你。建议可以做这么几个方向的探究：
  * 不同的成员变量组合，结构的sizeof如何，是否正好等于全部成员的sizeof之和？
  * 结构内的成员之间是否连续，相邻的成员的地址的差是否等于对应的成员的sizeof？
* 在存储过程中，为了提高CPU的存储速度，编译器会对变量的起始地址做“对齐”处理。VC规定结构体的各变量存放的起始地址，相对于结构体的起始地址的偏移量必须是该变量的类型所占字节数的倍数，并且整个结构体的字节数必须是该结构体中占用空间最大的类型的字节数的整数倍。

联合
----

自定义数据类型（typedef）
* C语言提供了一个叫做**typedef**的功能来声明一个已有的数据类型的新名字。比如：
  * `typedef int Length;`使得*Length*成为*int*类型的别名
* 这样，Length这个名字就可以代替int出现在变量定义和参数声明的地方了：
  * `Length a,b,len;`
  * `Length numbers[10];`

* 声明新的类型的名字
  * 新的名字是某种类型的别名
  * 改善了程序的可读性

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_65129E221818-1.jpeg)

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_49E89A32E77C-1.jpeg)

联合
* 联合起来使用同一份空间（例如下面的i,c在内存中占用一份空间）
* 不管有多少成员，所有成员合起来占用的空间只有一份

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/3.48.48.png)

* 储存
  * 所有成员共享一个空间
  * 同一时间只有一个成员是有效的
  * union的大小是其最大的成员
* 初始化
  * 对第一个成员做初始化
* 应用
  * 得到一个整数, double, float...内部的各个字节，例如写文件的时候，可以作为一个中间的媒介

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/4.01.10.png)
>这个结果表明我们所用的CPU是小端