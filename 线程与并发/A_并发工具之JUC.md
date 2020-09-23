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

###Semaphore

###CountdownLatch

###CyclicBarrier

###ConcurrentHashMap

###ConcurrentLinkedQueue

###BlockingQueue