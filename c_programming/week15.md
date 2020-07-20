<!--
 * @Date: 2020-07-20 14:52:43
 * @Author: Dai Zhechen
 * @Github: https://github.com/zhechendai
 * @LastEditTime: 2020-07-20 20:04:51
 * @Copyright ©️ 2020 Dai Zhechen. All Rights Reserved.
--> 


文件
====

文件
----

格式化输入输出

* `printf`
  * `%[flags][width][.prec][hlL]type`

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/2.53.55.png)

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/2.57.12.png)

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/2.57.26.png)

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/2.58.55.png)

* `scanf`
  * `%[flag]type`

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/3.04.33.png)

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/3.06.30.png)

printf 和 scanf 的返回值
* `scanf`读入项目数
* `printf`输出的字符数
* 在要求严格的程序中，应该判断每次调用`scanf`或`printf`的返回值，从而了解程序运行中是否存在问题

文件输入输出
* 用`>`和`<`做重定向

一般的方式，做一个file

```c
FILE
FILE* fopen(const char * restrict path, const char * restrict mode);
int fclose(FILE * stream);
fscanf(FILE*, ...)
fprintf(FILE*, ...)
```

打开文件的标准代码
```c
FILE* fp = fopen("file","r");   // r 参数见下表
if( fp ){
    fscanf(fp,...);
    fclose(fp);
}else{
    ...
}
```

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/3.36.57.png)


二进制文件
* 其实所有的文件最终都是二进制的
* 文本文件无非是用最简单的方式可以读写的文件
  * more, tail
  * cat
  * vi
* 而二进制文件是需要专门的程序来读写的文件
* 文本文件的输入输出是格式化的，可能经过了转码

文本 VS 二进制
* Unix喜欢用文本文件来做数据存储和程序配置
  * 交互式终端的出现使得人们喜欢用文本和计算机“talk”
  * Unix的shell提供了一些读写文本的小程序
* Windows喜欢使用二进制文件
  * DOS并不继承和熟悉Unix文化
  * PC刚开始的时候能力有限，DOS的能力更有限，二进制更接近底层
* 文本的优势是方便人类读写，而且跨平台
* 文本的缺点是程序的输入和输出要经过格式化，开销大
* 二进制的缺点是人类读写困难，而且不跨平台
  * int的大小不一致，大小端的问题
* 二进制的优点是程序读写快

程序为什么要用文件
* 配置
  * Unix用文本，Windows用注册表
* 数据
  * 稍微有点量的数据都放在数据库
* 媒体
  * 只能是二进制
* 现实是，程序通过第三方库来读写文件，很少直接读写二进制文件

二进制读写
* `size_t fread(void *restrict ptr, size_t size, size_t nitems. FILE *restrict stream);`
* `size_t fwrite(const void *restrict ptr, size_t size, size_t nitems. FILE *restrict stream);`
* 注意FILE指针是最后一个参数
* 返回的是成功读写的字节数

为什么nitem？
* 因为二进制文件的读写一般都是通过对一个结构变量的操作来进行的
* 于是nitem就是用来说明这次读写几个结构变量

在文件中定位
* `long ftell(FILE *stream);`
* `int fseek(FILE *stream, long offset, int whence);`
  * `SEEK_SET`: 从头开始
  * `SEEK_CUR`: 从当前位置开始
  * `SEEK_END`: 从尾开始（倒过来）

可移植性
* 这样的二进制文件不具有可移植性
  * 在int为32位的机器上写成的数据文件无法直接在int为64位的机器上正确读出
* 解决方案之一是放弃使用int，而是typedef具有明确大小的类型
* 更好的方案是使用文本


位运算
----

详情见[PDF](https://notes.daizhechen.com/file/c_15.2.pdf)