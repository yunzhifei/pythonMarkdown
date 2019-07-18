# ReentrantLock 可重入锁 
实现接口lock的一种可以重入的锁，
1. 使用过程
```
a Lock lock = new ReentrantLock();// 锁  
b lock.lock(); //获取锁 2
/**
* 业务逻辑，保证只有一个线程同时执行
**/
c lock.unlock() //释放锁  3
```
2. 内部结构

   ![ReentrantLock](https://static.oschina.net/uploads/img/201611/08104004_m0XL.jpg)

3. AQS和FairSync 和Sync NonfairSync的类图关系
   ![ Sync extends AbstractQueuedSynchronizer](https://static.oschina.net/uploads/img/201611/08105134_LRJ5.jpg)
   由图上可以知道公平锁和非公平锁都是继承同步队列，然后继承了AQS来实现的。

4. 初始化默认锁是非公平锁
   ![image-20190717153418810](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190717153418810.png)


   ​	可以通过有参数构造来指定公平锁：

   ![image-20190717153535033](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190717153535033.png)

5. 获取锁的流程
       公平锁的流程

    ![image-20190717153625003](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190717153625003.png)
   	非公平锁的流程
   ![image-20190717153703109](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190717153703109.png)

   发现非公平锁的差距是会先通过cas获取一下锁，如果获取失败了，才会去进入锁的队列中。
   ![image-20190717154211068](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190717154211068.png)
   cas 的代码是本地方法，看不到具体实现。

6. AQS的基本架构

   - Node 节点
   - head 头节点
   - tail 尾部节点
   - state 当前锁状态（共享和独占锁是不一样的)
   - acquire 尝试获取锁
   - acquireQueued 获取锁队列
   - addWaiter 加入等待队列
   - release 释放锁
   - unparkSuccessor唤醒继任节点
   - ConditionObject 条件对象 类似 wait /notify

   ![AQSæ¶æ](https://static.oschina.net/uploads/img/201611/08113748_Wf0t.jpg)
   state是关键所在volatile int state;用volatile修饰。state是0表示锁空闲可以获取。独占锁表示重入的次数。共享锁表示共享线程数，![image-20190717160805801](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190717160805801.png)

7. Acquire 内部实现

   	1. tryAcquire 先尝试获取锁
    	2. addWaiter 加入队列并放在队尾
    	3. acquireQueue 尝试从队列中获取锁同样也会先尝试tryAcquire
    	4. selfInterrupt 如果被中断就直接中断
    	5. ![image-20190717161445610](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190717161445610.png)

8. 公平锁和非公平锁最大的区别在于tryAcquire 的实现

   公平锁的tryAcquire 实现：

   ![image-20190717163220588](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190717163220588.png)	

   这里可以看到 首先获取了锁的状态，如果锁是空闲的，就开始判断有没有前置节点（hasQueuedPredcessors），如果没有前置节点开始尝试cas设置，成功了就设置当前线程是自己。

   如果发现当前线程是自己就增加重入数量即可。

   非公平锁实现
   ![image-20190717163910632](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190717163910632.png)


   ​	这里发现非公平锁是指对锁的状态判断了一下，就开始抢占了，并没有判断有没有前置节点，这就是非公平锁和公平锁的区别所在。

9. addWaiter 的实现

   ![image-20190717164602607](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190717164602607.png)

   新建节点并插入队尾，通过cas的方式。
   ![image-20190717164758969](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190717164758969.png)

   空队列初始化，然后cas失败会重新尝试

10. acquireQueued 获取锁队列，大多数情况是在这里的

  ![image-20190718154132264](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190718154132264.png)

11. 是否应该中断并阻塞当前线程

![image-20190718154154367](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190718154154367.png)

12. 解锁流程

    ​	![image-20190718154309445](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190718154309445.png)

    释放锁的过程就是释放锁，然后唤醒下一个节点
    ![image-20190718154420185](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190718154420185.png)
    释放锁的逻辑就是把当前重入次数-1 ，然后如果当前线程不是独占线程就抛出异常，释放锁应该是当前获得锁的线程才对。接下来判断如果c==0说明已经解锁成功，释放当前独占线程然后返回。如果返回true说明不需要再占有锁了。就唤醒下一个节点。

13. unparkSccessor

    ![image-20190718154857876](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190718154857876.png)
    这里就是唤醒下一个节点的逻辑了。唤醒下一个节点的主要逻辑就是清空头结点，然后删除后续链表中的无效节点，然后唤醒下一个有效节点即可。

14. 最后waitStatus说明![image-20190718160314487](/Users/yunzhifei/Library/Application Support/typora-user-images/image-20190718160314487.png)