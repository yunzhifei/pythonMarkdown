# redis 的pipeline 和事务的区别

1. **事务可以保证在有语法错误的情况下事务是原子的** pipe 不具有原子性
2.  Pipe 是批量发送，但是事务是单条发送，然后excu才执行的
3. 执行事务是会阻塞同一个key的其他的命令执行的
4. pipeLine是一个客户端的操作对于redis的服务端是不需要感知的

