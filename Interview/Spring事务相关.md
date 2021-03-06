### 事务的特性（ACID）
1. **原子性**：事务是最小的执行单位，不允许分割。事务的原子性确保动作要么全部完成，要么全部不起效果。
2. **一致性**：执行事务前后，数据保持一致。
3. **隔离性**：并发访问数据库时，一个用户的事务不被其他事务所干扰，各并发事务之间数据库是独立的。
4. **持久性**：一个事务被提交之后。它对数据库中数据的改变是持久的，即使数据库发生故障也不应该对其有任何影响。


### 事务的7种传播行为


| **事务传播行为类型** | **说明** |
| --- | --- |
| PROPAGATION_REQUIRED | 如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务种。这时常见的选择。 |
| PROPAGATION_SUPPORTS | 支持当前事务，如果当前没有事务，就以非事务的方式执行。 |
| PROPAGATION_MANDATORY | 使用当前的事务，如果没有事务，就抛出异常。 |
| PROPAGATION_REQUIRES_NEW | 新建事务，如果当前存在事务，就把当前的事务挂起。 |
| PROPAGATION_NOT_SUPPORTED | 以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。 |
| PROPAGATION_NEVER | 以非事务的方式执行，如果存在事务，就抛出异常。 |
| PROPAGATION_NESTED | 如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与PROPAGATION_REQUIRED类似的操作。 |



**a方法有事务注解，b方法没有事务注解，在b方法中调用a方法，也不会开启事务。
在a方法中调用b方法会开启事务。**