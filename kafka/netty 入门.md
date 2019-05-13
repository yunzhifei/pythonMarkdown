# netty 入门

第一步了解IO多路复用：

 1. 支持一个进程打开的FD(文件描述符)，仅受系统最大文件句柄数限制。

    select 默认最大限制是1024，这个值可以通过修改宏编译内核来解决，不过会降低网络效率。epoll可以打开的文件描述符数量不收限制。

 2. I/O效率不会随着FD数目增加而线性降低。

    select  和 epoll每次都会线性扫描全部的文件描述符。epoll不存在这个问题。只有活跃的描述符才会触发回调函数。

	3. 使用mmap加速内核和用户空间下次传递。

    epoll通过用户空间和内核空间mmap 公共用一块内存实现。

	4. epoll的API更简单
    ![image-20190508104927259](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190508104927259.png)



第二步了解什么是NIO 和NIO的组成部分

	1. 缓冲区buffer,缓冲区实质上是一个数组。所有的NIO操作都是对缓冲区操作的。
 	2. 通道channel，通道类似之前的inputstream 和outputstream 只不过 channel是全双工的。
 	3. 多路复用器selector多路复用器提供选择已经就绪的任务的能力。