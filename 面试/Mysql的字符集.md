记住结论 : 一般情况下Mysql建表的时候选 **`utf8mb4`** 即可
如果使用默认的`utf8` 则emoji和一些繁体字是存不下的 需要四个字节, 会报错
但如果使用`utf8mb4`则不会, 这个字符集的意思是 utf8编码 最大4字节(mb4: max byte 4)

简单的介绍下
[[Unicode与UTF8]]