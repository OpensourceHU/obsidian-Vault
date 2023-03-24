可以参考这篇文章: [RDB与AOF持久化](https://pdai.tech/md/db/nosql-redis/db-redis-x-rdb-aof.html)

需要注意的点:
RDB(Redis DataBase)
1. fork一个子进程出去给当前数据库做一个全量的快照, 其实是很"重度"的操作, 体现在fork时的系统调用与数据库KV全量复制的耗时
2. RDB文件是二进制的, 很紧凑, 便于读取与崩溃恢复

AOF(Append On File)
1. 以文本地方式记录每一条指令, 然后通过重做指令达到恢复的效果
2. 为了保证性能, 指令也不是直接追加到文件尾部的, 而是有一个AOF buffer, 可以选择刷盘的时机
   1. always: 每条指令都落库
   2. every Sec: 每秒刷盘一次
   3. No: 交由操作系统决定何时刷盘
      我们一般采用第二种, EverySec 这样兼顾了性能和可用性, crash后最多损失1S内的数据 是可以接受的
