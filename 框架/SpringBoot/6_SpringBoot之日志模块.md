#SpringBoot日志模块
此处需要日志框架基础，具体见 日志相关 部分
SpringBoot默认采用了slf4j+logback

###日志使用方式
原本slf4j的使用方式参考top.trial.logging.Slf4jQuickStart
```
public static final Logger LOGGER = LoggerFactory.getLogger(Slf4jQuickStart.class);
方法中调用如下方法即可
        LOGGER.error("error");
        LOGGER.warn("warn");
        LOGGER.info("info");
        LOGGER.debug("debug");
        LOGGER.trace("trace");
```

由于项目导入不同的框架所使用的日志记录的框架可能各有不同，因此需要进行统一，都使用slf4j+logback实现，都使用一套logback的日志配置文件。
怎么做呢:
    先排除系统中其它日志框架
    使用桥接器，让除了最终要用的实现之外的其它日志实现都调用slf4j
    增加slf4j以及最终要要用的实现
    在slf4j和实现之间加上集成器【老的框架需要，logback和slf4j2不需要】
Springboot就是这样实现的，具体是spring-boot-starter-logging。
!!!注意：如果自己引入其它框架，一定要手动移除框架带进来的默认日志框架实现，即在dependency中使用<exclusions>标签把原日志框架删掉。

###日志配置
SpringBoot已经有默认日志配置
可以在全局配置文件中使用logging.level.包名 配置指定的包的日志输出级别
同时还能配置其它东西
logging.file指定日志输出文件【如果只写文件名，将生成在当前项目下，也可以指定完整路径】
logging.path指定日志输出路径【指定了logging.file的话，此项不生效】【日志生成到指定路径下的spring.log中】
默认输出到文件中的日志是采用追加而不是覆盖的方式，默认10MB大小滚动
若想使用自己的配置文件，只需要在类路径下放置指定名称的文件即可
    logback:logback-spring.xml/logback.xml
    log4j2:log4j2-spring.xml/log4j2.xml
        【前者由SpringBoot识别加载，因此可以有些定制功能比如使用SpringProfile标签定义不同环境的独特配置（name属性填写环境名或者!环境名），后者由日志框架直接识别】
    JUL:logging.properties