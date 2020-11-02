#JAVA11

JAVA11LTS发布于2018年9月

1. 新增String中字符串的几个处理方法
    * isBlank   是否是空白的【空格、制表符、换行符都算空白】
    * strip     去除首位空白【空格、制表符、换行符】
    * stripTrailing     去除尾部空白
    * stripLeading      去除首部空白
    * repeat(count)     原字符串重复count次，生成新字符串
    * lines     按行获取字符串流
2. Optional继续增强，新增几个方法
    * isEmpty   与isPresent正好相反
3. 局部变量类型推断升级  
    var类型上可以加注解
4. 全新的HTTP客户端API，对JAVA9再升级【详见1_HTTP客户端API】
5. 简化编译运行，cmd中直接java+java源文件，不需要先javac了【要求：执行java文件中第一个类，其中必须有主方法，引用的类必须是本文件中存在的，不能引用其它文件的类】
6. 废除了Nashorn引擎，JAVA8出现，JAVA9增强，JAVA11废除，建议用Graal
7. ZGC【并发的、基于region、压缩型的垃圾回收器】
    * GC暂停时间不超10ms，仅root扫描阶段STW
    * 能处理更大的堆（几个T）
    * 和G1相比，应用吞吐能力下降小于15%
    * 支持64位系统，不支持32位系统
    * 可扩展的实现机制
8. 其它特性
    * JFR Flight Recorder
    * Epsilon GC【作用：性能测试、内存溢出测试、短任务、VM接口测试、延迟&吞吐改进】
    * 支持unicode10
    * TLS1.3