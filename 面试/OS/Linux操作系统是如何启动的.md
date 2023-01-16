装过机的可能会对这一过程有更直观的认识, 至少知道 MBR / Boot Sequence等概念 看过主板上的故障灯   今天做一个简单的总结
更全面的 可以参考 <<鸟哥的Linux私房菜>> 19章   [鸟哥的 Linux 私房菜 -- 启动关机流程与 Loader (vbird.org)](http://cn.linux.vbird.org/linux_basic/0510osloader_1.php)

### **1、POST (Power On Self Test 开机自检,也可以叫上电自检** 
linux开机加电后，系统开始开机自检，该过程主要对计算机各种硬件设备进行检测，如CPU、内存、主板、硬盘、CMOS芯片等，如果出现致命故障则停机 如果出现一般故障则会发出声音等提示信号，等待故障清除；若未出现故障，加电自检完成。
![[assets/Pasted image 20230112235023.png]]
这图就是主板上的故障灯,出自硬件茶谈的"装机圣经"
[【装机教程P14】接驳显示器和电源线_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Ze411A7u5/?spm_id_from=333.788&vd_source=3adda9cad978d53fdf6c33bd02d60d9b)


### **2、上电自检完成，查找可启动设备，加载主引导目录（MBR--Master Boot Record)** 
开机自检完成后，CPU首先读取位于CMOS中的BIOS程序，按照BIOS中设定的启动次序（Boot Sequence)逐一查找可启动设备, 找到可启动的设备后，去该设备的第一个扇区 中读取MBR(后来的GPT更先进一些 为了方便描述统一称作MBR吧)


### **3. MBR加载内核文件**
那么MBR是什么哪？它又有什么作用？ [主引导记录 - 维基百科](https://zh.wikipedia.org/wiki/%E4%B8%BB%E5%BC%95%E5%AF%BC%E8%AE%B0%E5%BD%95)

MBR存在于可启动磁盘的0磁道0扇区，占用512字节，就是每个磁盘的最开始位置的一小块区域  它主要用来告诉计算机从选定的可启动设备的哪个分区来加载引导加载程序(Boot loader)以及存放分区表与分区有效性标记位
在系统启动中, 只需要知道MBR存放了启动代码,现在常用的一般是GRUB

GRUB(一种Boot Loader)会先加载**用来识别文件系统的文件**  加载后就有了**文件系统**
有了文件系统后, 会直接找/boot目录里的内核文件 参考[[Linux根目录结构]]  将内核文件加载

### **4.内核完成初始化**
GRUB把内核加载到内存后展开并运行， 此时GRUB的任务已经完成，接下来内核将会接管并完成 
探测硬件–>加载驱动–>挂载根文件系统–>切换至根文件系统（rootfs）–>运行/sbin/init完成系统初始化(这个进程PID为1)

这个时候系统就会自动地 挂载设备,显示硬件,初始化RAID 等等 最后显示login界面 Linux系统就正常启动了.