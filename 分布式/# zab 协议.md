# zab 协议

1. zab 协议要求保证那些已经被**leader提交的事务最终被所有的服务器提交。**
2. zab协议要求那些在leader提出的但是**还没有被提交的的事务被丢弃**



## zab协议的作用

1. 使用一个单一的进程 leader来接收并处理客户端的事务请求，并采用原子广播协议将服务器数据的变更以事务的形式广播到所有的副本(follower)上去。
2. 保证全局的变更序列被顺序引用(通过事务ID来实现)
3. 当主进程异常的时候，还可以继续工作



## zab协议的原理

​	zab 协议要求leader都要经历三个阶段如下：

	1. **发现：**要求zookeeper集群必须选举出一个leader，然后维护一个follower列表，将来客户端可以和这些follower通信
 	2. **同步:**   leader 需要负责将本身的数据同步到follower上面去
 	3. **广播:**   leader可以接受客户端提出的新的事务请求。



## zab协议的核心

1. 定义了事务请求的处理方式
   1. 所有的事务都必须通过全局唯一的服务器来协调处理。 这个就是leader，其他的都是follower服务器
   2. leader负责把客户端的请求，转换成一个**事务Proposal**，并广播给所有的follower。
   3. leader在分发请求以后，需要等待半数以上的follower的ack，如果有半数以上的回应，就会发出commit请求。



## zab 协议的内容

1. 崩溃恢复
   1. 因为zab的协议要求，没提交的事务要丢弃，所以已经提交的事务必须都保留。这样就必须保证新的leader不可以有未提交的事务
      ，而且新的leader必须有最大的事务ID，也就是ZXID
2. 消息广播

## zab协议通过epoch 来区分选举周期

1. 选举的时候要求周期最大，然后是zxid最大，然后是server ID最大，