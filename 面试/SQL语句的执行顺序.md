SQL不同于其他编程语言的最明显特征是处理代码的顺序。在大多数据库语言中，代码按编码顺序被处理。但在SQL语句中，第一个被处理的子句式FROM，而不是第一出现的SELECT。SQL查询处理的步骤序号：

(1) FROM <left_table> 选表

(2) <join_type> JOIN <right_table> 连接表

(3) ON <join_condition> 连接条件

(4) WHERE <where_condition>  where子句

(5) GROUP BY <group_by_list>  分组

(6) WITH {CUBE | ROLLUP} 生成中间表

(7) HAVING <having_condition> 组过滤条件

(8) SELECT  投影, 即选择结果中的字段

(9) DISTINCT 去重

(10) ORDER BY <order_by_list> 排序

以上每个步骤都会产生一个虚拟表，该虚拟表被用作下一个步骤的输入。这些虚拟表对调用者(客户端应用程序或者外部查询)不可用。只有最后一步生成的表才会会给调用者。如果没有在查询中指定某一个子句，将跳过相应的步骤。