<!--
 * @Date: 2020-07-17 14:47:05
 * @Author: Dai Zhechen
 * @Github: https://github.com/zhechendai
 * @LastEditTime: 2020-07-19 21:03:23
 * @Copyright ©️ 2020 Dai Zhechen. All Rights Reserved.
--> 

线性结构
====

线性表及其实现
----

#### 什么是线性表

“线性表”：由**同类型数据元素**构成有序序列的线性结构
* 表中元素个数称为线性表的**长度**
* 线性表没有元素时，称为**空表**
* 表起始位置称为**表头**，表结束位置称**表尾**

#### 线性表的抽象数据类型描述

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

#### 线性表储存方法

#### 线性表的顺序储存实现

* 利用数组的连续存储空间顺序存放线性表的各元素
* 注：顺序存储中是序号是下标，从 0 开始

```c
#include<stdio.h>
#include<malloc.h>
#define MAXSIZE 100  // MAXSIZE 定义为 Data 数组的大小
typedef int ElementType;  // ElementType 可定义为任意类型
typedef struct LNode *List; 
struct LNode{
   ElementType Data[MAXSIZE]; 
   int Last;  // Last 定义线性表的最后一个元素
};
List L;
//访问下标为 i 的元素：L->Data[i]
//线性表的长度：L->Last+1

//初始化顺序表
List MakeEmpty();  
int Find(ElementType X,List L); //查找 X 第一次出现的下标 
void Insert(ElementType X,int i,List L); //在下标为 i 的地方插入 X 
void Delete(int i,List L);   //删除下标为 i 的当前值 
ElementType FindKth(int K,List L);  //返回下标为 K 的当前值
int Length(List L);  //返回顺序表的长度 
 
//初始化 
List MakeEmpty(){
    List L;
    L = (List)malloc(sizeof(struct LNode));
    L->Last = -1;
    return L;
}

// 按值查找 
int Find(ElementType X,List L){
    int i=0;
    while(i <= L->Last && L->Data[i] != X)  
        i++;
    if(L->Last < i)  //如果没找到，返回 -1
        return -1; 
    else    // 找到后返回下标 
        return i;
}

// 插入
void Insert(ElementType X,int i,List L){
    int j;
    if(L->Last == MAXSIZE-1){  //位置已满 
        printf("表满");
        return;
    }
    if(i < 0 || L->Last+1 < i){  //位置越界，如果将数插入 L->Data[L->Last+1]，下面都不用腾位置了 
        printf("位置不合法");
        return;
    }
    for(j=L->Last;j>=i;j--)   // 从后往前依次向后挪一个，给 a[i]腾出位置     
        L->Data[j+1] = L->Data[j];   
    L->Data[i] = X;    //新元素插入
    L->Last++;    // Last仍然指向最后元素
    return;
} 

//删除
void Delete(int i,List L){
    int j;
    if(i < 0 || L->Last <i){  //位置越界，而删除最多到 L->Data[L->Last]
        printf("L->Data[%d]不存在元素",i);
        return;
    }
    for(j=i;j<=L->Last;j++)   // 从前往后依次向前挪一个，将 a[i] 覆盖了 
        L->Data[j-1] = L->Data[j];
    L->Last--;  // Last仍然指向最后元素
    return;
}

// 按序查找
ElementType FindKth(int K,List L){
	if(K < 0 || L->Last < K){  //位置越界
        printf("L->Data[%d]不存在元素",K);
        return;
    }
    return L->Data[K];
}

//表长
int Length(List L){
	return L->Last+1;
} 

int main(){
	int i=0;
	L = MakeEmpty();
	Insert(11,0,L);
	printf("在线性表L-Data[0]插入11\n");
	Insert(25,0,L);
	printf("在线性表L-Data[0]插入25\n");
	Insert(33,0,L);
	printf("在线性表L-Data[0]插入33\n");
	Insert(77,0,L);
	printf("在线性表L-Data[0]插入77\n");
	printf("此时的线性表为："); 
	for(i=0;i<Length(L);i++)
		printf("%d ",L->Data[i]);
	printf("\n");
	printf("查找值为12的下标是：%d\n",Find(12,L));
	printf("下标为3的线性表的值是：%d\n",FindKth(3,L));
	Delete(2,L);
	printf("删除线性表中下标为2的元素\n");
	Delete(2,L);
	printf("删除线性表中下标为2的元素\n");
	printf("此时的线性表为："); 
	for(i=0;i<Length(L);i++)
		printf("%d ",L->Data[i]);
	printf("\n"); 
	return 0;
} 
```

#### 线性表的链表存储实现
* 不要求逻辑上相邻的两个元素物理上也相邻，通过"链"建立起数据之间的逻辑关系
  * 插入、删除不需要移动数据元素，只需要修改"链"
* 注：链表存储中序号从 1 开始

```c
#include<stdio.h>
#include<malloc.h>
typedef int ElementType; // ElementType 可定义为任意类型
typedef struct LNode *List;
struct LNode{
	ElementType Data;   //数据域 
	List Next;   // 下一个链表的地址 
}; 
List L;

List MakeEmpty(); //初始化链表 
int Length(List L);  // 以遍历链表的方法求链表长度 
List FindKth(int K,List L);  // 按序号查找 
List Find(ElementType X,List L);  // 按值查找 
List Insert(ElementType X,int i,List L);  //将 X 插入到第 i-1(i>0) 个结点之后 
List Delete(int i,List L); // 删除第 i(i>0) 个结点 
void Print(List L); // 输出链表元素 

// 初始化链表 
List MakeEmpty(){
	List L = (List)malloc(sizeof(struct LNode));
	L = NULL;
	return L;
}

//求表长 
int Length(List L){
	List p = L;
	int len=0;
	while(p){  // 当 p 不为空 
		p = p->Next;
		len++;
	}
	return len;
} 

// 按序查找 
List FindKth(int K,List L){
	List p = L;
	int i = 1;  //从 1 开始 
	while(p && i<K){
		p = p->Next;
		i++;
	}
	if(i == K)    // 找到了 
		return p;
	else    // 未找到 
		return NULL;
} 

// 按值查找  
List Find(ElementType X,List L){
	List p = L;
	while(p && p->Data!=X)
		p = p->Next;
	// 找到了，返回 p
	// 未找到，返回 NULL，此时 p 等于 NULL 
	return p;   
} 

/* 插入
1. 用 s 指向一个新的结点
2. 用 p 指向链表的第 i-1 个结点 
3. s->Next = p->Next，将 s 的下一个结点指向 p 的下一个结点 
4. p->Next = s，将 p 的下一结点改为 s   */
List Insert(ElementType X,int i,List L){
	List p,s;
	if(i == 1){     // 新结点插入在表头 
		s = (List)malloc(sizeof(struct LNode));
		s->Data = X;
		s->Next = L;
		return s;     //插入的结点为头结点 
	}
	p = FindKth(i-1,L);   // 找到第 i-1 个结点
	if(!p){   // 第 i-1 个结点不存在 
		printf("结点错误");
		return NULL;
	}else{
		s = (List)malloc(sizeof(struct LNode));
		s->Data = X;
		s->Next = p->Next;   //将 s 的下一个结点指向 p 的下一个结点 
		p->Next = s;   // 将 p 的下一结点改为 s
		return L;
	}
}

/* 删除
1. 用 p 指向链表的第 i-1 个结点 
2. 用 s 指向要被删除的的第 i 个结点
3. p->Next = s->Next，p 指针指向 s 后面
4. free(s)，释放空间 
*/
List Delete(int i,List L){
	List p,s;
	if(i==1){   //如果要删除头结点 
		s = L;
		if(L)   // 如果不为空 
			L = L->Next;
		else
			return NULL;
		free(s);   // 释放被删除结点 
		return L; 
	}
	p = FindKth(i-1,L);    // 查找第 i-1 个结点
	if(!p || !(p->Next)){     // 第 i-1 个或第 i 个结点不存在 
		printf("结点错误");
		return NULL;
	}else{
		s = p->Next;    // s 指向第 i 个结点 
		p->Next = s->Next;  //从链表删除 
		free(s);  // 释放被删除结点 
		return L;
	}
}

// 输出链表元素 
void Print(List L){
	List t;
	int flag = 1;
	printf("当前链表为：");
	for(t = L;t;t =t->Next){
		printf("%d  ",t->Data);
		flag = 0;
	}
	if(flag)
		printf("NULL");
	printf("\n"); 
}

int main(){
	L = MakeEmpty();
	Print(L);
	L = Insert(11,1,L);
	L = Insert(25,1,L);
	L = Insert(33,2,L);
	L = Insert(77,3,L);
	Print(L);
	printf("当前链表长度为：%d\n",Length(L));
	printf("此时链表中第二个结点的值是：%d\n",FindKth(2,L)->Data);
	printf("查找22是否在该链表中：");
	if(Find(22,L))
		printf("是！\n");
	else
		printf("否！\n");
	printf("查找33是否在该链表中：");
	if(Find(33,L))
		printf("是！\n");
	else
		printf("否！\n");
	L = Delete(1,L);
	L = Delete(3,L);
	printf("----------删除后-----\n"); 
	Print(L);
	return 0;
} 
```

#### 广义表

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/4.43.16.png)

* 广义表是**线性表的推广**
* 对于线性表而言，n个元素都是基本的**单元素**
* 广义表中，这些元素不仅可以是单元素也可以是另一个**广义表**

```c
typedef struct GNode *GList;
struct GNode{
  int Tag;              // 标志域：0表示结点是单元素，1表示结点是广义域
  union{                // 子表指针域Sublist与单元素数据域Data复用，即共用储存空间
    ElementType Data;
    GList SubList;
  }URegion;
  GList Next;           // 指向后继结点
}
```

#### 多重链表

多重链表：链表中的节点可能同时隶属于多个链
* 多重链表中结点的**指针域会有多个**，如前面例子包含了Next和SubList两个指针域；
* 但包含两个指针域的链表并不一定是多重链表，比如在**双向链表不是多重链表**

多重链表有广泛的用途：基本上如**树**、**图**这样相对复杂的数据结构**都可以采用多重链表**方式实现存储




堆栈
----

#### 什么是堆栈

堆栈（Stack）：具有一定操作约束的线性表

* 只在一端（栈顶，Top）做插入、删除
* 插入数据：入栈（Push）
* 删除数据：出栈（Pop）
* 后入先出：Last In First Out（LIFO）

#### 堆栈的抽象数据类型描述

* 类型名称：堆栈（Stack）
* 数据对象集：一个有 0 个或多个元素的有穷线性表
* 操作集：长度为 MaxSize 的堆栈 S ∈ Stack，堆栈元素 item ∈ ElementType

堆栈的基本操作主要有：

* `Stack CreateStack(int MaxSize)`：生成空堆栈，其最大长度为 MaxSize
* `int IsFull(Stack S,int MaxSize)`：判断堆栈 S 是否已满
* `void Push(Stack S,ElementType item)`：将元素 item 压入堆栈
* `int IsEmpty(Stack S)`：判断堆栈 S 是否为空
* `ElementType Pop(Stack S)`：删除并返回栈顶元素

#### 栈的顺序存储实现

* 栈的顺序存储结构通常由一个一维数组和一个记录栈顶元素位置的变量组成

```c
#include<stdio.h>
#include<malloc.h> 
#define MaxSize 100   // 堆栈元素的最大个数 
typedef int ElementType; // ElementType 暂时定义为 int 类型 
typedef struct SNode *Stack;
struct SNode{
	ElementType Data[MaxSize];   // 存储堆栈元素
	int Top;  // 记录栈顶元素下标 
}; 
Stack S;

Stack CreateStack();  // 初始化堆栈 
int IsFull(Stack S); // 判断堆栈是否已满 
int IsEmpty(Stack S);   // 判断堆栈是否为空 
void Push(Stack S,ElementType item);   // 入栈 
ElementType Pop(Stack S);   // 出栈 

// 初始化堆栈 
Stack CreateStack(){
	S = (Stack)malloc(sizeof(struct SNode));
	S->Top = -1;
	return S;
} 

// 是否已满 
int IsFull(Stack S){
	return (S->Top == MaxSize-1);
}

// 是否为空 
int IsEmpty(Stack S){
	return (S->Top == -1); 
} 

// 入栈 
void Push(Stack S,ElementType item){
	if(IsFull(S)){   // Top 从 0 开始 
		printf("堆栈满");
		return;
	}else{
		S->Top++;   // 栈顶元素加一 
		S->Data[S->Top] = item;   // 放进最上 
		return;
	}
}

// 出栈
ElementType Pop(Stack S){
	if(IsEmpty(S)){
		printf("堆栈空");
		return;
	}else{
		ElementType val = S->Data[S->Top];  //取出最上 
		S->Top--;  // 栈顶元素减一 
		return val;
	}
}
int main(){
	S = CreateStack();
	printf("5入栈\n");
	Push(S,5);
	printf("7入栈\n");
	Push(S,7);
	printf("66入栈\n");
	Push(S,66);
	printf("%d出栈\n",Pop(S));
	printf("%d出栈\n",Pop(S));
	return 0;
}
```

#### 栈的链表存储实现

* 栈的链式存储结构实际上就是一个单链表，叫做链栈。插入和删除操作只能在链栈的栈顶进行

```c
#include<stdio.h>
#include<malloc.h>
typedef int ElementType;
typedef struct SNode *Stack;
struct SNode{
	ElementType Data;
	Stack Next;
};


Stack CreateStack();  // 初始化链栈 
int IsEmpty(Stack S);  // 判断链栈是否为空 
void Push(Stack S,ElementType item);  // 入栈 
ElementType Pop(Stack S);  // 出栈
 

// 初始化 
Stack CreateStack(){
	Stack S;
	S = (Stack)malloc(sizeof(struct SNode));
	S->Next = NULL;
	return S;
}

// 判断是否为空 
int IsEmpty(Stack S){
	return (S->Next == NULL);
}

// 入栈
void Push(Stack S,ElementType item){
	Stack tmp;
	tmp = (Stack)malloc(sizeof(struct SNode));
	tmp->Data = item;
	// 链栈栈顶元素是链表头结点，新入栈的链表在栈顶元素后面 
	tmp->Next = S->Next;   
	S->Next = tmp;
} 

// 出栈
ElementType Pop(Stack S){
	Stack First;
	ElementType TopVal;
	if(IsEmpty(S)){
		printf("堆栈空");
		return;
	}else{
		First = S->Next;   // 出栈第一个元素在栈顶元素后面 
		S->Next = First->Next;  //把第一个元素从链栈删除 
		TopVal = First->Data;   // 取出被删除结点的值 
		free(First);  // 释放空间 
		return TopVal;
	}
} 

int main(){
	Stack S;
	S = CreateStack();
	printf("5入栈\n");
	Push(S,5);
	printf("7入栈\n");
	Push(S,7);
	printf("66入栈\n");
	Push(S,66);
	printf("%d出栈\n",Pop(S));
	printf("%d出栈\n",Pop(S));
	return 0;
} 
```

#### 中缀表达式如何转换为后缀表达式

从头到尾读取**中缀表达式的每个对象**，对不同对象按不同的情况处理。
* 运算数：直接输出
* 左括号：压入堆栈
* 右括号：将**栈顶的运算符弹出**并，**直到遇到左括号**（出栈，不输出）
* 运算符：
  * 若**优先级大于栈顶运算符**时，则把它**压栈**；
  * 若**优先级小于等于栈顶运算符**时，将**栈顶运算符弹出并输出**；再比较新的栈顶运算符，直到该运算符大于栈顶运算符优先级为止，然后**将该运算符压栈**；
* 若各对象处理完毕，则把堆栈中存留的**运算符一并输出**

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/6.10.13.png)

#### 堆栈的其他应用

* 函数调用及递归实现
* 深度优先搜索
* 回溯算法
* ...

列队
----

#### 什么是队列

队列（Queue）：具有一定操作约束的线性表
* 插入和删除操作：只能在一端（front）插入，而在另一端（rear）删除
* 数据插入：入队列（AddQ）
* 数据删除：出队列（DeleteQ）
* 先进先出：FIFO

队列的抽象数据类型描述
* 类型名称：队列（Queue）
* 数据对象集：一个有 0 个或多个元素的有穷线性表
* 操作集：长度为 MaxSize 的队列 Q∈Queue，队列元素 item∈ElementType

队列的基本操作主要有：
* `Queue CreateQueue(int MaxSize)`：生成长度为 MaxSize 的空队列
* `int IsFull(Queue Q)`：判断队列 Q 是已满
* `void AddQ(Queue Q,ElementType item)`：将数据元素 item 插入队列 Q 中
* `int IsEmpty(Queue Q)`：判断队列 Q 是否为空
* `ElementType DeleteQ(Queue Q)`：将队头数据元素从队列中删除并返回

#### 循环队列的顺序存储实现

* 队列的顺序存储结构通常由一个一维数组和一个记录队列头元素位置的变量 front 以及一个记录队列尾元素位置的变量 rear 组成，其中 front 指向整个队列的头一个元素的再前一个，rear 指向的是整个队列的最后一个元素，从 rear 入队，从 front 出队，且仅使用 n-1 个数组空间

```c
#include<stdio.h>
#include<malloc.h>
#define MaxSize 100
typedef int ElementType;
typedef struct QNode *Queue;
struct QNode{
	ElementType Data[MaxSize];
	int front;   // 记录队头 
	int rear;    // 记录队尾 
};

Queue CreateQueue();  // 初始化队列 
void AddQ(Queue Q,ElementType item);  //  入队
int IsFull(Queue Q); // 判断队列是否已满 
ElementType DeleteQ(Queue Q);  // 出队 
int IsEmpty(Queue Q); // 判断队列是否为空 

// 初始化 
Queue CreateQueue(){
	Queue Q;
	Q = (Queue)malloc(sizeof(struct QNode));
	Q->front = -1;
	Q->rear = -1;
	return Q;
} 

// 判断队列是否已满
int IsFull(Queue Q){
 	return ((Q->rear+1) % MaxSize == Q->front);
}

// 入队 
void AddQ(Queue Q,ElementType item){
	if(IsFull(Q)){
		printf("队列满");
		return;
	}else{ 
		Q->rear = (Q->rear+1) % MaxSize;
		Q->Data[Q->rear] = item; 
	}
}

//判断队列是否为空
int IsEmpty(Queue Q){
	return (Q->front == Q->rear);
}
 
// 出队
ElementType DeleteQ(Queue Q){
	if(IsEmpty(Q)){
		printf("队列空");
		return 0;
	}else{
		Q->front = (Q->front+1) % MaxSize;
		return Q->Data[Q->front];
	}
} 

int main(){
	Queue Q;
	Q = CreateQueue();
	AddQ(Q,3);
	printf("3入队\n");
	AddQ(Q,5);
	printf("5入队\n");
	AddQ(Q,11);
	printf("11入队\n");
	printf("%d出队\n",DeleteQ(Q));
	printf("%d出队\n",DeleteQ(Q));
	return 0;
} 
```

#### 队列的链式存储实现

* 队列的链式存储结构也可以用一个单链表实现。插入和删除操作分别在链表的两头进行，front 在链表头，rear 在链表尾，从 rear 入队，从 front 出队

```c
#include<stdio.h>
#include<malloc.h>
typedef int ElementType;
typedef struct QNode *Queue;
struct Node{
	ElementType Data;
	struct Node *Next;
};
struct QNode{
	struct Node *rear;    // 指向队尾结点 
	struct Node *front;   // 指向队头结点 
};

Queue CreateQueue();  // 初始化队列 
void AddQ(Queue Q,ElementType item);  //  入队
ElementType DeleteQ(Queue Q);  // 出队 
int IsEmpty(Queue Q); // 判断队列是否为空 

// 初始化 
Queue CreateQueue(){
	Queue Q;
	Q = (Queue)malloc(sizeof(struct QNode));
	Q->front = NULL;
	Q->rear = NULL;
	return Q;
}

// 是否为空 
int IsEmpty(Queue Q){
	return (Q->front == NULL);
}

// 入队
void AddQ(Queue Q,ElementType item){
	struct Node *node;
	node = (struct Node *)malloc(sizeof(struct Node));
	node->Data = item;
	node->Next = NULL;
	if(Q->rear==NULL){  //此时队列空 
		Q->rear = node;
		Q->front = node;
	}else{ //不为空 
		Q->rear->Next = node;  // 将结点入队 
		Q->rear = node;   // rear 仍然保持最后 
	}
} 

// 出队
ElementType DeleteQ(Queue Q){
	struct Node *FrontCell;
	ElementType FrontElem;
	if(IsEmpty(Q)){
		printf("队列空");
		return 0;
	}
	FrontCell = Q->front;
	if(Q->front == Q->rear){ // 队列中只有一个元素 
		Q->front = Q->rear = NULL; 
	}else{
		Q->front = Q->front->Next;
	}
	FrontElem = FrontCell->Data;
	free(FrontCell);
	return FrontElem;
}

int main(){
	Queue Q;
	Q = CreateQueue();
	printf("入队5\n"); 
	AddQ(Q,5);
	printf("入队4\n"); 
	AddQ(Q,4);
	printf("入队4\n"); 
	AddQ(Q,3);
	printf("出队%d\n",DeleteQ(Q));
	printf("出队%d\n",DeleteQ(Q));
	printf("出队%d\n",DeleteQ(Q));
	printf("%d\n",DeleteQ(Q));
	return 0;
} 
```

#### 应用实例：多项式加法运算

```c
Polynomial PolyAdd(Polynomial P1, Polynomial P2)
{
  Polynomial front, rear, temp;
  int sum;
  rear =(Polynomial)malloc(sizeof(struct PolyNode));  // 为方便表头插入，先产生一个临时空结点作为结果多项式链表头
  front = rear; //由front记录多项式链表头结点
  while(P1 && P2) //当两个多项式都有非零项待处理时
    switch(Compare(P1->expon, P2->expon))
    {
      case 1:   // P1中的数据项指数较大
            Attach(P1->coef, P1->expon, &rear);
            P1 = P1->link;
            break;
      case -1:  // P2中的数据项指数较大
            Attach(P2->coef, P2->expon, &rear);
            P2 = P2->link;
            break;
      case 0:  // 两数据项指数相等
            sum = P1->coef + P2->coef;
            if(sum) Attach(sum, P1->expon, &rear);  // 判断系数和是否为0
            P1 = P1->link;
            P2 = P2->link;
            break;
    }
    // 将未处理完的另一个多项式的所有节点依次复制到结果多项式中去
    for(;P1;P1 = P1->link)Attach(P1->coef,P1->expon,&rear);
    for(;P2;P2 = P2->link)Attach(P2->coef,P2->expon,&rear);
    rear->link = NULL;
    temp = front;
    front = front->link; //令front指向结果多项式第一个非零项
    free(temp); // 释放临时空表头结点
    return front;
}
```

#### Attach函数

```c
void Attach(int c, int e, Polynomial *pRear)
{
  // 由于在本函数中需要改变当前结果表达式尾项指针的值，
  // 所以函数传递进来的是结点指针的地址， *pRear指向尾项
  Polynomial P;

  P =(Polynomial)malloc(sizeof(struct PolyNode));  //申请新结点
  P->coef = c;   // 对新结点赋值
  P->expon = e;
  P->link = Null;
  // 将P指向的新结点插入到当前结果表达式尾项的后面
  （*pRear)->link = P;
  *pRear = P; // 修改pRear的值
}
```
