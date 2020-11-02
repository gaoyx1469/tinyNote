#log4j2

###简介
log4j2是Apache参考了logback，在log4j基础上修复了一些问题，提升了性能的日志框架。
优点有：
* 异常处理机制升级
* 性能提升，超越logback
* 自动重载配置，生产上可以动态修改日志级别而不用重启应用
* 无垃圾机制，对象复用

###快速入门
由于log4j2常常搭配slf4j日志门面使用，因此导入依赖后直接使用slf4j的API即可
```
<!--log4j-core已经依赖log4j-api-->
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.13.3</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.25</version>
</dependency>
```
###日志级别
若自己单独使用，默认输出ERROR级别及以上的日志

###使用日志门面
```
<!--log4j-slf4j-impl已经依赖slf4j-api和log4j-api和log4j-core-->
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-slf4j-impl</artifactId>
    <version>2.13.3</version>
</dependency>
```

###配置文件
src\main\resources\log4j2.xml

###异步日志
依赖导入
```
<dependency>
    <groupId>com.lmax</groupId>
    <artifactId>disruptor</artifactId>
    <version>3.4.2</version>
</dependency>
```
#####AsyncAppender方式
此种方式，性能提升不多，不常用

#####AsyncLogger方式
分为全局异步和混合异步
全局异步：在类路径下增加一个文件log4j2.component.properties，内容是```Log4jContextSelector=org.apache.logging.log4j.core.async.AsyncLoggerContextSelector```  
混合异步即自定义异步，xml文件配置如下即可
```
<!--includeLocation是否包含方法和行号信息，关闭后性能好，据说不关，性能还不如同步的-->
<AsyncLogger name="top.trial" level="warn" includeLocation="false" additivity="false">
    <AppenderRef ref="CONSOLE_OUT"/>
</AsyncLogger>
```

###无垃圾机制介绍
重用对象以及善用缓冲区，尽可能不分配对象，低垃圾模式