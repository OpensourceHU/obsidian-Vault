Unicode是一种统一码制, 世界上各国语言文字不同  需要一套统一的码值来表示所有的文字 Unicode由此诞生
但是UTF8只是规定了`码制` 给每一个字符一个全局唯一的编码(Unicode嘛 unique code)

但是UTF8定义了这个Unicode在计算机中如何存储, 相当于Unicode给出了标准 UTF8给出了实现
UTF8全称 Unicode Transformation Format-8-Bit, 建名知意: 将Unicode转化为8位(1字节)码
是一种兼容ASCII的编码格式, 并且是一种边长的编码格式, 每个字符的编码后长度为1~4字节
