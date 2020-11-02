#park&unpark

1. park和unpark方法
    LockSupport类中的静态方法，LockSupport.park()暂停当前线程，LockSupport.unpark(暂停的线程对象)。
    park后线程的状态时wait状态。
    若先执行了unpark，再执行park，将不会暂停。
    原理：
        park是先判断state是0还是1，若是1，则变为0；若是0，则当前线程进入阻塞；
        unpark是先给state置为1，然后判断线程是否在阻塞，阻塞则唤醒且state变为0，不阻塞则无行动。