#JAVA14

JAVA14发布于2020年3月

1. instanceof模式匹配增强，可以为匹配的模式定义变量名，然后在后面直接用，算是替我们强转了
2. 实用的空指针异常
    * 空指针异常会使代码膨胀->代码充斥着各种嵌套的null检查
    * null本身无意义无语义
    * 破坏java哲学，让程序员感知指针的存在
    * null的传递导致类型模糊
    * JAVA8提供了Optional进行部分优化
    * JAVA14提供特性更好地展示在哪里出现了空指针，使用 -XX:+ShowCodeDetailsInExceptionMessages 开启
3. Record
    * JAVABEAN中的get/set方法、构造函数、equals、hashcode、toString等方法自动生成，价值低
    * JAVA14 Record类型引入，实际上编译后就是含构造器及上述各种方法且属性为final的类
    * Record类型可以定义静态字段、静态方法、构造器和实例方法
    * Record类型不能声明为abstract，不能声明显式的父类，因为已经有父类了
4. switch在JAVA12和JAVA13中的预览特性升级为正式特性
5. text blocks增加两个转译符 \ 取消换行操作 和 \s 表示一个空格
6. 弃用ParallelScavenge 和 SerialOld GC的组合
7. 删除CMS垃圾回收器
    * 会产生内存碎片
    * 对CPU资源敏感，可能降低性能
    * 无法处理浮动垃圾
8. ZGC可以在MacOS和Windows上使用了 -XX:+UnlockExperimentalVMOptions -XX:+UseZGC
9. 其它新特性