# kafka的broker基本配置

	1. broker.id 默认是0集群唯一
 	2. port 监听的端口默认9092
 	3. zookeeper.connect保存元数据的zk
 	4. log.dirs kafka的所有消息都会保存到磁盘上。如果有多个文件系统路径broker会遵循最新少分区数量原则 
 	5. num.recovery .threads.per . data.dir 这个代表着每一个路径在的并发处理线程数量。
      - 服务器正常启动，用于打开每个分区的日志片段 g
      - 服务器崩愤后重启，用于检查和截短每个分区的日志片段：
      -  服务器正常关闭，用于关闭日志片段。
	6. auto.c reate .topics.enable 自动创建主题的时机，当然也可以显示的手动创建了
    	1. 当生产者开始写入消息
    	2. 当消费者开始拉取消息
    	3. 当有任意客户端来请求元数据消息的时候。
	7. kafka现在支持的主题的参数可以覆盖服务器配置的参数。
    	1. num.partitions 多少个分区
    	2. log.segment.bytes 片段的大小
    	3.  message.max.bytes 单个消息的大小默认是1M
	8. 需要多少个broker 应该考虑主要是两个问题
    	1. 需要多少个broker来保存数据
    	2. 集群的处理能力，单个broker的网卡处理能力
	9. 