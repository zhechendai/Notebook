<!--
 * @Date: 2020-07-15 15:00:52
 * @Author: Dai Zhechen
 * @Github: https://github.com/zhechendai
 * @LastEditTime: 2020-07-15 15:28:43
 * @Copyright ©️ 2020 Dai Zhechen. All Rights Reserved.
--> 

ACLLib入门
====

非正式课程要求

Windows API
* 从第一个32位的Windows开始就出现了，就叫做Win32API
* 它是一个纯C的函数库，就和C标准库一样，使你可以写Windows应用程序
* 过去很多Windows程序是用这个方式做出来的

main()?
* main()成为C语言的入口函数其实和C语言本身无关，你的代码是被一段叫做启动代码的程序所调用，它需要一个叫做main的地方
* 操作系统把你的可执行程序装载到内存里，启动运行，然后调用到你的main函数

ACLLib结构
* WinMain()
  * 初始化窗口结构并注册给Windows OS
  * 调用你的setup()
  * 跑一个无限循环来读入并处理Windows消息
* 当有用户动作发生的时候，调用你的回调函数来处理

![](https://gitbook-daizhechen.oss-cn-hangzhou.aliyuncs.com/notesstack/IMG_CDCD70FA1105-1.jpeg)
