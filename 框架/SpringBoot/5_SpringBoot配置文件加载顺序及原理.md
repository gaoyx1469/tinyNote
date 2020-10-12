#配置文件加载顺序及原理

###配置文件加载位置
SpringBoot加载以下位置的application.properties或application.yml作为默认全局配置文件：
    文件路径下config文件夹中
    文件路径下根目录
    类路径下config文件夹中
    类路径下根目录【这个是默认位置】
上述四个位置优先级从高到低全部加载，重复属性优先级高的覆盖优先级低的
可以通过spring.config.location来改变默认配置，用法是：
    使用命令行参数的形式，启动项目时指定配置文件的新位置；该指定的配置文件将与原配置文件共同起作用，该指定的配置文件优先级最高。

###SpringBoot外部配置加载顺序
1、命令行参数指定的位置
2、来自java:comp/env的JNDI属性
3、java系统属性（System.getProperties）
4、操作系统环境变量
5、RandomValuePropertySource配置的random.*属性
6、jar包外部的Profile配置文件，application-{profile}.properties或application.yml【内含 spring:profiles:】
7、jar包内部的Profile配置文件
8、jar包外部的全局配置文件，properties或yml
9、jar包内部的全局配置文件
10、@Configuration注解类上的@PropertiesSource
11、SpringApplication.setDefaultProperties指定的默认属性
上述十一个位置优先级从高到低全部加载，重复属性优先级高的覆盖优先级低的

###SpringBoot配置加载原理
