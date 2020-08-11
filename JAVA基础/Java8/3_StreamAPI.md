#Stream API

java.util.stream.*

1. 
    数据源：集合、数组；  
    中间操作：对数据源进行各种操作（计算）  
    操作结果：产生新的持有结果的流，原数据源不发生变化  
    Stream是延迟操作，只有存在终止操作才开始执行中间操作链，没有终止操作，中间操作链都不执行。【惰性求值】
2.  获取流的四种方式  
    * Collection集合提供的stream方法或parallelStream方法获取集合数据源的流
    * Arrays类提供的stream方法获取数组数据源的流
    * Stream类提供的of方法获取数据源的流
    * 无限流-迭代：Stream类提供的iterator方法，提供种子和操作，返回无限流
    * 无限流-生成：Stream类提供的generate方法，提供操作，返回无限流
3. 中间操作
    * 筛选和切片
        * filter 接收断言型Lambda表达式，排除掉流中某些元素
        * limit  截断流，使元素数量不超过某个值
        * skip   截断流，跳过前n个元素
        * distinct 筛选去重，通过元素的hashcode和equals去重
    * 映射
        * map 接收函数型Lambda表达式，对每个元素进行函数操作
        * flatMap 接收函数型Lambda表达式，对每个元素进行函数操作，当函数操作返回的是流的时候，将返回的流拼接成一个流
    * 排序
        * sorted 自然排序或接收比较器排序，比较器可使用Lambda表达式
4.  终止操作
    * 查找与匹配
        * allMatch 检查是否匹配所有元素，传入断定型Lambda表达式，返回boolean类型
        * anyMatch 
        * noneMatch 
        * findFirst 无参，返回Optional对象
        * findAny   无参，返回Optional对象
        * count 返回Long类型
        * max   传入比较器，可使用Lambda表达式，返回Optional对象
        * min   传入比较器，可使用Lambda表达式，返回Optional对象
    * 归约
        * reduce(BinaryOperation bo)/reduce(T identity,BinaryOperation bo)将流中元素反复结合起来，得到一个值  
            解释：传入identity作为二元运算的第一个值，流中元素依次作为第二个值，结果再次作为第一个值，最终得到结果。  
            与map方法结合形成map-reduce模式
    * 收集
        * collection 将流转换为其他格式，接收一个收集器Collector接口，JDK提供了Collectors工具类有很多静态方法如toList等
            * 可以筛选
            * 可以分组
            * 可以分区
     
5. 并行流和串行流  
    底层是fork/join模式  
    * parallel方法和sequential方法切换数据流的并行串行