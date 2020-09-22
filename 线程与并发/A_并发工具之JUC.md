#JUC

###AQS
AbstractQueuedSynchronizer，是阻塞式锁和相关同步器工具的框架
使用state属性表示资源状态：独占模式/共享模式，子类来定义如何维护该状态，以实现获取锁和释放锁
    独占模式是只有一个线程能访问资源，共享模式是允许多个线程访问资源【可设置访问上限】
    提供基于FIFO的等待队列
    提供条件变量实现等待唤醒机制，支持多个条件变量
使用方式：子类继承并覆盖五大方法【tryAcquire获取锁，tryRelease释放锁，tryAcquireShared获取锁，tryReleaseShared释放锁，isHeldExclusively】

###ReentrantLock可重入锁
参考【5_死锁活锁饥饿&可重入锁】

###读写锁

###Semaphore

###CountdownLatch

###CyclicBarrier

###ConcurrentHashMap

###ConcurrentLinkedQueue

###BlockingQueue