ABA问题常见于各种乐观锁的实现

再回顾下基于CAS实现乐观锁的过程:
1. 取出oldValue
2. 基于oldValue计算出newValue
3. 再看DB中, 依然是oldValue则认为这段时间没有发生冲突 CAS地换入newValue

问题出在第三步, 依然是oldValue则这段时间没有修改吗? 
有没有一种可能,  其它进程将oldValue修改了两次
第一次 A->B  第二次 B->A  只是刚好数值上 新值与老值相等

所以每次修改需要引入一个版本号, 版本号自增
每次检查时, 不但检查oldValue是不是预期值, 还要检查版本号version == version+1