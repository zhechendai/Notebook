<!--
 * @Date: 2020-07-12 14:06:08
 * @Author: Dai Zhechen
 * @Github: https://github.com/zhechendai
 * @LastEditTime: 2020-07-12 18:45:43
 * @Copyright ©️ 2020 Dai Zhechen. All Rights Reserved.
--> 

数组
====

数组
----

初试数组

eg. 如何写一个程序计算用户输入的数的平均值，并输入所有大于平均数的数？
* 如何记录很多数`int num1, num2, num3...?`
* 希望有一个地方可以储存这些输入的数  
  * 引入数组
```c
int number[100];
scanf("%d", &number[i]);
```

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/2.19.07.png)

>* 这个程序存在安全隐患  
> * 我们没有判断cnt是否小于100，如果输入的数的个数大于100？

定义数组
* <类型>变量名称[元素数量]；
  * int grades[100];
  * double weight[20];
* 元素数量必须是整数
* C99之前：元素数量必须是编译时刻确定的字面量
* 数组是一种容器（放东西的东西），特点是：
  * 其中所有的元素具有相同的数据类型
  * 一旦创建，不能改变大小
  * *数组中的元素在内存中是连续依次排列的

`int a[10]`
* 一个int的数组
* 10个单元：a[0], a[1],..., a[9]

| a[0] | a[1] | a[2] | a[3] | a[4] | a[5] | a[6] | a[7] | a[8] | a[9] 
| ---- |---- |---- |---- |---- |---- |---- |---- |---- |---- 

* 每个单元就是一个int类型的变量
* 可以出现在赋值的左边或右边
  * a[2] = a[1]+6;
* *在赋值左边的叫做左值，在右边的叫做右值（指针章节详细解释）

数组的单元
* 数组的每个单元就是数组类型的一个变量
* 使用数组是放在[]中的数字叫做**下标**或**索引**，下标从0开始计数：
```c
grades[0]
grades[99]
average[5]
```

有效的下标范围
* 编译器和运行环境都不会检查数组的下标是否越界，无论是对数组单元做读还是写
* 一旦程序运行，越界的数组访问可能造成问题，导致程序崩溃
  * segmentation fault
* 但是也有可能运气好，没造成严重的后果
* 所以要保证程序只使用有效的下标值：[0, 数组的大小-1]

如何避免越界
* 判断cnt>100时，停止读入
* C99 Only方法：先输入数组中将包含几个数，然后定义一个该大小的数组，例如
![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_F840C593E556-1.jpeg)

是否可以定义长度为0的数组
* `int a[0];`
* 可以存在，但是无用


eg. 统计个数（投票统计）

要求：输入数量不确定的[0,9]范围内的整数，统计每一种数字出现的次数，输入-1表示结束
![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/5.12.10.png)

数组运算
----

eg. 在一组给定的数据中，如何找出某个数据是否存在？

数据的集成初始化

`int a[] = {2,4,6,7,1,3,5,9,11,13,23,14,32};`
* 直接用大括号给出数组的所有元素的初始值
* 不需要给出数组的大小，编译器自动数数
`int b[20] = {2}`
* 如果给出了数组的大小，但是后面的初始值数量不足，则其后面的元素被初始化为0

集成初始化时的定位（C99 Only）

```c
int a[10] = {[0] = 2, [2] = 3,6};
```
* 用[n]在初始化数据中给出定位
* 没有定位的数据接在前面的位置后面
* 其他位置的值补0
* 也可以不给出数组大小，让编译器算，此时大小为[n]中n最大的那一位
* 特别适合初始数据稀疏的数组

数组的大小
```c
sizeof(a)/sizeof(a[0])
```
* `sizeof`给出整个数组所占据的内容的大小，单位是字节
* `sizeof(a[0])`给出数组中单个元素的大小，于是相除就得到了数组的单元个数
* 这样的代码，一旦修改数组中初始的数据，不需要修改遍历的代码

数组的赋值
```c
int a[] = {2,4,6,7,1,3,5,9,11,13,23,14,32};
int b[] = a; //不能这样将数组a赋值给数组b
```
* 数组变量本身不能被赋值
* 要把一个数组的所有元素交给另一个数组，必须采用遍历
```c
for ( i=0; i<length; i++){
    b[i] = a[i];
}
```

遍历数组
* 通常都是使用for循环，让循环变量i从0到<数组的长度，这样循环体内最大的i正好是数组最大的有效下标
* 常见的错误：
  * 循环结束条件是<=数组长度，或
  * 离开循环后，继续用i的值来做数组元素的下标（此时i的值正好是length的值，此下标在数组中不存在）

* 数组作为函数的参数时
  * 不能在[]中给出数组的大小
  * 不能再利用`sizeof`来计算数组的元素个数（需要定义一个length）

eg. 素数

构造素数表

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/6.16.04.png)

二维数组

`int a[3][5]`
* 通常理解为a是一个3行5列的矩阵
  
![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/6.18.42.png)

二维数组的遍历
* 二维数组需要两重循环，对行和列分别遍历
```c
for ( i=0; i<3; i++){
    for ( j=0; j<5; j++){
        a[i][j] = i*j;
    }
}
```
* a[i][j]是一个int
* 表示第i行第j列上的单元
  * a[i,j]是什么？  逗号是一种运算符，表达式i,j的计算结果是j，所以a[i,j]=a[j]

二维数组的初始化
```c
int a[][5] = {
    {0,1,2,3,4},
    {2,3,4,5,},
}
```
* **列数**是必须给出的，行数可以由编译器来数
* 每行一个{}，逗号分隔
* 最后的逗号可以存在，有古老的传统
* 如果省略，表示补0
* 也可以用定位（*C99 Only）

eg. tic-tac-toe游戏

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/6.31.50.png)

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_0633.PNG)
![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_0634.PNG)
![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_0635.PNG)
![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_0636.PNG)


>？能否用两重循环来检查行和列