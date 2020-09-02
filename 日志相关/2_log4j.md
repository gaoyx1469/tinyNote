#log4j

###log4j简介
   Apache旗下的开源日志框架，可控制日志信息输出到控制台、文件和数据库，可控制日志输出格式和输出级别，日志输出的过程控制更为灵活
 
###快速入门
   * 首先导入依赖  
```
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.12</version>
</dependency>
```  
   * 示例代码 top.trial.logging.Log4jQuickStart
   
###日志级别
   log4j 默认日志级别是DEBUG  
   OFF      关闭日志输出  
   FATAL    严重错误  
   ERROR    错误  
   WARN     警告  
   INFO     信息  
   DEBUG    debug信息
   TRACE    程序所有流程信息
   ALL      全部日志输出

###组件
   Loggers   日志记录器，作为入口程序，应用程序获取Logger对象并调用其API进行日志发布，控制日志输出级别及是否输出日志，继承已经废弃的Category  
   Appenders 也称为Handlers，指定日志的输出方式，log4j已经提供了以下几种Appender  
        ConsoleAppender输出到控制台  
        FileAppender输出到单个文件  
            DailyRollingFileAppender输出到文件，每天生成的文件不同  
            RollingFileAppender输出到文件，达到指定大小后产生新文件  
        JDBCAppender输出到数据库  
   Layouts    也称为Formatters，Handlers的具体实现，决定了日志的呈现形式【格式】，log4j提供以下集中Layout 
        HTMLLayout  
        XMLLayout
        SimpleLayout
        PatternLayout
   Level     日志级别，继承已经废弃的Priority  
   
###Logger的父子关系
   同JUL一样，根据getLogger方法的参数的包结构，logger对象形成一串继承关系

###配置文件
   配置文件名称固定为log4j.properties或log4j.xml，放在类路径下  
   如src\main\resources\log4j.properties  
   若要使用自己的配置文件，直接放到类路径下即可，无需其他操作，log4j会自动读取该配置文件信息来初始化  
###其它功能
   开启log4j自身的内置日志记录，可以看到log4j自身的加载流程等信息
   ```LogLog.setInternalDebugging(true)```  
   
   Layout的ConversionPattern占位符说明
   ```
    %m  输出日志信息，即方法中的message
    %p  输出当前日志优先级
    %n  换行符
    %r  输出从应用启动到输出该日志语句的毫秒数
    %c  输出打印语句所属类全名
    %t  输出产生该日志的线程全名
    %d  输出服务器当前时间，默认ISO8601格式，也可指定格式如：%d{yyyy年MM月dd日 HH:mm:ss}
    %l  输出日志发生的位置，包括类名、线程名和行数
    %F  输出日志消息产生时所在的文件名称
    %L  输出代码中的行号
    %%  输出%

    同时，在%与字符之间可以加修饰符控制其它属性，如
    %5c     表示输出打印语句所属类全名且最小宽度为5，若宽度不足，右对齐
    %-5c    表示输出打印语句所属类全名且最小宽度为5，若宽度不足，左对齐
    %.5c    表示输出打印语句所属类全名且最大宽度为5，若宽度超出，只保留后半部分
    %5.10c  表示输出打印语句所属类全名且最小宽度为5且最大宽度为10，若宽度不足5，补空格右对齐，若宽度大于10，保留后半部分
```  
   