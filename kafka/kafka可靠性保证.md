# kafka可靠性保证

 ## kafka有以下保证：

1. kafka保证分区消息顺序，如果同一个生产者向同一个分区中写入消息先写入消息A，然后写入消息B，kafka保证B的偏移量比A的大。也就是说明，消费者会小读到消息A，然后读到B。
2. 只有当消息写入全部同步副本以后才会算是已提交。(客户端可以认为根据不同的ack参数来判断是否提交成功了)
3. 只要有一个副本还是活跃的，消息就不会丢失。
4. 消费者只能读到已经提交的消息。(这里的已提交是broker的认为的已提交，不是client的已提交)

## kafka的复制

​	kafka的主题被分为多个分区，分区是基本的数据块。

1. 同步副本的前提：和zookeeper之间有一个活跃的会话(过去的6s内可以配置)，就是发送过心跳
2. 过去的10S内从首领哪里获取过最新的消息。
3. 在过去的10S内从首领那里获取最新消息是0延迟的



# broker会影响消息存储的可靠性的三个参数

## 1、kafka的复制系数

​	默认复制系数是3

##  2、不完全的首领选举

​	unclean . leade .selecti.on（只能在broker级别配置）实际上是集群级别。默认是true。就是说当首领副本挂掉的时候，如果新的同步副本成为了新的首领副本就是完全的首领选举。但是如果所有的副本都是不同步的呢。就是首领副本不可用的时候，如果没有可用的同步副本，就必须做出选择是否让非同步副本成为新的首领。比如我们配置了复制系数是3， 除了首领副本以外，其他的副本可能在同步消息，但是不一定是同步的。

## 3、最小同步副本数

​	这个参数可以保证当出现同步副本数量大于等于这个参数的时候，才可以向kafka写入数据。出现的同步副本数量如果低于这个数值，client就会收到一个异常。NotEnoughRepli.casExcepti.on 但是不影响消费者读取数据。

