# Java内存分区

1. 程序计数器： **<font color='red'>线程私有</font>**的用于记录Java方法**当前执行到虚拟机字节码的位置**，如果当前运行的是**<font color='red'>native方法就是空</font>**。此块内存区域是唯一一个**<font color='red'>没有任何OutOfMemory异常</font>**的位置。

2. Java虚拟机栈

   