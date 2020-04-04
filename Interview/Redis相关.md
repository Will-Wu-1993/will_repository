### Redis数据类型：
    String：简单的k-v类型
    Hash：特别适合储存对象，比如储存用户信息
    Set：不会重复的列表
    sorded set：有序的列表
    List：列表
### Redis的好处：  
1. 速度块：因为数据储存在内存中。
2. 支持丰富的数据类型：支持String、Hash、Set、sorded set和List。
3. 支持舒服
4. 丰富的特性：可用于缓存，消息队列。
5. set类型的数据是可以排重的。
### 6种淘汰机制：
Redis内存达到一定大小时，就会实行数据淘汰策略（回收策略）
1. volatile-lru：从已经设置了过期时间的数据中，挑选使用最少的数据
2. volatile-ttl：从已经设置了过期时间的数据中，淘汰即将过期的数据。
3. volatile-random：从已经设置了过期时间的数据中，随机淘汰数据。
4. allkeys-lru：从数据集中，挑选使用最少的数据淘汰。
5. allkeys-random：从数据集中随机淘汰数据。
6. no-envivtion（驱逐）：禁止驱逐数据。


#### Redis是单线程的，采用队列模式将并非访问变成串行访问。


### Redis分布式锁：
1. 使用Redis  setnx获取锁；
2. 发现Redis中有数据，说明已经加锁了；
3. 发现Redis中没有数据，说明可以获取锁
4. 这些操作都是显性的操作，都需要在Java代码中编写：
    1. Java调用setnx获取锁
    2. Java调用Redis del删除
5. 设置锁超时时间 expire
    1. Java掉用Redis  expire

可以使用Redisson框架实现分布式锁
但是还会有一个问题，就是Redis主从模式，主Redis获取了锁，但是还没有来得及同步到从Redis，主Redis挂了，此时的从节点变为了主节点，当下一个请求过来时，没有发现锁，就是正常执行下去，但是同时之前的请求还在执行，这样就会出现问题。



### Redis异步队列：
&emsp;&emsp;一般使用List结构作为队列，rpush生产消息，lpop消费消息。当lpop没有消息消费的时候，要适当的sleep一会再重试。
&emsp;&emsp;如果不使用sleep呢？List还有一个指令blpop，在没有消息的时候，他会阻塞，等到有新的消息到来。

### Redis的持久化：
&emsp;&emsp;RDB持久化：能够在设定的时间间隔对数据进行快照储存。
&emsp;&emsp;AOF持久化：记录每次对服务器的操作。
&emsp;&emsp;Redis重启的时候，会优先载入AOF文件来恢复数据，因为在通常情况下，AOF文件保存的数据要比RDB文件保存的数据完整。