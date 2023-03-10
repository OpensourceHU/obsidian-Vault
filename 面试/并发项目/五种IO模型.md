准备知识:

Linux的思想是万事万物皆文件, 对于文件 有file descriptor-fd来描述,写过C语言IO的都记得, 对待读写文件调用相应的系统命令后 会返回两个fd fd[0]读取 fd[1]写入; 对于socket而言 同样通过fd来控制, 称为socket fd


### 阻塞:

recevfrom函数不断地读取内核, 数据没准备好就一直等待 直到复制完成



### 非阻塞:

recevfrom函数读取内核时, 系统立刻返回一个数据是否准备就绪的信号: EWOULDBLOCK
通过这个信号判断数据是否就绪  未就绪则一直轮询



### IO多路复用:

Linux提供了 select/poll/epoll三种IO多路复用模型, 简单的说就是让请求阻塞在复用器上(selector)

其中 select与poll底层一个是数组 一个是链表; 将待读写的socket fd注册到selector上 形成一个注册表,不断遍历这个注册表,就知道哪些socket的IO就绪了(轮询)

其中select可注册的fd是有限的 (默认是1024可以重新编译FD_SETSIZE这个宏来重置) poll理论上是无限的

两者的缺点显而易见, 随着注册的socket fd线性增加, 遍历的耗时也线性增加  最终Linux改用了epoll作为替代

epoll有以下好处		

- 注册的socket fd不受限制: 严格的说 是OS所支持的最大文件句柄数, 反正是远大于1024且绝对够用的 服务器1G内存大概对应10万个句柄
- IO效率不会随着注册的fd数目线性下降: epoll只会对活跃的fd进行操作, 具体是通过监听fd上的callBack函数是否活跃来实现的
- mmap加速读写: 每次读写需要在内核与用户空间来回拷贝数据, epoll可以将用户态内存与内核态内存 通过内存映射(mmap)指向同一块物理内存 来加快读写速度(零拷贝的底层)

epoll的底层是链表+红黑树

三者具体的实现与差异  参考<<UNIX网络编程>>



### 信号驱动IO

首先要打开socket的信号驱动IO功能,每次通过sigaction来读取数据(系统调用立即返回) 并在数据就绪时(即从网卡拷贝到内核后)通过SIGIO信号通知调用方, 使用recevfrom函数来接收



### 异步IO

和信号驱动IO大致相同,  不同的是通知的时机不再是 网卡->内核 拷贝完成  而是 网卡->内核->用户空间 拷贝完成, 即整个IO过程已经完成的时候才通知;   与之相对的, 信号驱动IO是在数据就绪可以读取时通知

ref
[<< Netty权威指南>>](https://github.com/yyxyz/Book/blob/master/Netty权威指南%20第2版.pdf)
