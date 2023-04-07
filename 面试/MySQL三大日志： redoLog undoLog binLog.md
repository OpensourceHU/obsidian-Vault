Ref: [mysql45讲--日志系统](https://funnylog.gitee.io/mysql45/02%E8%AE%B2%E6%97%A5%E5%BF%97%E7%B3%BB%E7%BB%9F%EF%BC%9A%E4%B8%80%E6%9D%A1SQL%E6%9B%B4%E6%96%B0%E8%AF%AD%E5%8F%A5%E6%98%AF%E5%A6%82%E4%BD%95%E6%89%A7%E8%A1%8C%E7%9A%84.html)

## undoLog
回滚与MVCC做版本链时有用, 本质是将上一条操作的反向操作给记录下来

## redoLog
Mysql是`写前日志`   所以redoLog可以当作写缓存(用两个指针实现的循环队列)
redoLog的另一个作用是 保证crashSafe

写缓存: 减少磁盘IO, MySQL使用写前日志Write Ahead Logging 先写日志再写磁盘, 所以相当于日志是个buffer, 更新一般在系统空闲时/redoLog满时(即首尾指针相交)进行落盘
`innodb_flush_log_at_trx_commit`这个参数设置为1的话, 每次事务提交redolog会进行刷盘, 建议设置为1 以保证安全

## binlog
在server端的日志, 以二进制的形式记录了数据行的变化, 可以用来做主从同步等