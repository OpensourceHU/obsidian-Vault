Ref: [mysql45讲--日志系统](https://funnylog.gitee.io/mysql45/02%E8%AE%B2%E6%97%A5%E5%BF%97%E7%B3%BB%E7%BB%9F%EF%BC%9A%E4%B8%80%E6%9D%A1SQL%E6%9B%B4%E6%96%B0%E8%AF%AD%E5%8F%A5%E6%98%AF%E5%A6%82%E4%BD%95%E6%89%A7%E8%A1%8C%E7%9A%84.html)

## undoLog
回滚与MVCC做版本链时有用, 本质是将上一条操作的反向操作给记录下来

## redoLog
Mysql是写前日志 redoLog当作写缓存(用两个指针实现的循环队列) 与 保证crashSafe

## binlog
在server端的日志, 以二进制的形式记录了数据行的变化, 可以用来做主从同步等