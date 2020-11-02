#JUC

###AQS
AbstractQueuedSynchronizer，是阻塞式锁和相关同步器工具的框架
使用state属性表示资源状态：独占模式/共享模式，子类来定义如何维护该状态，以实现获取锁和释放锁
    对state的操作有get、set和CAS
    独占模式是只有一个线程能访问资源，共享模式是允许多个线程访问资源【可设置访问上限】
    提供基于FIFO的等待队列
    提供条件变量实现等待唤醒机制，支持多个条件变量
使用方式：子类继承并覆盖抽象父类的五大方法【tryAcquire获取锁，tryRelease释放锁，tryAcquireShared获取锁，tryReleaseShared释放锁，isHeldExclusively是否持有独占锁】
使用场景：自定义一个锁类，实现Lock接口；
    在自定义的锁类中自定义一个内部类继承AbstractQueuedSynchronizer抽象类，并生成该自定义内部类的对象作为同步器工具对象。
    借助同步器工具对象
    示例：top.trial.concurrent.juc.MyLockWithAQS
###ReentrantLock可重入锁
参考【5_死锁活锁饥饿&可重入锁】

###读写锁
读写锁分两种，分别是ReentrantReadWriteLock和StampedLock
当读操作远远高于写操作时，使用读写锁让读操作并发，出现写操作与读操作并行时再加锁使之互斥【即读读可并发，读写和写写互斥】
使用读锁保护读取操作，使用写锁保护修改操作
#####ReentrantReadWriteLock
使用方式：new该类对象，同时使用方法生成读锁和写锁，用读锁保护读操作，用写锁保护写操作【操作前加锁，try包围操作部分，finally中解锁】
注意事项：
    读锁不支持条件变量，
    不支持重入升级【即获取到读锁的情况下再获取写锁，会导致获取写锁的操作永久等待】
    支持重入降级【即获取到写锁的情况下再获取读锁，没有问题】
原理：
    读写锁维护一个AQS
    state低16位记录写锁状态，高16位记录读锁状态
    ***读取源码分析读锁和写锁的加锁解锁机制
#####StampedLock
带戳的读写锁，在1.8后新增，进一步提高读性能【减少读读并发时的各种验证操作】
在加读写锁时返回一个long值的戳，解锁时传入戳值即可
注意事项：
    不支持条件变量
    不支持锁重入
```
加锁解锁
long stamp = lock.readLock
lock.unlockRead(stamp)
long stamp = lock.writeLock
lock.unlockWrite(stamp)

//读取操作
long stamp = lock.tryOptimisticRead();
//此处进行读取
if(!lock.validate(stamp)){
    验戳失败，说明中间进行了写操作，此处应锁升级至读锁
}
```


###Semaphore信号量
用来限制能同时访问共享资源的线程上限，可以用于单机版的高峰期限流
创建信号量时构造方法传入能同时访问共享资源的线程上限
acquire方法获得信号量，当获取信号量的线程已经达到上限，则会阻塞在此处，等待其它线程释放信号量
release方法释放信号量


###CountdownLatch倒计时锁
用于线程间同步协作。
用法：构造器传入计数值，调用await方法，当计数值大于0时等待，其它线程调用countDown方法将计数值减1，当计数值减到0，继续执行await后方法
该类对象属于消耗品，无法重置计数值，若想有重用，请使用下面的CyclicBarrier

###CyclicBarrier
用于线程间同步协作
用法，构造器传入计数值，调用await方法，当计数值大于0时等待。
与CountdownLatch用法上的区别：
    CountdownLatch是调用countDown方法将计数减1，然后继续运行，主线程使用await方法等待；
    而CyclicBarrier是在任务线程中使用await方法等待，await方法是将计数值减1并等待，直到计数值变为0，然后唤醒所有等待着的任务线程。
    另外，CyclicBarrier构造方法还可以传入一个Runnable接口，该接口的方法在计数值为0时被调用。
    CyclicBarrier在计数值归0后再次调用await方法，计数值又从原始值开始进行减一，因此可以循环调用，对象创建一次即可。
    循环使用时，线程池线程数建议与计数值一致

###线程安全集合类概述
old线程安全集合类：HashTable【线程安全的Map，方法由Synchronized修饰】、Vector
修饰的线程安全集合类：SynchronizedMap、SynchronizedList【对原集合包装，方法增加Synchronized包围原方法】
JUC中的线程安全集合类：Blocking*、CopyOnWrite*、Concurrent*
    Blocking大部分的实现基于锁，提供阻塞方法，不满足条件时等待
    CopyOnWrite采用修改时拷贝的方式避免多线程读写时并发安全问题，适用于读多写少的场景，写时开销大效率低
    Concurrent多数采用CAS进行优化提高吞吐量，也有采用多把锁以提高并发效率
        Concurrent存在的问题：弱一致性
            遍历时：利用迭代器遍历，若遍历时发生修改，遍历仍可继续，但是内容是修改前的旧内容；
            多线程并发场景，调用size方法时，数据不一定准确；
            读取值时，也可能出现弱一致性。

###ConcurrentHashMap
常用的线程安全的方法：
    构造方法：构造方法仅计算初始数组大小，懒惰初始化，仅在存入第一个数据时初始化
    get流程：无锁实现，拿取hash码，若map存在，根据hash码找到对应桶，
        头元素就是，则直接返回，
        头元素hash值是负数，指正在扩容或已成红黑树，调用find方法去扩容后的数组或红黑树里找
        若头元素不是，而且头元素hash值是正数，说明是个不在扩容中的链表，while循环比较取出hash码相等且equals的那个元素即可
    put流程：调用putVal方法，最后一个参数false指同一个key新值会覆盖旧值
        先检查key和value是否为空，两者都不允许为null
        判断表是否存在，不存在就初始化表
        判断链表头是否存在，不存在就添加链表头
        判断链表头的hash值是否是-1【正在扩容】，在扩容的就去帮忙扩容
        其余情况，说明可以去正式添加值了，此时加锁【锁是链表头】，先判断是链表还是红黑树，
            链表的话，遍历，找到相同的，替换或不操作，到最后都找不到，追加到链表尾
            红黑树的话，调用红黑树的添加方法
        此时添加完成，判断是否需要扩容或链表转红黑树
        增加count计数，且也有扩容逻辑
    size流程：
    remove流程：
    computeIfAbsent：一句顶三句，传入key和function，检查key存不存在，不存在时，使用function计算一个值并存入map。方法返回value
注意：
    扩容扩到64后，链表转换成红黑树，删除结点到一定阈值，红黑树又会还原成链表

###ConcurrentLinkedQueue
设计思想与LinkedBlockingQueue一致，使用了两把“锁“和Dummy节点，允许同一时刻两个线程一个做入队一个做出队。
不同的是，ConcurrentLinkedQueue的”锁“是使用CAS实现，而不是LinkedBlockingQueue那种使用ReentrantLock实现

###BlockingQueue
常用实现有LinkedBlockingQueue
    内部维护节点类
    初始化时：head和last指针指向一个Dummy占位节点，其值item和next指针都是null
    节点入队：last指向新节点node，且原last指向的节点的next指向新节点node，即last = last.next = node;
    节点出队：head指向Dummy节点的next，并返回该节点的item值，同时将item置空使其成为新的Dummy
    使用了两把锁和Dummy节点，允许同一时刻两个线程一个做入队一个做出队。
还有ArrayBlockingQueue实现，与LinkedBlockingQueue相比：
    Linked支持有界和无界，Array必须有界
    Linked是懒惰初始化，Node在入队时创建，Array需要提前初始化Node数组，且Node提前创建好，增加重用减少了垃圾回收
    Linked时两把锁，而Array只有一把锁
    
###CopyOnWriteArrayList
底层实现采用”写入时拷贝“的思想，增删改操作都将底层数据拷贝一份，在新数据上进行修改，不影响在旧数据上的读操作。
读读和读写并发，写写会触发锁导致等待
同样存在弱一致性问题