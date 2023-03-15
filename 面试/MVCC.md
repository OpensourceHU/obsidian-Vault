MVCC Muti-version Concurrent Control多版本并发控制
在并发读的时候 , 利用版本链 在不同的[[事务的隔离级别]]下形成不同的readView

每一行数据有三个隐藏字段

- db_trx_id

    最近修改(修改/插入)`事务ID`：记录`创建`这条记录/`最后一次修改`该记录的`事务ID`。

- db_roll_pointer（版本链关键）

    `回滚指针`，指向`这条记录`的`上一个版本`（存储于rollback segment里）

- db_row_id

    隐含的`自增ID`（隐藏主键），如果数据表`没有主键`，InnoDB会自动以db_row_id产生一个`聚簇索引`。

我们只需要关注trx_id与roll_pointer
undoLog, 一个记录了所有修改操作的反向操作的日志[[MySQL三大日志： redoLog undoLog binLog]]

如果有更新: undoLog里记录了旧值  如果有插入,记录主键值  如果有删除 记录原值

当有update的时候: 

1. 事务修改该行(记录)数据时，数据库会先对该行加排他锁

2. 然后把该行数据拷贝到undo log中，作为旧记录，即在undo log中有当前行的拷贝副本

3. 拷贝完毕后，修改该行name为Tom，并且修改隐藏字段的事务ID为当前事务1的ID,，回滚指针`roll pointer`指向拷贝到undo log的副本记录，即表示我的上一个版本就是它

4. 事务提交后，释放锁

这样 通过`roll pointer`不断地指向undolog中某行记录 遍历这个链表, 相当于可以获得一条版本链

这个版本链上, 不同的记录都有一个trx_id  通过比较trx_id 就可以知道哪些是当前事务可见的,哪些是不可见的 是为ReadView

**ReadView与隔离级别**

在RR的隔离级别下: 当前读的时候, 所有其它事务的修改 当前事务是不可见的 所以不管怎么读, 都是同一个视图

在RC的隔离级别下: 每个快照读会读到最新的ReadView,别的事务的提交可见











------

著作权归@pdai所有 原文链接：https://pdai.tech/md/db/sql-mysql/sql-mysql-mvcc.html
