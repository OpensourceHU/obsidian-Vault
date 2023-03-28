[很直观的动画演示: Raft ](https://thesecretlivesofdata.com/raft/)
## 问题引入-- 什么是分布式一致性问题?
[Raft intro](https://thesecretlivesofdata.com/raft/#intro)
假如说我们有一个单点系统(这个例子中, 你可以想象为只存储了一个值的DB服务器)
我们同时有一个可以向服务器发送值的客户端
这种情况下, 保持数据一致是非常简单的(只有一台实例)

但是如果我们的系统中有多个节点呢?如何维护这多个节点间数据的一致性, 并对外(对客户端) 展示一个统一的视图呢?
这个问题即所谓**分布式一致性问题(distributed consensus problem)**

## overView
[Raft overView](https://thesecretlivesofdata.com/raft/#overview)
Raft是一种基于节点选举, 事件传播的一致性算法
网络中有三类节点
1. **follower**: 相当于从节点, 拥有投票资格
2. **candidate**: 候选节点, 从节点到主节点的中间态. 它需要获取网络中大多数follower节点(超过一半)的支持,才能升级为leader节点
3. **leader**: 相当于主节点

Raft协议将一致性问题拆解成了三个部分
1. 节点选举 Leader Election ---- 解决谁来当主节点 充当权威数据源的问题
3. 日志复制问题 Log replication  ---- 解决主从节点间数据同步的问题
4. 安全性问题 Safety  ---- 解决在集群分裂后依旧保持一致性的问题


## 节点选举问题
[Leader election](https://thesecretlivesofdata.com/raft/#election)
1. 所有节点初始化为follower  如果follower无法监听到来自leader的消息, 则可以票选自己为candidate
2. candidate向其它follower拉选票(当然, 这里的follower只要有来自candidate的vote request就会直接响应)
3. 如果一个candidate收获了这个网络中大多数节点的选票, 则可以晋升为leader节点
这个过程称为节点选举

## 日志复制问题
[log replication](https://thesecretlivesofdata.com/raft/#replication)
刚才, 我们选举出了leader节点
现在, 对系统的所有更改 都要经由leader了 这里涉及到[[2PC保证强一致性]]
1. 每当有客户端想要修改value的值时, 首先将这条命令记录成一条日志项
注意,此时这条日志还没有提交生效, 所以value的值还未发生改变
2. leader将这条日志广播给所有follower
3. follower记录这条日志, 并回给leader一个确认消息: 更新日志已收到, 现在处于待提交状态
4. leader等待大多数的follower在本地写入这条日志了,再将value的值改变, 并通知给所有follower, 日志可以正式被写入了
5. 所有follower正式提交这条日志, 节点同步完成
这个过程称为日志复制过程

## 安全性问题
当集群发生了分裂, 典型的, 比如网络问题隔断了某几个节点一段时间, 集群如何保证可用, 并在网络恢复后保证集群数据的最终一致性?
