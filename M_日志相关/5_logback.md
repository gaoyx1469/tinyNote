#logback

###logback简介
logback是原log4j工程师搞的，性能强于log4j，经常使用

###快速入门
由于logback常常搭配slf4j日志门面使用，因此导入依赖后直接使用slf4j的API即可
```
<!--logback-classic已经依赖slf4j-api-->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.3</version>
</dependency>
```

###日志级别
默认搭配slf4j使用，默认输出DEBUG及以上级别日志

###配置文件
依次读取logback.groovy、logback-test.xml、logback.xml
若都没有，采用默认配置
示例：src\main\resources\logback.xml
```
日志输出格式
    %-5level                    日志级别，最长长度5，左对齐
    %d{yyyy-MM-dd HH:mm:ss.SSS} 日期
    %c                          全类名
    %M                          方法名
    %L                          行号
    %thread                     线程名
    %m或%msg                    日志信息
    %n                          换行
```

###access
略