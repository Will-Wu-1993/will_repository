**Java并发编程的目的就是充分利用计算机的资源，把计算机的性能发挥到最大**

### 1、什么是高并发
并发concurrent和并行parallelism的区别。
> 并发是着多个线程操作同一个资源，不是同时操作，而是交替操作。

> 并行才是正真的同时执行，多核CPU，每个线程使用一个独立的CPU的资源来运行。

并发编程是指使系统允许多个任务在重叠的时间段内执行的设计结构。

高并发：设计的程序可以支持海量任务的同时执行（任务的执行在时间段上有重叠的情况）
- QPS：每秒响应的请求说，QPS并不是并发数。
- 吞吐量：单位时间内处理的请求数，QPS和并发数决定。
- 并发用户数：系统可以承载的最大用户数量。
互联网项目架构中，如何提高系统的并发能力？
- 垂直扩展
- 水平扩展
### 垂直扩展
提升单机的处理能力
1. 增强单机的硬件性能：增强CPU的核数，硬盘扩容，升级内存。
2. 提升系统的架构性能：使用Cache来提高效率，异步请求增加单个服务的吞吐量，使用NoSQL来提升数据的访问性能。

### 水平扩展
集群、分布式都是水平扩展的方案
> 集群：多个人做同一件事

> 分布式：把一件复杂的事情，拆分成几个简单的步骤，分别找不同的人去完成。

1. 站点层扩展：Nginx反向代理，高并发系统，一个Tomcat带不起来，就找十个Tomcat去带。
2. 服务层扩展：通过RPC框架实现远程调用、Dubbo、Spring Cloud，将业务逻辑拆分到不同的RPC Client，各自完成不同的业务，如果某些业务的并发量恒大，就增加新的RPC Client，理论上实现无限高并发。
3. 数据层扩展：一台数据库拆成多台，主从复制、读写分离、分表分库。


### 2、线程和进程
进程就是计算机正在运行的一个独立的应用程序，进程是一个动态的概念，必须事运行的状态，如果一个应用程序没有启动，那就不是进程。

线程就是组成进程的基本单位，可以完成特定的功能，一个进程是由一个或多个线程组成。

进程和线程的区别在于运行时是否拥有独立的内存空间，每个进程所拥有的空间事独立的，互不相干扰，多个线程是共享内存空间的，但每个线程的执行是相互独立的。

线程必须依赖于进程才能执行，单独线程是无法执行的，由进程来控制多个线程的执行。

**Java默认有几个线程？**
Java默认有两个线程：Main 和 GC


**Java中实现多线程有几种方式？**
继承Thread
实现Runnable
实现Callable
Thread是线程对象，Runnable是任务
```java
package com.southwind.demo;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

public class Test {
    public static void main(String[] args) {
        MyCallable myCallable = new MyCallable();
        FutureTask futureTask = new FutureTask(myCallable);
        Thread thread = new Thread(futureTask);
        thread.start();
        //获取Callable的返回值
        try {
            System.out.println(futureTask.get());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}

class MyCallable implements Callable<String>{

    @Override
    public String call() throws Exception {
        System.out.println("callable");
        return "hello";
    }
}
```

**Runnable与Callable的区别：**
* Callable的call方法有返回值，Runnable的run方法没有返回值。
* Callable的call方法可以抛出异常，Runnable的run方法不能抛出异常。
* 在外部通过FutureTask的get方法异步获取执行结果，FutureTask是一个可以控制的异步任务，是对Runnable实现的一种继承和扩展。
get方法可能会产生阻塞，一般放在代码的最后。
Callable有缓存。

#### **JUC**
java.util.concrrent，JDK提供的并发编程工具包，是由一组类和接口组成，可以更好的支持高并发任务。


是开发中不会直接使用sleep休眠，而是使用juc的方式。
TimeUnit.SECONDS.sleep(1);
底层调用的还是Thread.sleep()方法。
让休眠更加准确，毫秒+纳秒。


### 3、sleep和wait的区别
1. 来自不用的类，sleep是Thread的方法，wait是Object类的方法。
2. sleep是让当前线程对象暂停执行任务，操作的线程对象。wait是让正在访问当前对象的线程休眠，不是针对线程对象，而是针对线程对象要访问的资源。但是wait有一个前提，当前线程对象必须拥有该资源，所以wait方法只能在同步方法或同步代码块中，否则会抛出异常。
3. wait释放锁，sleep不释放锁。
wait的使用
```java
package com.southwind.demo3;

import java.util.concurrent.TimeUnit;

public class Test {
    public static void main(String[] args) {
        A a = new A();
        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                a.test(i);
            }
        }).start();
    }
}

/**
 * 资源
 */
class A{
    public synchronized void test(int i){
        if(i == 5){
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(i+"------------------");
    }
}
```

**如何解除阻塞？**
1. 指定wait时间
```java
this.wait(2000);
```
2. 通过调用notify方法唤醒线程
```java
package com.southwind.demo3;

import java.util.concurrent.TimeUnit;

public class Test {
    public static void main(String[] args) {
        A a = new A();
        //任务线程
        new Thread(()->{
            for (int i = 0; i < 10; i++) {
                a.test(i);
            }
        }).start();

        //唤醒线程
        new Thread(()->{
            try {
                TimeUnit.SECONDS.sleep(7);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            a.test2();
        }).start();
    }
}

/**
 * 资源
 */
class A{
    public synchronized void test(int i){
        if(i == 5){
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(i+"------------------");
    }

    public synchronized void test2(){
        this.notify();
    }
}
```
notify随机唤醒一个wait线程、
notifyAll唤醒所有wait线程。


### 4、synchronize锁定的是谁？

> 如果synchronize修饰非静态方法（实例方法），则锁定的是方法的调用者，只需要判断有几个对象，如果是一个对象，那么一定需要排队（线程同步），如果是多个对象，不需要排队。
> 如果synchronize修饰的静态方法，则锁定的是类，无论多少个对象调用，都会同步。
> 如果synchronize静态方法和实例方法同时存在，静态方法锁类，实例方法锁对象。
> 如果synchronize修饰的是代码块，则锁定的传入的对象，能否实现线程同步，就看锁的对象是否为同一个对象。

### 5、Lock锁
Lock是对synchronize的升级，是一个JUC接口，常用的实现类是ReentrantLock（重入锁）。
synchronize通过JVM实现锁机制，ReentrantLock通过JDK实现锁机制。
重入锁：可以给同一个资源添加多把锁。