---
title: AQS与ReentrantLock
---



## 什么是AQS

abstract queueed synchronizer 抽象队列同步器。位于JUC下，是一个抽象类

以下都是基于AQS实现的，是一个JUC的基础类

- ReentrantLock（可重入锁）
- ReentrantReadWriteLock（读写锁）
- CountdownLatch
- Semaphore（信号量）
- Worker（ThreadPool下）

AQS的主要使⽤⽅式是继承，⼦类通过继承AQS并实现它的抽象⽅法来管理同步状态，同步器的
设计基于模板⽅法模式，所以如果要实现我们⾃⼰的同步⼯具类就需要覆盖其中⼏个可重写的⽅
法，如tryAcquire、tryReleaseShared等等。  

设计这样一个基础类需要考虑的事情：

1. 要做到通用，抽象出所有需要用的方法
2. 利用CAS，原子地修改竞争资源的标志位（一个资源能被几个线程使用需要一个标识）
3. 阻碍其它线程的调用，需要设计一个等待队列
    - 有的线程只是尝试一下获取对象，获取不到就算了
    - 有的线程就是想要获取到对象，获取不到它宁愿等待

## ReentrantLock

ReentrantLock基于AQS，在并发编程中可以实现公平锁与非公平锁来对资源进行同步，同时与synchronized一样，reentrantLock支持可重入。除此之外ReentrantLock在调度上更加灵活