#redis #面试总结
参考书籍: [<<Redis设计与实现>>](https://github.com/7-sevens/Developer-Books/blob/master/Redis/Redis设计与实现.pdf)

## 基础知识部分:

1. redis的数据结构部分, 即对象设计与底层数据结构的实现
[[Redis/Redis的数据结构与编码方式]]
[[redis为什么使用跳表而不是红黑树]]

2. Redis如何作分布式缓存系统:
[[Redis/Redis集群]]

3. Redis的持久化问题:
[[RDB与AOF持久化]]

4. redis的键过期策略与删除策略
[[redis过期策略与删除策略]]

5.redis为什么快
[[Redis/Redis为什么快]]

## 实际应用中的问题:
[redis内存雪崩\内存击穿\缓存穿透](https://segmentfault.com/a/1190000022029639)
[Redis大key与热key问题](https://help.aliyun.com/document_detail/353223.html)
[[缓存数据一致性问题]]
