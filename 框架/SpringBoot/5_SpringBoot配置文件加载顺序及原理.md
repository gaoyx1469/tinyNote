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
@EnableAutoConfiguration注解中的@Import(AutoConfigurationImportSelector.class)注解进行自动配置的加载。
具体到方法上是AutoConfigurationImportSelector类中selectImports()方法，方法内调用getAutoConfigurationEntry()方法完成的；
方法中getCandidateConfigurations方法，作用是获取类路径下META-INF/spring.factories文件中配置的所有EnableAutoConfiguration的值，形成一个List<String>返回。
后续方法是去重等操作
获取到的每个configuration字符串对应的类都是配置类，以HttpEncodingAutoConfiguration为例，类上面注解如下
    @Configuration(proxyBeanMethods = false)//表明是配置类
    @EnableConfigurationProperties(ServerProperties.class)//启用指定类的ConfigurationProperties功能，允许指定类从全局配置文件加载绑定数据并将该类添加到IOC容器
        注意：使用@EnableConfigurationProperties与@ConfigurationProperties搭配使用跟@Component与@ConfigurationProperties搭配使用效果一致
        但是这么用，就将注入的控制权交给类@EnableConfigurationProperties注解修饰的类，而不是不管不顾全都绑定注入
    @ConditionalOnWebApplication(type = ConditionalOnWebApplication.Type.SERVLET)//基于Spring的@Conditional注解，注解作用是满足条件才加载该配置文件
        因此，这个注解作用是web应用的servlet-based web application才加载该配置文件
    @ConditionalOnClass(CharacterEncodingFilter.class)//注解作用是判断项目中有没有CharacterEncodingFilter这个类，没有也不加载本配置文件
    @ConditionalOnProperty(prefix = "server.servlet.encoding", value = "enabled", matchIfMissing = true)//判断配置文件中是否存在指定配置
    
SpringBoot通过 @Conditional*系列注解控制拿到的/spring.factories文件中的自动配置类中哪些生效哪些不生效。
我们可以通过debug=true配置SpringBoot开启debug模式，启动时控制台日志打印自动配置加载报告。可以看到哪些自动配置类生效了，哪些没有生效。
