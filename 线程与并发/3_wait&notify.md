#wait&notify

1. wait方法让当前线程等待，等待于锁对象的Monitor的WaitSet中，同时清除Monitor的Owner允许其它线程抢占。【即释放锁】
2. notify方法唤醒锁对象的Monitor的WaitSet中的随机一个线程，将之放到EntryList中参与线程抢占。
3. notifyAll方法唤醒锁对象的Monitor的WaitSet中的全部线程，将之放到EntryList中参与线程抢占。
4. wait、notify、notifyAll都是Object中的方法，当前线程必须获得此对象的锁，才能调用这几个方法，即Owner的线程才能使用锁对象调用这几个方法
    即：三个方法需要和synchronized一起用。  
5. 同步模式--保护性暂停：用在一个线程等待另一个线程结果时
    * 有一个结果需要从一个线程传递到另一个线程，如果不断有结果需要传递，则需要消息队列解决
    * join和Future是此模式的具体实现示例
    * 示例代码：
        GuardedObjectTest
        GuardedObjectPlusTest
6. 异步模式--生产者消费者模式：