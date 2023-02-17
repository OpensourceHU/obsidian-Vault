
>MYSQL锁机制Ref:[MySQL :: MySQL 8.0 Reference Manual :: 15.7.1 InnoDB Locking](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html#innodb-shared-exclusive-locks)

根据锁的粒度来划分： 行锁  or  表锁

根据锁的类型来划分： 排他锁（X锁 exclusive lock写锁） 共享锁（S锁 share lock 读锁） 意向锁（理解成一种标志位就好  表示在更细的粒度上有锁）