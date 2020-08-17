#JAVA9

JAVA9发布于2017年9月

1. 模块化系统【详见1_模块化系统】
2. jShell【详见2_jShell】
3. 多版本兼容jar包
4. 接口私有方法  
    接口支持方法的访问权限修饰符私有了
5. 钻石操作符（泛型）  
    匿名内部类也可以泛型推断了
6. try语法改进
    JAVA7中允许的在try后括号中定义资源并初始化，然后自动关闭，JAVA9进一步优化，允许资源在try之前定义初始化，将资源对象的引用传到try后括号就行，多个的话分号分隔。
7. String存储结构改变  
    之前String存储结构是char数组，由于String是堆空间中主要类型且大部分char都是一个字节能表示完的东西，用char的两个字节长度太浪费。
    因此，JAVA9改用byte数组加标志位来存储【标志位来判断当前byte是完整的还是需要和后面的组起来】，能一个字节表示的，都用一个字节表示，节省空间。
    同时StringBuffer、StringBuilder、AbstractStringBuffer都改用此结构。JVM中String相关操作同样改为支持此结构。
8. 集合特性of()
    原来要变更集合为只读集合，需要Collections类的静态方法unmodifiableList实现，如再对只读集合修改，编译可通过，执行时报错。
    JAVA9的List、Set、Map都提供了of方法，直接构造为只读集合。
9. InputStream增强  
    JAVA9中InputStream拥有了一个新方法transferTo，可以将数据直接传到输出流，不再需要创建字符数组自己read又write了，transferTo方法的实现其实就是创建byte数组read又write。
10. Stream API增强  
    Stream新增4个方法takeWhile,dropWhile,ofNullable,iterate重载，
    同时，Optional也增加stream方法转换成流对象
    * takeWhile 从前往后遍历，取满足条件的，遇到不满足的结束，不管再后面有没有满足的
    * dropWhile 跟上个相反，从前往后遍历，不要满足条件的，知道遇到不满足条件的，不管后面满不满足，全都取
    * ofNullable Stream原来的of方法的参数不可以只有一个null，多个值时允许有null值（可以多个都是null），而ofNullable接口允许参数为一个null，其不算一个元素，且打印也不会输出
    * iterate重载  新提供了重载方法，增加了一个断言接口入参，使用条件可以使无限流变为有限流
11. 新HTTP客户端API【在JAVA11中进一步升级，详见JAVA11】
12. Deprecated相关API
13. javadoc的H5支持
14. js引擎升级Nashorn（JDK11又干掉了）
15. java动态编译器