#线程与线程池

1. 创建线程
    * 继承线程类，实现run方法，子类对象调用start方法创建线程并执行run方法
    * 实现Runnable接口，实现run方法，创建线程对象传入Runnable接口实现类对象，
        调用线程对象的start方法创建线程并执行传入的Runnable接口实现类的run方法
    * 实现Runnable接口或Callable接口，将对象作为参数构造FutureTask对象，
        FutureTask对象作为参数传给Thread对象，Thread对象调用start方法创建线程并执行。
        【最后可以使用FutureTask对象的get方法获取返回值】【参考FutureTaskDemo】
        
2. JVM栈和栈帧
3. 线程上下文切换
    * 线程的CPU时间片用完
    * 垃圾回收
    * 有更高优先级的线程出现
    * sleep、join、yield、wait、park、synchronized、lock等方法调用
    * 线程私有的程序计数器、栈信息等状态信息先保存下来，然后恢复要执行线程的状态信息。频繁切换上下文影响性能
    
4. 线程中常见方法
    * Thread
        * start Thread类的方法，作用是启动一个新线程并使其进入就绪状态，仅能调用一次。
        * run   新线程启动后调用的方法，默认是调用构造方法传入的Runnable接口的run方法。
        * join  实际上调用了Object类的wait方法
            【在A线程中调用了B线程的join()方法时，表示只有当B线程执行完毕时，A线程才能继续执行。】
            【join传入毫秒数，则是最多等待传入毫秒数，之后A线程就可以执行了】
            【当前线程调用t.join方法，则当前线程会在t线程对象的monitor上等待】
        * interrupt     打断线程，若要打断的线程正处于阻塞sleep、wait、join，会抛异常且打断标记为false，否则，设置打断标记为true
        * isInterrupted 判断线程是否被打断，不清除打断标记
        * interrupted   静态方法，判断当前线程是否被打断，并清除打断标记【置为FALSE】
        * sleep 静态方法，让当前线程休眠一定时间，休眠后进入RUNNABLE状态，开始抢CPU执行权
        * yield 静态方法，让当前线程让出CPU使用权，开始抢CPU执行权
        * setDaemon 将调用者置为当前线程的守护线程
    * Object
        * wait 当前线程【不是调用该方法的线程对象，而是语句所在的线程】等待，同时释放锁，详见【3_wait&notify】
        * notify 详见【3_wait&notify】
        * notifyAll 详见【3_wait&notify】
    * LockSupport
        * park  暂停当前线程，可被打断，当打断标记置为true时，此方法无效

5. 线程状态
    * NEW           新建的线程，尚未调用start方法
    * RUNNABLE      调用start方法后
    * BLOCKED       阻塞  未拿到锁的阻塞
    * WAITING       等待  调用join或wait方法
    * TIMED_WAITING 调用sleep方法后
    * TERMINATED    终止