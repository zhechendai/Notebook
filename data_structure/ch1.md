<!--
 * @Date: 2020-07-15 15:32:51
 * @Author: Dai Zhechen
 * @Github: https://github.com/zhechendai
 * @LastEditTime: 2020-07-15 20:37:24
 * @Copyright ©️ 2020 Dai Zhechen. All Rights Reserved.
--> 

基本概念
====

什么是数据结构
----

* 没有官方统一定义
* **数据结构**常与**算法**一起出现

eg1. 如何在书架上摆放图书
* 操作1: 新书怎么插入
* 操作2: 怎么找到某本指定的书

>解决问题方法的效率，跟数据的组织方式有关

eg2. 写程序实现一个函数PrintN，使得传入一个正整数为N的参数后，能顺序打印从1到N的全部正整数
* 循环实现
* 递归实现

令N= 100，1000，10000，100000
* 循环函数一切正常
* 递归函数在N=100000时报错不运行（对空间的占用很恐怖 ）

>解决问题方法的效率，跟空间的利用效率有关

eg3. 写程序计算给定多项式在给定点x的值

>解决问题方法的效率，跟算法的巧妙程度有关

所以到底什么是数据结构
* 数据对象在计算机的中的组织方式
  * 逻辑结构
  * 物理储存结构（数组？链表？）
* 数据对象必定与一系列加在其上的操作相关联
* 完成这些操作所用的方法就是**算法**

描述数据结构——抽象数据类型（Abstract Data Type）
* 数据类型
  * 数据对象集
  * 数据集合相关联的操作集
* 抽象：描述数据类型的方法不依赖于具体实现
  * 与存放数据的机器无关
  * 与数据储存的物理结构无关
  * 与实现操作的算法和编程语言无关
>只描述数据对象集和相关操作集“**是什么**”，并不涉及“**如何做到**”的问题

eg4. “矩阵”的抽象数据类型定义

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_E594F062BB2E-1.jpeg)


什么是算法
----

算法（Algorithm）
* 一个**有限**指令集
* 接受一些输入（有些情况下不需要输入）
* 一定产生输出
* 一定在有限步骤之后终止
* 每一条指令必须
  * 有充分明确的目标，不可以有歧义
  * 计算机能处理的范围之内
  * 描述应不依赖于任何一种计算机语言以及具体的实现手段

eg1. 选择排序算法的伪码描述

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/5.04.26.png)

抽象
* List到底是数组还是链表（虽然看上去很像数组）？
* Swap用函数还是用宏去实现？

>上面的这些细节问题我们在算法中是不考虑的

什么是好的算法？（两个指标）
* **空间复杂度S(n)**————根据算法写成的程序在执行时**占用储存单元的长度**。这个长度往往与输入数据的规模有关。空间复杂度过高的算法可能导致使用的内存超限，造成程序非正常中断。
* **时间复杂度T(n)**————根据算法写成的程序在执行时**耗费时间的长度**。这个长度往往也与输入数据的规模有关。时间复杂度过高的低效算法可能导致我们在有生之年都等不到运行的结果。
>n——数据的规模

在分析一般算法的效率时，我们经常关注下面两种复杂度
* 最坏情况复杂度 $$T_{worst}\left(n\right)$$
* 平均复杂度 $$T_{avg}(n)$$
* $$T_{avg}\left(n\right)\leq T_{worst}(n)$$

复杂度的渐进表示法
* $$T\left(n\right)=O\left(f\left(n\right)\right)$$ 表示存在常数 $$C>0,\;n_0>0$$ 使得当 $$n\geq n_0$$ 时有 $$T(n)\leq C\cdot f\left(n\right)$$
* $$T\left(n\right)=\Omega\left(g\left(n\right)\right)$$ 表示存在常数 $$C>0,\;n_0>0$$ 使得当 $$n\geq n_0$$ 时有 $$T(n)\leq C\cdot g\left(n\right)$$
* $$T\left(n\right)=\Theta\left(h\left(n\right)\right)$$ 表示同时有 $$T\left(n\right)=O\left(h\left(n\right)\right)$$ 和 $$T\left(n\right)=\Omega\left(h\left(n\right)\right)$$

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/5.47.30.png)

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_6E95DD166484-1.jpeg)

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_1DAC8CFC39C1-1.jpeg)

复杂度分析
* 若两段算法分别有复杂度 $$T_1(n)=O(f_1(n))$$ 和 $$T_2(n)=O(f_2(n))$$, 则
  * $$T_1(n)+T_2(n)=max(O(f_1(n)),O(f_1(n)))$$
  * $$T_1(n)\times T_2(n)=O(f_1(n)\times f_2(n))$$
* 若$$T(n)$$ 是关于$$n$$的$$k$$阶多项式，那么$$T(n)=\Theta(n^k)$$
* 一个**for**循环的时间复杂度等于循环次数乘以循环体内代码的复杂度
* **if-else**结构的复杂度取决于**if**的条件判断复杂度和两个分枝部分的复杂度，总体复杂度取三者最大

应用实例：最大子列和问题
----

给定N个正数的序列$$\left\{A_1,\;A_2,\;...,\;A_n\right\}$$，求函数 $$f\left(i,j\right)=max\left\{0,\; \sum_{k=i}^j A_k \right\}$$ 的最大值。

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_CE4D078B2D43-1.jpeg)

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_C96508340EE9-1.jpeg)

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_EAB5862DAD8B-1.jpeg)

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_7CD58D08494B-1.jpeg)

>“**在线**”的意思是指每输入一个数据就进行**即时处理**，在任何一个地方终止输入，算法都能正确给出当前的解。