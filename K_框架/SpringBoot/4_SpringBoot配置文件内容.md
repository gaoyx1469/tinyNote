# 配置文件

### SpringBoot使用一个全局配置文件

可以是以下两者之一，名称是固定的，若都存在，以properties为主
application.properties
application.yml
具体能配置什么内容，可以参考SpringBoot官方文档附录部分。

##### YAML语法

1、基本使用 key:空格value表示基本键值对
2、层级关系表示：以空格缩进控制层级关系，左对齐的一列数据都是同一层级的。
3、key和value都是大小写敏感
4、值的写法：
    字面量：直接key: value，不用加引号
        若加上引号，双引号中特殊字符会展示效果，如\n会换行；而单引号中特殊字符会作为字面量，就是输出字符串\n
        不加引号与加单引号效果一致

        ```
        port: 8090
        ```
​        注意：日期格式是yyyy/MM/dd
​    对象、Map:
​        第一种可以使用层级缩进，即value对象部分为换行缩进的字面量形式；
        ```
        socket:
            port: 8090
            desc: 90
        ```
​        第二种可以在一行中使用类似json的大括号形式表示，同样大括号中为字面量形式并用逗号分割
        ```
        socket: {port: 8090,desc: 90}
        ```
​    数组、集合：
​        第一种是使用 -空格value表示数组中的一个元素
        ```
        socket:
         - 8080
         - 8090
        ```
​        第二种可以在一行中使用中括号括起每个元素。注意冒号后的空格。
        ```
        socket: [8080,8090]
        ```

### 配置文件的值的注入

1、新建javabean，定义需要注入的属性
2、在javabean上增加@Component和@ConfigurationProperties(prefix="配置文件中属性的前缀")
    @Component将javabean交给容器管理
    @ConfigurationProperties将全局配置文件中的参数与javabean中的属性进行绑定
        @ConfigurationProperties相当于对javabean中的所有基本属性都加上了Value，进行松散注入【多种形式，可以是驼峰命名、下划线分割、中划线分割等都支持】；
            同时支持JSR303数据校验，即类上增加@Validated注解，属性上可以使用校验注解进行校验【比如@Email】
3、在pom.xml中导入依赖spring-boot-configuration-processor
    这是配置文件处理器，配置文件进行绑定会有提示。
        注意，自定义javabean配上 @ConfigurationProperties写完后需要运行一次项目之后，自定义javabean才会有提示。
示例：
    top.trial.springboot.entity.BindingTestJavaBean
    resources\application.yml

##### @PropertiesSource

加载指定的配置文件，而不是仅能加载全局配置文件【用于配置文件拆分，避免所有配置全部配置在全局配置文件中】
注解加在需要使用该配置文件内容的类上
value属性是数组，填写配置文件的路径，如classpath:xxx.properties

##### @ImportSource

导入Spring的配置文件，让其中的内容生效
注解加在spring boot的配置类上，即跟@SpringBootApplication放一块就行
locations属性是数组，填写spring配置文件的路径，如classpath:xxx.xml
但是，SpringBoot推荐全注解形式，因此这个不常用，而是使用Spring配置类的方式引入组件，使用方式：
    1、@Configuration注解定义一个配置类；【SpringBoot能自动扫描到该配置类，并加载配置】
    2、@Bean、@ComponentScan、@Import、@propertySource等注解的使用，完全代替XML形式

### 配置文件中的随机数和占位符

配置文件中可以使用随机数，如${random.value}/${random.int}/${random.long}/${random.int(10)}/${random.int[1024,65536]}形式
同时可以使用占位符，${key名}/${key名:默认值}，没设置默认值的情况，如果key没定义，则占位符会作为字符串字面量绑定

### Profile

Profile是Spring对不同环境提供不同配置功能的支持，可通过激活、指定参数等方式快速切换

##### 文件形式

我们可以创建多个配置文件application-{profile}.properties/yml
比如application-dev.yml/application-prod.yml
默认加载的是不带后缀的配置文件
    1、在全局配置文件中，使用spring.profiles.active=dev，就能激活application-dev.yml或application-dev.properties配置文件，加载其中的配置
    2、另外，如果配置文件是yml格式，还支持更方便的多文档块的方式，即在一个yml文件中使用---可以将文档分块，使用方式：

    ```
    server:
        port: 8081
    spring:
        profiles:
            active: dev
    ---
    server:
        port: 8082
    spring:
        profiles: dev
    ---
    server:
        port: 8083
    spring:
        profiles: prod
    ```
##### 激活方式

1、前面已经介绍了在配置文件中直接使用spring.profiles.active=dev 来激活
2、命令行激活：--spring.profiles.active=dev 来激活【优先级高于方式一】
3、虚拟机参数激活: -Dspring.profiles.active=dev 来激活