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


###CountdownLatch

###CyclicBarrier

###ConcurrentHashMap

###ConcurrentLinkedQueue

###BlockingQueue