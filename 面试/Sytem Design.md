
[(306) System Design Interview – Step By Step Guide - YouTube](https://www.youtube.com/watch?v=bUHFg8CZFws)


四个需要考虑的方面
- who   
	  谁在用这个系统
	  这个系统用来干什么的
- scale
	  QPS是多少
	  每次请求数据量多大
- performance
	  读写延迟多少
	  P99 延迟是多少
- cost
	  开发成本
	  维护成本

据此, 将需求拆分成功能需求(functional requirement) 与 非功能需求(non-functional requirement)  
对于功能需求 我们需要设计API了
	设计API:  根据功能  命名  确定入参  出参最后考虑
对于非功能需求
	scale: QPS
	availablity: 单点故障下仍可用
	performance:
需要用CAP理论对其进行取舍, CP还是AP呢  取决于系统的具体场景侧重哪一方面


接下来, 需要考虑整个系统的大致框架是什么了, 这一步比较难着手  不妨从我们要处理的数据开始着手
DB无非是用内存/硬盘来存储数据 以供查询的东西,我们的系统可以简化成这样
![[assets/image-20230117162723067.png]]
典型的CS架构, 分为两个过程  数据的存储与查询


我们要处理的数据需要以哪种方式存储呢?  是独立的还是聚合的?
独立的数据, 如前端埋点 有如下优点
- 方便写入(监听事件,直接往数据库里一塞 打个时间戳...)
- 后期需要时再处理,随便怎么聚合数据 取决于需要
也有如下缺点:
- 数据量会大,对存储不友好
- 数据不够直观(全是事件,很难一眼看出想要的信息)

相比较而言,聚合的数据走向了独立数据的反面
聚合的数据 如每分钟的流量实时监控
优点
- 数据直观
- 存储友好
缺点
- 只能以预先定义好的方式来查数据了
- 不方便定位问题查bug
- 需要数据聚合的流水线 (即流处理, 独立数据只需要批处理即可)
![[assets/image-20230117162703732.png]]


数据库选型:
数据库可以分为SQL与NoSQL, NoSQL的架构也有部分不同: 比如Casandra Hbase MongoDB都有各自的实现与适用的领域
[Sql Or NoSql，看完这一篇你就懂了 - 五月的仓颉 - 博客园 (cnblogs.com)](https://www.cnblogs.com/xrq730/p/11039384.html)





