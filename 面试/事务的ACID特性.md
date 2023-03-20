 #事务/ACID 

Ref: [ACID - Wikipedia](https://en.wikipedia.org/wiki/ACID)

>Transactions are not a law of nature; they were created with a purpose, namely to simplify the programming model for applications accessing a database. By using transactions, the application is free to ignore certain [potential](https://www.zhihu.com/search?q=potential&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A362597203%7D) error scenarios and concurrency issues, because the database takes care of them instead (we call these safety guarantees).

事务并不是天然就有的, 我们引入这个概念, 是为了简化编程时访问数据的模型, 通过使用事务, 应用程序可以无视掉潜在的错误场景和并发问题, 因为数据库自身代替程序员来保证它们.( 我们称之为安全保证)

**A: Atomic 原子性**, 事务中所有的语句 要不然全部成功, 要不然全部失败(回滚)
**C: Consistency 一致性**,  保证数据库是从一个合法状态迁移到另一个合法状态, 通过实现AID来实现
**I: Isolation 隔离性** 确保数据库在事务并发执行的情况下 结果与顺序执行相同 , 主要由隔离级别来确认
**D: Durability 永久性**  永久性保证事务一旦提交则不会更改, 即便系统崩溃

这里要注意, 一致性 Consistency这个概念 在
[[事务的ACID特性]]
[[系统设计/负载均衡术#一致性哈希分区]]
[[CAP theorem]] 
这几个场景下, 会有一词多义的现象, 具体是什么意思需要联系上下文来确定