# Java内存分区

1. 程序计数器： **<font color='red'>线程私有</font>**的用于记录Java方法**当前执行到虚拟机字节码的位置**，如果当前运行的是**<font color='red'>native方法就是空</font>**。此块内存区域是唯一一个**<font color='red'>没有任何OutOfMemory异常</font>**的位置。字节码解释器需要按照这个来寻找下一条字节码。

2. Java虚拟机栈：**<font color='red'>线程私有的</font>**和线程生命周期一致的。

   