# mysql 服务器性能剖析

 	1. 剖析服务器负载
     - 开启慢查询日志，对IO消耗不大，但是对磁盘存储比较大，可以开启日志转轮工具通过日志可以找到我们需要优化的慢查询
 	2. 剖析单条查询
     - show profiles 默认关闭可以链接动态修改的
     - show status 
     - 检查慢查询日志
 	3. 

