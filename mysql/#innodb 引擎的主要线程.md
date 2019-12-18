# innodb 引擎的主要线程

1. master thread
   1. 负责把缓冲池中的数据，异步刷新到磁盘保证数据的一致性，包涵脏页刷新，合并插入缓冲。回收undo页等。
2. I/O thread 
   1. read write log thread insert buffer 等读线程一般是小于写线程的。
3. purge thread 
   1. 主要用于回收undo页面。1.1以后可以配置多个purge线程
4. page cleaner thread 
   1. 主要是负责脏页的刷新，来减轻master thread线程的工作量。



# 内存池

1. 1.0.x 可以有多个内存池，每个页面平均分配到不同的缓冲池。来降低内部的资源竞争
2. LRU list free list flush list
3. redolog的日志会  
   1. 一秒刷新到磁盘一次
   2. 事务提交的时候刷新一次
   3. 当日志空间不足一半的时候刷新一次
4. 使用write ahead log来先写redolog保证数据不丢失，然后再写入数据页。可以保证持久性



# 两次写

1. ![image-20191218104827553](/Users/yunzhifei/IdeaProjects/pythonMarkdown/images/image-20191218104827553.png)



# 检查点check point

1. 缩短数据库的恢复时间
2. 缓冲池不够用的时候，脏页刷新到磁盘
3. redolog不可用的时候，刷新脏页
4. check point的种类
   1. sharp point 数据库关闭的时候所有的脏页都刷新到磁盘
   2. fuzzy check point 只在运行的时候刷新部分脏页到磁盘上去
      1. fuzzy 发生的时机
         1. master thread check point 
         2. flush lru list  checkpoint
         3. async sync check point  redolog 不可用的情况
         4. dirty page too much check point



# 自适应hash 索引

1. 访问的条件应该是一样的。
   1. 用该模式访问超过100次
   2. 页通过该模式访问了N次 N= 记录/16



# 异步IO

1. 合并IO 
2. 刷新临近页





