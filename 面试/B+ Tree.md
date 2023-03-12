>B-tree
>
>A tree data structure that is popular for use in database indexes. The structure is kept sorted at all times, enabling fast lookup for exact matches (equals operator) and ranges (for example, greater than, less than, and `BETWEEN` operators). This type of index is available for most storage engines, such as [`InnoDB`](https://dev.mysql.com/doc/refman/8.0/en/innodb-storage-engine.html) and [`MyISAM`](https://dev.mysql.com/doc/refman/8.0/en/myisam-storage-engine.html).
>
>Because B-tree nodes can have many children, a B-tree is not the same as a binary tree, which is limited to 2 children per node.
>
>Contrast with ***\*hash index\****, which is only available in the [`MEMORY`](https://dev.mysql.com/doc/refman/8.0/en/memory-storage-engine.html) storage engine. The `MEMORY` storage engine can also use B-tree indexes, and you should choose B-tree indexes for `MEMORY` tables if some queries use range operators.
>
>The use of the term B-tree is intended as a reference to the general class of index design. B-tree structures used by MySQL storage engines may be regarded as variants due to sophistications not present in a classic B-tree design. For related information, refer to the `InnoDB` Page Structure [Fil Header](https://dev.mysql.com/doc/internals/en/innodb-fil-header.html) section of the [MySQL Internals Manual](https://dev.mysql.com/doc/internals/en/index.html).
>
>See Also [hash index](https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_hash_index).