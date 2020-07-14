<!--
 * @Date: 2020-07-14 11:01:07
 * @Author: Dai Zhechen
 * @Github: https://github.com/zhechendai
 * @LastEditTime: 2020-07-14 16:31:51
 * @Copyright ©️ 2020 Dai Zhechen. All Rights Reserved.
--> 

指针与字符串2
====

指针的使用
----

指针运用场景1
* 交换两个变量的值
```c
void swap(int *pa, int *pb)
{
    int t = *pa;
    *pa = *pb;
    *pb = t;
}
```

指针运用场景2
* 函数返回多个值，某些值就只能通过指针返回
  * 传入的参数实际上是需要保存带回的结果的变量

指针运用场景2b
* 函数返回运算的状态，结果通过指针返回
* 常用的套路是让函数返回特殊的不属于有效范围内的值来表示出错：
  * -1或0（在稳健操作会看到大量例子）
* 但是当任何数值都是有效的可能结果时，就得分开返回了
  * 后续的语言（C++，Java）采用了异常机制来解决这个问题

指针最常见的错误
* 定义了指针变量，还没有指向任何变量，就开始使用指针

指针与数组（见[第八周](week8.html)）

转换
* 总是可以把一个非const的值转换成const的

```c
void f(const int* x);
int a = 15;
f(&a); // OK
const int b = a;

f(&b); //OK
b = a+1; // ERROR
```

* 当要传递的参数的类型比地址大的时候，这是常用的手段：既能用比较少的字节数传递值给参数，又能避免函数对外面的变量的修改

数组变量和指针的关系
* 数组变量可以被看作是const的指针变量，到底是“可以被看作”，还是“就是”指针呢？

```c
#include <stdio.h>
int main(){
    int a[] = {1,1,2,3,4,5,6,7,8,9,0,};
    int *const p = &a[0];
     
    printf("sizeof(a) = %d , sizeof(p) = %d \n",sizeof(a) , sizeof(p));
        //sizeof(a)显示为44，是数组整个的一个长度，sizeof(p)显示为8； 是指针的长度 
    printf("main:p = %p\n",p);
    printf("main:a = %p\n",a);
        //在主函数中仍然是数组的首地址 
    a[13] = 10;
    printf("main:a[13] = %d\n",a[13]); 
    p[13] = 10;
    printf("main:p[13] = %d\n",p[13]);
    //在主函数中都可以顺利执行 
    sum(a , 11);
} 
 
int sum(int a[],int len){
    int sum = 0;
    int i = 0; 
    int *const p = &a[0];
//    int *const a = &a[0]; //[Error] 'a' redeclared as different kind of symbol （a 重新声明为不同类型的符号） 
//    int *const p = &a[0]; //[Error] redefinition of 'p'                          （p重复定义） 
    //上面两个提示的是不同的错误， 说明数组参数实际上不是常量指针这个类型
//    int * a = &a[0];  26    //[Error] 'a' redeclared as different kind of symbol
//    int *const p = &a[0];   //[Error] redefinition of 'p'
    //说明数组参数也不是指针 
    printf("p = %p\n",p);
    printf("a = %p\n",a);
    //他两指向的是一个数组
    p[1] = 10;
    printf("p[1] = %d\n",p[1]);
    a[1] = 10; 
    printf("a[1] = %d\n",a[1]);
    //上面无论执行哪个都是一样的，而且都可执行，这说明在修改变量，访问变量上是相同的
    a[13] = 10;
    printf("a[13] = %d\n",a[13]); 
    p[13] = 10;
    printf("p[13] = %d\n",p[13]);
    //这说明a也并没有存储关于他的长度的信息，它仅仅是一个地址，和p一样，两个都没有警告，可以编译出相同的结果
     
      
      
    for(i = 0;i < len;i++){
        sum += a[i];
    }
    printf("sizeof(a) = %d  \n",sizeof(a) ); 
    //通过编译看以看到sizeof(a)仍然是8，从这点看，传递的数组参数确实和常量指针一样 
    return sum;
}
//从上面的测试来看，当数组变量**作为参数**时，它不是严格意义上的指针，更不是常量指针，但是他可以被看做指针，而且常量指针的操作都可以作用于数组变量
//当数组变量**不是参数**时，即它在主函数中，它有两层含义 ，在用在sizeof上时，它似乎被看做了一个数组整体，但是其他情况下，都可以被当做常量指针。
```

```c
int a[] = {5, 15, 34, 54, 14, 2, 52, 72};
int *p = &a[1];

printf("%d ",p[2]);
printf("%d ",p[0]);
printf("%d\n",p[-1]);
// 结果为：54 15 5
```

指针运算
----

* 给一个指针加1表示要让指针指向下一个变量
```c
int a[10];
int *p = a;
*(p+1)  --> a[1]
```

* 如果指针不是指向一片连续分配的空间，如数组，则这种运算没有意义

示例，
```c
char ac [] ={0,1,2,3,4,5,6,7,8,9, }; 
char *p = ac; 
printf ("p =%p\n", p);
printf ("p+1=%p\n", p+1);
printf ("*(p+1) =%d\n", *(p+1));
// *(p+n) <-> ac [n]

int ai [] ={0,1,2,3,4,5,6,7,8,9, };
int *q = ai; 
printf ("q =%p\n", q);
printf ("q+1=%p\n", q+1);
printf ("*(q+1) =%d\n", *(q+1));
// 结果为：
// p =0x7ffee81fd7be
// p+1=0x7ffee81fd7bf
// *(p+1) =1
// q =0x7ffee81fd790
// q+1=0x7ffee81fd794
// *(q+1) =1
```

指针计算
* 这些算数运算可以对指针做
  * 给指针加、减一个整数（+，+=，-，-=）
  * 递增递减（++/--）
  * 两指针相减  // 给的是**地址差/sizeof** 

`*p++`
* 取出p所指的那个数据来，完事之后顺便把p移到下一个位置去
* *的优先级虽然高，但是没有++高
* 常用于数组类的连续空间操作
* 在某些CPU上，这可以直接被翻译成一条汇编指令

```c
#include <stdio.h>
int main()
{
    char ac [] ={0,1,2,3,4,5,6,7,8,9,-1, }; 
    char *p = &ac[0];
    // 普通遍历方法
    int i;
    for ( i = 0; i < sizeof(ac)/sizeof(ac[0]); i++)
    {
        printf("%d ", ac[i]);
    }
    // 使用*p++的方法
    while (*p != -1)
    {
        printf("%d ", *p++);
    }
    
	return 0;
}
```

指针比较
* <, <=, ==, >, >=, != 都可以对指针做
* 比较他们在内存中的地址
* 数组中的单元的地址肯定是线性递增的

0地址
* 当然你的内存中有0地址，但是0地址通常是个不能随便碰的地址
* 所以你的指针不应该具有0值
* 因此可以用0地址来表示特殊的事情，如
  * 返回的指针是无效的
  * 指针没有被真正初始化（先初始化为0）
* NULL是一个没有预定定义的符号，表示0地址（建议使用NULL，注意为全部大写）
  * 有的编译器不愿意你用0来表示0地址

指针的类型
* 无论指向什么类型，所有的指针的大小都是一样的，因为都是地址
* 但是指向不同类型的指针是不能直接互相赋值的
  * 这是为了避免用错指针

指针类型转换
* void* 表示不知道指向什么东西的指针（只是指向了一块地址）
  * 计算时与char* 相同（但不相通）
* 指针也可以转换类型
  * int \*p = &i; void \*q = (void\*)p;
* 这并没有改变p所指的变量的类型，而是让后人用不同的眼光通过p看它所指的变量
  * 表示我不在当你是int啦，我认为你就是个void

用指针来做什么
* 需要传入较大的数据时用作参数
* 传入数组后对数组做操作
* 函数返回不止一个结果
  * 需要用函数来修改不止一个变量
* 动态申请的内存...

动态内存

输入数据
* 如果输入数据时，先告诉你个数，然后再输入，要记录每个数据
* C99可以用变量做数组定义的大小，C99之前？
* 使用malloc函数：`int *a=(int*)malloc(n*sizeof(int));`

malloc
* `#include <stdlib.h>`
* `void* malloc(size_t size);`
* 向malloc申请的空间的大小是以字节为单位的
* 返回的结果是void*，需要类型转换为自己需要的类型
  * `(int*)malloc(n*sizeof(int))`

示例（非C99）
```c
#include <stdio.h>
#include <stdlib.h>
int main()
{
    int number;
    int* a;
    int i;
    printf("输入数量：");
    scanf("%d", &number);
    a = (int*)malloc(number*sizeof(int));
    for( i=0; i<number; i++ ){
        scanf("%d", &a[i]);
    }
    for( i=number-1; i>=0; i--){
        printf("%d ", a[i]);
    }
    free(a);  // 注意归还
    
	return 0;
}
```

没空间了
* 如果申请失败则返回0，或者叫做NULL
* 你的系统能给你多大的空间？
```c
#include <stdio.h>
#include <stdlib.h>
int main(void)
{
    void *p;
    int cnt = 0;
    while ((p=malloc(100*1024*1024)))
    {
        cnt++;
    }
    printf("分配了%d00MB的空间\n", cnt);
    
	return 0;
}
```

`free()`
* 把申请得来的空间还给“系统”
* 申请过来的空间，最终都是要还的
* 只能还申请来的空间的首地址
* `free(0)`
  * 定义指针后先初始化为0，这样不管任何时候`free(0)`都是正确的

常见问题
* 申请了没free --> 长时间运行内存逐渐下降
* free过了再free
* 地址变过了，直接去free

函数间传递指针

好的模式
* 如果程序中要用到动态分配的内存，并且会在函数之间传递，不要让函数申请内存后返回给调用者
* 因为十有八九调用者会忘了free，或找不到合适的时机来free
* 好的模式是让调用者自己申请，传地址进函数，函数再返回这个地址出来

函数返回指针
* 返回指针没问题，关键是谁的地址？
  * 本地变量（包括参数）？函数离开后这些变量就不存在了，指针所指的是不能用的内存
  * 传入的指针？没问题
  * 动态申请的内存？没问题
  * 全局变量？

函数返回数组（和返回指针是一样的）
* 如果一个函数的返回类型是数组，那么它实际返回的也是数组的地址
* 如果这个数组是这个函数的本地变量，那么回到调用函数那里，这个数组就不存在了
* 所以只能返回
  * 传入的参数：实际就是再调用者那里
  * 全局变量或动态分配的内存

字符串操作
----

putchar
* int putchar(int c);
* 向标准输出写一个字符
* 返回写了几个字符，EOF（-1）表示写失败

getchar
* int getchar(void);
* 从标准输入读入一个字符
* 返回类型是int是为了返回EOF（-1）
  * Windows --> ctrl-Z
  * Unix --> ctrl-D

字符串数组（见[第八周](week8.html)）

程序参数
* `int main(int argc, char const *argv[])`
* `argv[0]`是命令本身
  * 当使用Unix的符号链接时，反应符号链接的名称

字符串函数的实现
----

详细见[第八周](week8.html)，补充

复制一个字符串
```c
char *dst = (char*)malloc(strlen(src)+1);  //保证空间比要拷贝的内容大1，这样内容都能放下
strcpy(dst,src);
```

