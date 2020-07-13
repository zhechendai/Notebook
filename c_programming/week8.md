<!--
 * @Date: 2020-07-13 11:42:15
 * @Author: Dai Zhechen
 * @Github: https://github.com/zhechendai
 * @LastEditTime: 2020-07-13 17:53:26
 * @Copyright ©️ 2020 Dai Zhechen. All Rights Reserved.
--> 

指针与字符串
====

指针
----

取地址运算

运算符`&`
* `scanf("%d", &i);`里的&
* 获得变量的地址，它的操作必须是变量
  * `int i; printf("%x", &i);`
* 地址的大小是否与int相同取决于编译器
  * `int i; printf("%p", &i); // %p 输出为地址` 地址、整数并不永远是相同的
> 32位架构下两个打印值相同；64位架构时，地址大小要大很多  
> `&`是用来取地址的

&不能取的地址
> &取地址这个运算符必须对一个变量去取地址，如果它的右边不是变量，则无法取地址，例如
* 以下不可取
  * &(a+b)
  * &(a++)
  * &(++a)

>堆栈分配变量时，从上往下分配

`scanf()`
* 如果能够将取得的变量的地址传递给一个函数，能否通过这个地址在那个函数内访问这个变量？
* `scanf()`的原型应该是怎么样的？我们需要一个参数能保存别的变量的地址，如何表达能够保存地址的变量？

指针
* 一个指针类型的变量就是保存地址的变量
  * int i;
  * int* p = &i;   // *表示p是一个指针，指向int i的地址（p的值是i的地址）
  * int* p,q;    // 不论*靠近int还是p，只有 *p表示指针， 没有int *这种东西
  * int *p,q;   // 以上两种，p是指针，**q是普通int**
  * int *p,*q //这样表示两个都是指针

指针变量
* 变量的**值是内存的地址**
  * 普通变量的值是实际的值
  * 指针变量的值是具有**实际值的变量的地址**
  
![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/2.46.10.png)

作为参数的指针
* void f(int *p);
* 在被调用的时候得到了某个变量的地址
  * int i=0; f(&i);
* 在函数里面可以通过这个指针访问外面的这个i

访问那个地址上的变量*
* *是一个单目运算符，用来访问指针的值所表示的地址上的变量
* 可以做右值也可以做左值
  * int k = *p;//读值
  * *p = k+1;//写值
>`*p`一个整体可以看成是一个整数

指针与数组
* 函数参数表中的数组实际上是指针
  * `sizeof(a) == sizeof(int*)`
  * 但是可以使用数组的运算符[]进行计算

数组参数
* 以下四种函数原型是等价的
  * int sum(int *ar, int n);
  * int sum(int *, int);
  * int sum(int ar[], int n);
  * int sum(int [], int);

数组变量是特殊的指针
* 数组变量本身表达地址，所以
  * int a[10]; int *p=a;//无需用&取地址
  * 但是数组的单元表达的是变量，需要用&取地址
  * a == &a[0]
* []运算符可以对数组做，也可以对指针做：
  * p[0] <==> a[0]
* *运算符可以对指针做，也可以对数组做：
  * *a=25;
* 数组变量是const的指针，所以不能被赋值
  * int a[] <==> int* const a=...

指针与const（C99 Only）
* 指针可以是const
* 值可以是const

指针是const
* 表示一旦得到了某个变量的地址，不能再指向其他变量
  * int * const q = &i; //q是const
  * *q=26;//OK
  * q++; //ERROR

所指是const
* 表示不能通过这个指针去修改那个变量（并不能使得那个变量成为const）
  * const int *p=&i;
  * *p=26;//ERROR!(*p)是const
  * i=26;//OK
  * p=&j;//OK
>判断哪个被const了的标志是const在*的前面还是后面

const数组
* const int a[] = {1,2,3,4,5,6};
* 数组的变量已经是const的指针了，这里的const表明数组的每个单元都是const int
* 所以必须通过初始化进行赋值

保护数组
* 因为把数组传入函数时传递的是地址，所以那个给函数内部可以修改数组的值
* 为了保护数组的值不被函数破话，可以设置参数为const
  * int sum（const int a[], int length);

字符串
----

* `char word[]={'H','e','l','l','o','!'};`这不是C语言的字符串，因为不用能字符串的方式做计算  

修改
* `char word[]={'H','e','l','l','o','!','\0'};`

<img src="https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_DC5DE98AF314-1.jpeg" width="300" height="400"/>

字符串
* 以0（整数0）结尾的一串字符
  * 0或'\0'是一样的，但是和'0'不同
* 0标志字符串的结束，但它不是字符串的一部分
  * 计算字符串长度的时候不包含这个0
* 字符串以数组的形式存在，以数组或指针的形式访问
  * 更多的是以指针的形式
* `string.h`里有很多处理字符串的函数

字符串变量
* char *str = "Hello";  //一个指针，指向的内容是Hello，Hello在哪？
* char word[] = "Hello";  // 一个数组，里面的内容是Hello
* char line[10] = "Hello"; //一个大小为10个字节的数组，Hello占据6个字节

字符串常量（字面量）
* "Hello"
* `"Hello"`会被编译变成一个字符数组放在某处，这个数组的长度是6，结尾还有表示结束的0
* 两个相邻的字符串常量会被自动连接起来
* 行末的\表示下一行还是这个字符串常量

* C语言的字符串是以**字符串数组**的形态存在的
  * 不能用运算符对字符串做运算
  * 通过数组的方式可以遍历字符串
* 唯一特殊的地方是字符串字面量可以用来初始化字符数组
* 以及标准库提供了一系列字符串函数

字符串变量

`char* s = "Hello, world!";`
* s是一个指针，初始化为指向一个字符串常量
  * 由于这个常量所在的地方，所以实际上s是`const char* s`，但是由于历史原因，编译器接受不带const的写法
  * 但是试图对s所指的字符串做写入会导致严重的后果
* 如果需要修改字符串，应该用数组：`char s[] = "Hello, world!";`

使用字符串时选择指针还是数组？
* `char *str = "Hello";`
* `char word[] = "Hello";`
* 数组：这个字符串在这里
  * 作为本地变量空间自动被回收
* 指针：这个字符串不知道在哪里
  * 处理参数
  * 动态分配空间

总结：
* 如果要构造一个字符串-->数组
* 如果要处理一个字符串-->指针

`char*`是字符串？
* 字符串可以表达为char*的形式
* char*不一定是字符串
  * 本意是指向字符的指针，可能指向的是字符的数组（就像int*一样）
  * 只有它所指的字符数组有结尾的0，才能说它所指的是字符串

字符串赋值
* `char *t="title";`
* `char *s;`
* `s = t;`
* 并没有产生新的字符串，只是让指针s指向了t所指的字符串，对s的任何操作就是对t做的

字符串的输入输出
* `char string[8];`
* `scanf("%s", string);`
* `printf("%s", string);`
* `scanf`读入一个单词（到空格、tab、回车为止）
* `scanf`是不安全的，因为不知道要读入的内容的长度

安全的输入
* `char string[8];`
* `scanf("%7s", string);`
* 在%和s之间的数字表示最多允许读入的字符的数量，这个数字应该比数组的大小小1
  * 下一次scanf从哪里开始
  * 下一次从读完7位的下一位开始，不再按照空格、tab、回车来确定下一次scanf

常见错误
```c
char *string;   // 只是定义了一个指针，而不是字符串
scanf("%s", string);
```
* 以为char*是字符串类型，定义了一个字符串类型的变量string就可以直接使用了
  * 由于没有对string初始化为0，所以不一定每次运行都出错

空字符串
* `char buffer[100]="";`
  * 这是一个空的字符串，`buffer[0] == '\0'`
* `char buffer[] = "";`
  * 这个数组的长度只有1！

字符串数组示例
* `char **a`
  * a是一个指针，指向另一个指针，那个指针指向一个字符（串）
* `char a[][]`
  * a是一个二维数组，第二个维度的大小不知道，无法编译
* `char a[][10]`
  * a是一个二维数组，a[x]是一个char[10]
* `char *a[]`
  * a是一个一维数组，a[x]是一个char*

字符串函数

标准库中的字符串函数
```c
#include <string.h>
strlen
    size_t strlen(const char *s); // 返回s的字符串长度（**不包括结尾的0**）
strcmp
    int strcmp(const char *s1, const char *s2); // 比较两个字符串，返回
                                                                0: s1=s2
                                                返回的值是差值   >0: s1>s2
                                                               <0: s1<s2
strcpy
    char * strcpy(char *restrict dst, const char *restrict src);
    // 把src的字符串拷贝到dst（restrict表明src和dst不重叠（C99））
    // 返回dst（为了能链起代码）
strcat
    char * strcat(char *restrict s1, const char *restrict s2);
    // 把s2拷贝到s1的后面，接成一个长的字符串，返回s1.（s1必须具有足够的空间）
strchr
strstr
```

安全问题
* strcpy和strcat都有可能出现安全问题
  * 如果目的地没有足够的空间？
* 安全版本
  * `int strncmp(const char *s1, const char *s2, size_t n);`  // 只判断开头n个 
  * `char * strncpy(char *restrict dst, const char *restrict src, size_t n);`
  * `char * strncat(char *restrict s1, const char *restrict s2, size_t n);`
  * 参数`size_t n`告诉函数最多只能过去n个，多了便舍去

字符串中找字符
* `char * strchr(const char *s, int c);` // 寻找**从左到右**第一个出现c的位置，返回的是指针
* `char * strrchr(const char *s, int c);` // 寻找**从右到左**第一个出现c的位置，返回的是指针
* 返回NULL表示没有找到
* ？如何寻找第二个

字符串中找字符串
* `char * strstr(const char *s1, const char *s2);`
* `char * strcasestr(const char *s1, const char *s2);`