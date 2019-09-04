# zookeeper ZAB协议选主过程

1. 选主时机
   1. 启动的时候 所有的节点都是 LOOKING状态
   2. leader节点死亡的时候。leader启动以后是定期向从节点发送心跳，当从节点收不到心跳的时候从FOLLOWING变成LOOKING
   3. 多个follower节点进入异常状态态。