#JUL

###JUL简介
   JUL:Java Util Logging，java原生日志框架，不用导入第三方类库，功能简单，适合小型应用
   
###核心API
   Logger   日志记录器，作为入口程序，应用程序获取Logger对象并调用其API进行日志发布  
   Appenders    也称为Handlers，每个Logger关联一组Handlers，实现日志的数据转换和格式化  
   Layouts  也称为Formatters，Handlers的具体实现，决定了日志的呈现形式  
   Level    日志级别  
   Filters  过滤器，定制需要记录的信息和不需要记录的信息
   
###快速入门
  top.trial.logging.JULQuickStart
  
###JUL日志级别
   JUL 默认日志级别是INFO
   OFF      关闭日志输出  
   SEVERE   错误  
   WARNING  警告  
   INFO     信息  
   CONFIG   配置  
   FINE     debug信息  
   FINER    debug信息  
   FINEST   debug信息  
   ALL      全部信息都输出  
   自定义配置日志级别
###Logger的父子关系
   根据getLogger方法的参数的包结构，logger对象形成一串继承关系，顶级是LogManager$RootLogger，
   在其中定义了默认的日志输出级别等信息，子logger对象使用父logger对象的默认的日志输出级别等信息  
   使用setUseParentHandlers(false)可以使当前logger对象与其父logger对象的配置脱钩  
   示例：  
    top.trial.logging.JUCLogLevel演示不使用父logger的配置，改用自定义配置  
    top.trial.logging.JUCLoggerMsg演示父子关系
###配置文件
   src/main/resources/logging.properties复制的jdk默认配置，源文件位置在conf下  
   若要使用自己的配置文件，需要自己调用LogManager.getLogManager().readConfiguration(配置文件输入流)
   若要修改Formatters，指定其属性format的值即可  
   若要修改日志输出Handler的追加属性，指定其属性append为true即可
    