#slf4j

###slf4j简介
Simple Logging Facade For Java java简单日志门面  
优化了JCL扩展性不强的问题，可实现日志框架的绑定和桥接

###常用API


###快速入门
   首先导入依赖，包括slf4j的api和内置的简单实现
```
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.25</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-simple</artifactId>
    <version>1.7.30</version>
</dependency>
```
   快速入门代码
    top.trial.logging.Slf4jQuickStart

###日志级别
一共五个级别，默认输出info及以上级别的日志信息   
ERROR  
WARN  
INFO  
DEBUG  
TRACE  

