<!--
 * @Date: 2020-07-20 20:58:15
 * @Author: Dai Zhechen
 * @Github: https://github.com/zhechendai
 * @LastEditTime: 2020-07-20 22:03:13
 * @Copyright ©️ 2020 Dai Zhechen. All Rights Reserved.
--> 

树（上）
====

#### 树的定义
* 树（Tree）：n（n≥0）个结点构成的有限集合
* 当 n=0 时，称为空树

#### 特征

对于任一棵非空树（n＞0），它具备以下特征：
* 树中有个称为“根（Root）”的特殊结点，用 r 表示
* 其余结点可分为 m(m>0) 个互不相交的有限集 T<sub>1</sub>, T<sub>2</sub>, ..., T<sub>m</sub>,其中每个集合本身又是一棵树，称为原来树的"子树（SubTree）"
* 子树是不相交的
* 除根结点外，每个结点有且仅有一个父结点
* 一棵 N 个结点的树有 N-1 条边

#### 基本术语
* 结点的度（Degree）：结点的子树个数
* 树的度：树的所有结点中最大的度数
* 叶结点（Leaf）：度为 0 的结点
* 父结点（Parent）：有子树的结点是其子树的根结点的父结点
* 子结点（Child）：若 A 结点是 B 结点的父结点，则称 B 结点是 A 结点的子结点，也称孩子结点
* 兄弟结点（Sibling）：具有同一父结点的各个结点彼此是兄弟结点
* 路径：从结点 n<sub>1</sub> 到 n<sub>k</sub> 的路径为一个结点序列 n<sub>1</sub>, n<sub>2</sub>, ..., n<sub>k</sub>, n<sub>i</sub> 是 n<sub>i+1</sub> 的父结点
* 路径长度：路径所包含边的个数
* 祖先结点（Ancestor）：沿树根到某一结点路径上的所有结点都是这个结点的祖先结点
* 子孙结点（Descendant）：某一结点的子树中的所有结点是这个结点的子孙
* 结点的层次（Level）：规定根结点在 1 层，其他任一结点的层数是其父结点的层数加一
* 树的深度（Depth）：树中所有结点中的最大层次是这棵树的深度

#### 树的表示

1. 儿子-兄弟表示法

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/20181026114834352.jpg)

* Element 存值
* FirstChild 指向第一个儿子
* NextSibling 指向下一个兄弟

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/20181026114856464.jpg)

2. 二叉树

即度为 2 的树

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/20181026114920772.jpg)

* Element 存值
* Left 指向左子树
* Right 指向右子树

二叉树其实就是儿子-兄弟表示法的链表右移 45° 得到的结果

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/20181026114938359.jpg)


#### 二叉树

1. 定义

* 二叉树 T：一个有穷的结点集合
* ​这个集合可以为空
* ​若不为空，则它是由**根结点**和称为其**左子树 T<sub>L</sub>** 和**右子树 T<sub>R</sub>** 的两个不相交的二叉树组成
* ​二叉树的子树有左右顺序之分

2. 五种基本形态

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/20181026114956314.jpg)

3. 特殊形态
4. 
* 斜二叉树
  * 只有左儿子或只有右儿子

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/20181026115013432.jpg)

* 完美二叉树（满二叉树）
  * 除最后一层叶结点外，每个结点都有两个子结点

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/20181026115028605.jpg)

* 完全二叉树
  * 有 n 个结点的二叉树，对树中结点按从上至下、从左到右顺序进行编号，编号为 i（1 ≤ i ≤ n）结点与满二叉树中编号为 i 结点在二叉树中位置相同

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/20181026115044316.jpg)

4. 重要性质
* 一个二叉树第 i 层的最大结点数为：2<sup>i−1</sup>，i ≥ 1
* 深度为 k 的二叉树有最大结点总数为：2<sup>k</sup>-1，k ≥ 1
* 对任何非空二叉树 T，若 n<sub>0</sub> 表示叶结点的个数、n<sub>2</sub> 是度为 2 的非叶结点个数，那么二者满足关系 n<sub>0</sub> = n<sub>2</sub> +1


5. 抽象数据类型定义

* 类型名称：二叉树
* 数据对象集：一个有穷的结点集合，若不为空，则由根结点和其左、右二叉子树组成
* 操作集：BT ∈ BinTree，Item ∈ ElementType

主要操作有：

* `Boolean IsEmpty(BinTree BT)`：判别 BT 是否为空
* `void Traversal(BinTree BT)`：遍历，按某顺序访问每个结点
* `BinTree CreatBinTree()`：创建一个二叉树

常用的遍历方法有：

* `void PreOrderTraversal(BinTree BT)`：先序——根、左子树、右子树
* `void InOrderTraversal(BinTree BT)`：中序——左子树、根、右子树
* `void PostOrderTraversal(BinTree BT)`：后序——左子树、右子树、根
* `void LevelOrderTraversal(BinTree BT)`：层次遍历，从上到下、从左到右

#### 储存结构

1. 顺序存储结构

按从上至下、从左到右顺序存储 n 个结点的完全二叉树的结点父子关系：

* 非根结点（序号 i > 1）的父结点的序号是 ⌊i/2⌋（向下取整）
* 结点（序号为 i）的左孩子结点的序号是 2i（若 2 i ≤ n，否则没有左孩子
* 结点（序号为 i）的右孩子结点的序号是 2i+1（若 2 i +1 ≤ n，否则没有右孩子

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/20181026115118948.jpg)

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/20181026115134161.jpg)


2. 链表存储

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/20181026114920772.jpg)


```c
typedef struct TreeNode *BinTree;
struct TreeNode{
	Element Data;  // 存值 
	BinTree Left;    // 左儿子结点 
	BinTree Right;   // 右儿子结点 
};
```