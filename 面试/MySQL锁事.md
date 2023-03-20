
>MYSQL锁机制Ref:[MySQL :: MySQL 8.0 Reference Manual :: 15.7.1 InnoDB Locking](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html#innodb-shared-exclusive-locks)

根据锁的粒度来划分： 行锁  or  表锁  意向锁

根据锁的类型来划分： 排他锁（X锁 exclusive lock写锁） 共享锁（S锁 share lock 读锁） 意向锁（理解成一种标志位就好  表示在更细的粒度上有锁

根据设计思路:  乐观锁   悲观锁   这里需要注意的是, 乐观锁是需要程序员自己实现的(版本号)

innoDB为了解决幻读: 临键锁


## 如何解决死锁
1. 谁上锁谁释放, 超时主动释放
2. Wating for graph, 深搜判断有无依赖成环 有则返回一个错误不允许事务创建