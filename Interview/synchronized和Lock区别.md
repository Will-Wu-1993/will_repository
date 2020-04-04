### synchronized和Lock的区别：
1. synchronize是JVM层面的，lock是API层面的；
2. Lock是公平锁和非公平锁，而synchronize只有非公平锁；
3. synchronize不需要手动释放锁，而Lock需要手动释放锁；
4. 是否可以中断：synchronize不可中断，Lock是可以中断的；
5. synchronize不能根据条件唤醒，不能精确唤醒，Lock可以（Condition）


- synchronize是关键字，Lock是接口
- synchronize通过JVM实现锁机制，Lock是通过JDK实现锁机制
- synchronize自动上锁，自动解锁，Lock手动上锁，手动解锁
- synchronize无法判断是否可以获取锁，Lock可以判断是否拿到了锁
- synchronize拿不到锁会一直等待，Lock不一定会一直等待
- synchronize是非公平锁，Lock可以设置是否为公平锁
公平锁：排队，当锁没有被占用时，当前线程需要判断队列中是否有其他等待线程。
非公平锁：插队，当锁没有被占用时，当前线程可以直接占用，不需要判断队列中是否有其他等待线程。