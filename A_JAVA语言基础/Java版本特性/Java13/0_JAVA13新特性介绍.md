#JAVA13

JAVA13发布于2019年9月

1. 动态CDS档案
2. ZGC取消使用未使用的内存  
    JAVA12中G1的特性赋予了ZGC
3. 重新实现旧版套接字API  
    对1.0版本的Socket和ServerSocket及实现类进行了优化  
    原来的问题：底层混合了JAVA和C语言，维护调试不易；实现类使用了线程栈作为I/O的缓冲，导致在某些情况下还需要增加线程栈大小；支持异步关闭，但是是通过本地数据结构实现，存在不稳定性和并发问题。  
    因此，使用NioSocketImpl替换旧实现PlainSocketImpl  
4. switch表达式
    JAVA12中switch表达式再增强
    引入yield关键字结束switch结构，相当于switch中的return
5. 文字块  
    解决JAVA中String字符串拼接问题  
    使用三个双引号包围HTML或JSON、SQL等代码块，作为字符串，而不需要再字符串换行符等的拼接  
    简化跨越多行的字符串，避免对换行等特殊字符的转译，增强可读性  
6. 其它新特性
    * FileSystems新增newFileSystem方法
    * 支持unicode12.1
    * ZGC最大堆空间增至16T