## 分词与语法解析

**Ruby需要读取并变换多少次才能运行代码？**

答案是三次。

1. tokenize 分词 - 读取文本文件中的文本字符，将其转换为词条

2. parse 解析 - 对词条进行语法解析

3. compile 编译 - 变成底层指令，运行在Ruby虚拟机上

Ruby虚拟机叫做 Yet Another Ruby Virtual Machine(YARV)

