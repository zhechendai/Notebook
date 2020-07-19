<!--
 * @Date: 2020-07-19 13:04:32
 * @Author: Dai Zhechen
 * @Github: https://github.com/zhechendai
 * @LastEditTime: 2020-07-19 14:41:39
 * @Copyright ©️ 2020 Dai Zhechen. All Rights Reserved.
--> 

交互图形设计
====

图形程序的终端输入输出
----

* 当做scanf()时，程序是停下来的。
* 停下来的程序是无法在图形界面响应用户的输入
* ？如何像真正的程序一样做交互

函数指针及其应用
----

* `printf("%p\n", &i);`可以打印得到i的地址
* `int a[]={1,2}; printf("%p\n", a);`可以打印得到数组a的地址
* `printf("%p\n", main)`可以打印得到函数main的地址

* C语言中，每个函数都有一个地址，使用那个函数的名字，就可以得到它的地址
* `void(*pf)()=f;`定义了一个名为pf指向函数f的指针

```c
void f(void)
{
    ...
}
void(*pf)(void)=f;   // 定义了一个变量，这个变量表达了函数
printf("%p\n", pf);
(*pf)() // 调用f函数，类同f()
```

函数指针的使用

```c
void f(int i)
{
    printf("in f(), %d\n", i);
}
void g(int i)
{
    printf("in g(), %d\n", i);
}
void h(int i)
{
    printf("in h(), %d\n", i);
}
int main(void)
{
    int i = 0;
    scanf("%d", &i);
    
    // 函数指针的做法
    void(*fa[])(int) = {f,g,h};                 // 只需在这里补充函数名称即可 
    if(i>0 && i< sizeof(fa)/sizeof(fa[0])){
        (*fa[i])(0);                            // 此时与需要调用多少个函数无关 
    }

    // 普通做法
    switch(i)
    {
        case 0:f(0);break;
        case 1:g(0);break;
        case 2:h(0);break;
    }

    return 0;
}
```

```c
int plus(int a, int b)
{
    return a+b;
}
int minus(int a, int b)
{
    return a-b;
}
void cal(int (*f)(int, int))
{
    printf("%d\n", (*f)(2,3));
}
int main(void)
{
    cal(plus);
    cal(minus);       // 程序运行过程中，可以把一个函数像值一样传入另一个函数

    return 0;
}
```

交互图形程序设计
----

回调函数

图形交互信息
* 可以使用回调的消息
* AcLLib中
```c
typedef void(*KeyboardEventCallback)(const char key);
typedef void(*CharEventCallback)(int key);
typedef void(*MouseEventCallback)(int x, int y, int button, int status);
typedef void(*TimerEventCallback)(int timerID);
```

游戏设计思路
----

mvc设计思路
* view：从model中得到数据，然后画图
* model：从control中感知数据
* control：感知鼠标键盘等变化，传数据给model