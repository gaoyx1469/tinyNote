#JCL日志门面

###基本介绍-为啥要用JCL
当有需求要切换日志框架，由于实现不同，需要改动的代码相当多，
因此需要一套接口API，统一日志记录方式，使得当切换日志框架时，只修改一两处配置，而不需要修改日志记录代码，
JCL就时兼容了JUL和log4j的API，且自己也实现了一套API：SimpleLog。  
另外，Apache收购后，2014年不再维护，因此属于过时产品

###核心API
Log 抽象类，日志记录器
LogFactory  抽象类，日志记录器工厂

###快速入门
   首先导入依赖
```
<dependency>
    <groupId>commons-logging</groupId>
    <artifactId>commons-logging</artifactId>
    <version>1.2</version>
</dependency>
```  
   实现示例
    top.trial.logging.JCLQuickStart
###支持的日志框架列表
```
LogFactoryImpl类中的
private static final String[] classesToDiscover = {
        LOGGING_IMPL_LOG4J_LOGGER,
        "org.apache.commons.logging.impl.Jdk14Logger",
        "org.apache.commons.logging.impl.Jdk13LumberjackLogger",
        "org.apache.commons.logging.impl.SimpleLog"
};
```
   