<!--
 * @Date: 2020-07-17 14:47:05
 * @Author: Dai Zhechen
 * @Github: https://github.com/zhechendai
 * @LastEditTime: 2020-07-17 16:52:41
 * @Copyright ©️ 2020 Dai Zhechen. All Rights Reserved.
--> 

线性结构
====

线性表及其实现
----

### 什么是线性表

“线性表”：由**同类型数据元素**构成有序序列的线性结构
* 表中元素个数称为线性表的**长度**
* 线性表没有元素时，称为**空表**
* 表起始位置称为**表头**，表结束位置称**表尾**

### 线性表的抽象数据类型描述

* 类型名称：线性表（List）
* 数据对象集：线性表是 n (≥0) 个元素构成的**有序序列**（a<sub>1</sub>, a<sub>2</sub>, ...,a<sub>n</sub>)
* 操作集：线性表 L ∈ List，整数 i 表示位置，元素 X ∈ ElementType
* 线性表基本操作主要有：
  * `List MakeEmpty()`： 初始化一个空线性表 L
  * `ElementType FindKth(int K,List L)`：根据位序 K，返回相应元素
  * `int Find(ElementType X,List L)`：在线性表 L 中查找 X 的第一次出现位置
  * `void Insert(ElementType X,int i,List L)`：在位序 i 前插入一个新元素 X
  * `void Delete(int i,List L)`：删除指定位序 i 的元素
  * `int Length(List L)`：返回线性表 L 的长度 n

### 线性表储存方法
#### 1. 线性表的顺序储存实现
