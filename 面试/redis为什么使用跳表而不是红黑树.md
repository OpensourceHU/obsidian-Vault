看看开发者怎么说: 

>there a few reasons:
>
>\1) They are not very memory intensive. It's up to you basically. Changing parameters about the probability of a node to have a given number of levels will make then *less* memory intensive than btrees.
>
>\2) A sorted set is often target of many ZRANGE or ZREVRANGE operations, that is, traversing the skip list as a linked list. With this operation the cache locality of skip lists is at least as good as with other kind of balanced trees.
>
>\3) They are simpler to implement, debug, and so forth. For instance thanks to the skip list simplicity I received a patch (already in Redis master) with augmented skip lists implementing ZRANK in O(log(N)). It required little changes to the code.


1. 跳表在内存占用上较红黑树更优,  取决于节点的level属性晋升概率
2. redis中常见的遍历操作, ZRANGE与ZRERANGE, 这一点上跳表的表现比红黑树好或者至少同样好
3. 跳表易于实现, 且容易修改