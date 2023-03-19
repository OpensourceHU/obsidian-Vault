#一致性问题 #mysql
2PC即 two phase commit, 两阶段提交
MySQL使用的是写前日志, 即先写日志再写DB, 这里的日志有

1. 保证crashSafe的redolog
2. 用于主从间同步的binlog
详见[[MySQL三大日志： redoLog undoLog binLog]]
所谓的保证强一致性, 是指主从间保证一致性
如果事务成功, 主从间理当同步
如果事务失败, 应当主库回滚, binlog不记录不记录, 来维护主从同步

看下面这张图
![[assets/image-20230318120623656.png]]

如果在写redo log成功, 还没写bin log的时刻crash掉
那么, 主库可以通过redo log回滚, 保证crash safe

如果在写redo log成功 写binlog 也成功, 但是数据还没有落库, 即事务还没有提交 之前的时刻, 数据库crash掉
那么,  主库与从库都可以通过binlog 重做, 来保证数据的一致性 ,相当于这条事务成功.