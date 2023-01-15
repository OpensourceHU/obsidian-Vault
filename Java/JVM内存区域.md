## JVM内存结构

《深入理解Java虚拟机（第2版）》中的描述是下面这个样子的：

![img](https://pic4.zhimg.com/80/v2-abefb713de46f1e6dd241246c0afe263_720w.jpg)

JVM的内存结构大概分为：

- 堆（Heap）：线程共享。所有的对象实例以及数组都要在堆上分配。回收器主要管理的对象。
- 方法区（Method Area）：线程共享。存储类信息、常量、静态变量、即时编译器编译后的代码。
- 方法栈（JVM Stack）：线程私有。存储局部变量表、操作栈、动态链接，对象指针。
- 本地方法栈（Native Method Stack）：线程私有。为虚拟机使用到的Native 方法服务。如Java使用c或者c++编写的接口服务时，代码在此区运行。
- 程序计数器（Program Counter Register）：线程私有。有些文章也翻译成PC寄存器（PC Register），同一个东西。它可以看作是当前线程所执行的字节码的行号指示器。指向下一条要执行的指令。



先看一张图，这张图能很清晰的说明JVM内存结构的布局和相应的控制参数：

![img](https://pic4.zhimg.com/80/v2-8845236d1ab9f22fcc658375967d53fb_720w.jpg)