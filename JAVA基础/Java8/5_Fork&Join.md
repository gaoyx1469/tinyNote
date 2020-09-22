#Fork/Join

JDK的JUC包提供了RecursiveTask和RecursiveAction两个类，分别执行有返回值的任务和无返回值的任务
代码示例
    top.trial.concurrent.forkjoin.ForkJoinTest
    top.trial.concurrent.forkjoin.MyTask
    top.trial.concurrent.forkjoin.MyTaskPlus
    
new ForkJoinPool来创建线程池
线程池调用invoke方法处理task【继承RecursiveTask】
task中compute方法执行计算，
    对于拆分后足够小的情况，直接计算返回结果，
    对于还要继续拆分的情况，新建task对象调用fork方法执行拆分的任务，调用join方法获取拆分后任务的执行结果。
    
在实际应用中，由于compute方法中要自己写拆分逻辑，因此应用不多。
大多数的应用实际上是在StreamAPI的并行流中使用
