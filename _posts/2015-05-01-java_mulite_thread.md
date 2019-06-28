---
layout: post
title: JAVA多线程知识点总结
date: 2015-05-01
tags: [JAVA, 多线程, 阻塞/非阻塞, 同步/异步]
---

## java 多线程基础

### java 阻塞式多线程同步详解
java final引用的对象其属性可以改变，但是final型的引用是不能被修改的


### 名词解释：同步、异步、阻塞、非阻塞
**同步与异步**：同步和异步是客户端与服务端通信的一种机制。  
**同步通信**：就是客户端发送请求后，必须要等着接收到服务端的响应才能进行下一个请求。  
**异步通信**：就是客户端发送请求后，不必一直等待服务端的响应，它仍然可以进行下一个请求，最终结果可能会由服务端通过回调函数返还给客户端。  
**阻塞和非阻塞**：阻塞和非阻塞是客户端线程在等待服务端响应过程中的一个线程状态。如果在等待的过程中线程可以继续运行则叫做非阻塞，但是如果等待的过程中线程挂起或者休眠了就叫做阻塞。

>    场景：小明用下载器下载一部电影，分析这个过程。  
>     1、同步阻塞
>     场景：小明（客户端），下载器（服务端），下载电影（客户端请求），下载完成（服务端响应），下载器有无完成时提醒（服务端回调函数）
>     小明使用下载器下载电影，一直等在电脑前面，什么事情没有做，等待电影下载完成。
>     同步：小明一直等待电影下载完成=客户端一直在等待服务端响应。
>     阻塞：等待的过程，小明啥事也没做，空等。  
>     2、同步非阻塞
>     场景：小明（客户端），下载器（服务端），下载电影（客户端请求），下载完成（服务端响应），下载器有无完成时提醒（服务端回调函数）
>     小明使用下载器下载电影，跑去看电视，时不时看下下载进度，电影下载完成。
>     同步：就下载电影这件事，小明一直等待电影下载完成=客户端一直在等待服务端响应。
>     非阻塞：小明在下载电影的过程中，还去看电视了。  
>     3、异步阻塞
>     场景：小明（客户端），下载器（服务端），下载电影（客户端请求），下载完成（服务端响应），下载器有无完成时提醒（服务端回调函数）
>     小明使用下载器下载电影，一直等待在电脑前面，啥事也不做，下载完成后下载器有提醒。
>     异步：小明下载电影1后其实可以立马下载其他电影，不用等电影1下载完。但是因为阻塞了，所以没有得到体现。
>     阻塞：小明还是一直等待在电脑前面。空等。  
>     4、异步非阻塞
>     场景：小明（客户端），下载器（服务端），下载电影（客户端请求），下载完成（服务端响应），下载器有无完成时提醒（服务端回调函数）
>    小明使用下载器下载电影1，过了一分钟（电影1没下载完），小明又开始下载电影2，然后再去看电视，下载器下载完成提醒小明。
>    异步：小明用下载器下载电影1，不用等电影1完成，又下载电影2。客户端请求服务端，不用等它响应，又请求客户端。
>    非阻塞：小明在下载电影的过程中，还去看电视了。

参考文档：[同步与异步、阻塞与非阻塞理解](https://www.cnblogs.com/chinaifae/p/10271005.html){:target="_blank"}


**阻塞式线程同步，做了什么？**   
在程序中，对共享变量的使用一般遵循一定的模式，即读取、修改和写入三步组成。之前碰到的问题是，这三步执行中可能线程执行切换，造成非原子操作。锁机制是把这三步变成一个原子操作。目前CPU本身实现 将这三步 合起来 形成一个原子操作，无需线程锁机制干预，就是下面所讲的非阻塞式线程同步。


### 非阻塞式线程同步方式

    线程之间同步机制的核心是监视对象上的锁，竞争锁来获得执行代码的机会。当一个对象获取对象的锁，然后其他尝试获取锁的对象会处于等待状态，这种锁机制的实现方式很大程度限制了多线程程序的吞吐量和性能（线程阻塞），且会带来死锁（线程A有a对象锁，等着获取b对象锁，线程B有b对象锁，等待获取a对象锁）和优先级倒置（优先级低的线程获得锁，优先级高的只能等待对方释放锁）等问题。

    如果能不阻塞线程，又能保证多线程程序的正确性，就能有更好的性能。

    在程序中，对共享变量的使用一般遵循一定的模式，即读取、修改和写入三步组成。之前碰到的问题是，这三步执行中可能线程执行切换，造成非原子操作。锁机制是把这三步变成一个原子操作。

    目前CPU本身实现 将这三步 合起来 形成一个原子操作，无需线程锁机制干预，常见的指令是“比较和替换”（compare and swap,CAS），这个指令会先比较某个内存地址的当前值是不是指定的旧指，如果是，就用新值替换，否则什么也不做，指令返回的结果是内存地址的当前值。通过CAS指令可以实现不依赖锁机制的非阻塞算法。一般做法是把CAS指令的调用放在一个无限循环中，不断尝试，直到CAS指令成功完成修改。

java.util.concurrent.atomic包中提供了CAS指令。（不是所有CPU都支持CAS，在某些平台，java.util.concurrent.atomic的实现仍然是锁机制）
注意：java.util.concurrent.atomic包中的Java类属于比较底层的实现，一般作为java.util.concurrent包中很多非阻塞的数据结构的实现基础。

java.util.concurrent.atomic包提供的java类可以分成三类：
1. 支持以原子操作来进行更新的数据类型的Java类（AtomicBoolean、AtomicInteger、AtomicReference），在内存模型相关的语义上，这四个类的对象类似于volatile变量。写：立即刷新主存，读：以主存为标准；
    填充 JDK 中的api。
    在实现线程安全的计数器时，AtomicInteger和AtomicLong类时最佳的选择。

2. 提供对数组类型的变量进行处理的Java类，AtomicIntegerArray、AtomicLongArray和AtomicReferenceArray类。（同上，只是放在类数组里，调用时也只是多了一个操作元素索引的参数


## java线程同步高级抽象(上层封装API接口)
java.util.concurrent包为多线程提供了高层的API，满足日常开发中的常见需求。

### 常用接口

1、Lock接口，表示一个锁方法：

    a、lock()，获取所，如果无法获取所锁，会处于等待状态
    
    b、unlock()，释放锁。（一般放在finally代码块中）
    
    c、lockInterruptibly()，与lock()类似，但允许当前线程在等待获取锁的过程中被中断。（所以要处理InterruptedException）
    
    d、tryLock()，以非阻塞方式获取锁，如果无法获取锁，则返回false。（tryLock()的另一个重载可以指定超时，如果指定超时，当无法获取锁，会等待而阻塞，同时线程可以被中断）

2、ReadWriteLock接口，表示两个锁，读取的共享锁和写入的排他锁。（适合常见的读者--写者场景）
    
    ReadWriteLock接口的readLock和writeLock方法来获取对应的锁的Lock接口的实现。
    在多数线程读取，少数线程写入的情况下，可以提高多线程的性能，提高使用该数据结构的吞吐量。
    如果是相反的情况，较多的线程写入，则接口会降低性能。

3、ReentrantLock类和ReentrantReadWriteLock，分别为上面两个接口的实现类。

    他们具有重入性：即允许一个线程多次获取同一个锁（他们会记住上次获取锁并且未释放的线程对象，和加锁的次数，getHoldCount()）
    同一个线程每次获取锁，加锁数+1，每次释放锁，加锁数-1，到0，则该锁被释放，可以被其他线程获取。
    
    public class LockIdGenrator{
    //new ReentrantLock(true)是重载，使用更加公平的加锁机制，在锁被释放后，会优先给等待时间最长的线程，避免一些线程长期无法获得锁
       private int ReentrantLock lock = ReentrantLock();
       privafte int value = 0;
       public int getNext(){
          lock.lock();      //进来就加锁，没有锁会等待
          try{
             return value++;//实际操作
          }finally{
             lock.unlock();//释放锁
          }
       }
    }
    注：重入性减少了锁在各个线程之间的等待，例如便利一个HashMap，每次next()之前加锁，之后释放，可以保证一个线程一口气完成便利，而不会每次next()之后释放锁，然后和其他线程竞争，降低了加锁的代价， 提供了程序整体的吞吐量。（即，让一个线程一口气完成任务，再把锁传递给其他线程）。

4、Condition接口，Lock接口代替了synchronized，Condition接口替代了object的wait、nofity。

    a、await()，使当前线程进入等待状态，知道被唤醒或中断。重载形式可以指定超时时间。
    b、awaitNanos()，以纳秒为单位等待。
    c、awaitUntil()，指定超时发生的时间点，而不是经过的时间，参数为java.util.Date。
    d、awaitUninterruptibly()，前面几种会响应其他线程发出的中断请求，他会无视，直到被唤醒。
    注：与Object类的wait()相同，await()会释放其所持有的锁。0
    e、signal()和signalAll， 相当于 notify和notifyAll

### concurrent包下常用多线程数据结构

阻塞队列

    BlockingQueue.class，阻塞队列接口
    DelayQueue.class，阻塞队列，无界队列，并且元素是Delay的子类，保证元素在达到一定时间后才可以取得到
    
    BlockingDeque.class，双端阻塞队列接口
    ArrayBlockingQueue.class，阻塞队列，数组实现。有界阻塞队列
    
    LinkedBlockingDeque.class，阻塞双端队列，链表实现
    LinkedBlockingQueue.class，阻塞队列，链表实现，无界，以下是该类对与生产者消费者模型的简单实现：
    
    SynchronousQueue.class，同步队列，但是队列长度为0，生产者放入队列的操作会被阻塞，直到消费者过来取，所以这个队列根本不需要空间存放元素；有点像一个独木桥，一次只能一人通过，还不能在桥上停留

非阻塞队列
   
   常见非阻塞队列：
　　ConcurrentLinkedDeque.class，非阻塞双端队列，链表实现，无界队列
　　ConcurrentLinkedQueue.class，非阻塞队列，链表实现

转移队列：

　　TransferQueue.class，转移队列接口，生产者要等消费者消费的队列，生产者尝试把元素直接转移给消费者
　　LinkedTransferQueue.class，转移队列的链表实现，它比SynchronousQueue更快，transfer 方法有客户端准备消费，直接把消息直接传递给消费者，不放到队列里，没有消费者线程的话该线程会阻塞。但是可以调用 add put 王队列里丢，队列还是有容量的。

常见集合容器

    ConcurrentMap.class，并发Map的接口，定义了putIfAbsent(k,v)、remove(k,v)、replace(k,oldV,newV)、replace(k,v)这四个并发场景下特定的方法
    ConcurrentHashMap.class，并发HashMap，ConcurrentHashMap在并发中效率比 HashTable高，因为 HashTable 在往里添加东西的时候药锁定整个对象，而 ConcurrentHashMap 分成了16段，插入的时候只锁定了其中的一段，其实就是把锁细粒度化了，因此在多线程情况下回比 hashTable高 ，同样也比 Collections.synchronizedMap(map1) 高。
    ConcurrentSkipListMap.class，跳表数据结构，它也是NavigableMap的实现类（要求元素之间可以比较），只有你确实需要快速的遍历操作，并且可以承受额外的插入开销的时候，在高并发中要求排序才去使用它。
    ConcurrentSkipListSet.class，和上面类似，只不过map变成了set
    CopyOnWriteArrayList.class，copy-on-write（写时复制）模式的array list，每当需要插入元素，不在原list上操作，而是会新建立一个list，然后将原先的引用指向副本。适合读远远大于写的场景
    CopyOnWriteArraySet.class，和上面类似，list变成set而已　

## 多线程任务的执行与管理

1、基本接口（描述任务）

a、Callable接口：

    Runnable接口受限于run方法的类型签名，而Callable只有一个方法call()，可以有返回值，可以抛出受检异常。
b、Future接口：

    过去，需要异步线程的任务执行结果，要求主线程和任务执行线程之间进行同步和数据传递。
    Future简化了任务的异步执行，作为异步操作的一个抽象。调用get()方法可以获取异步的执行结果，如果任务没有执行完，会等待，直到任务完成或被取消，cancel()可以取消。
c、Delayed接口：

    延迟执行任务，getDelay()返回当前剩余的延迟时间，如果不大于0，说明延迟时间已经过去，应该调度并执行该任务。

2、组合接口（描述任务）

a、RunnableFuture接口：继承自Runnable接口和Future接口。

    当来自Runnalbe接口中的run方法成功执行之后，相当于Future接口表示的异步任务已经完成，可以通过get()获取运行结果。
    
b、ScheduledFuture接口：继承Future接口和Delayed接口，表示一个可以调用的异步操作。

c、RunnableScheduledFuture接口：继承自Runnable、Delayed和Future，接口中包含isPeriodic，表明该异步操作是否可以被重复执行。

3. ThreadPoolExecutor Executor  ExecutorServer  ScheduleExecutorService  CompletionService接口

[参考实例](https://blog.csdn.net/escaflone/article/details/10418651){:target="_blank"}

4. ThreadLocal类
