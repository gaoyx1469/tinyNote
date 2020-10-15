#Web开发基础

###1、SpringBoot对静态资源的映射规则
#####1、一般静态资源
规则写在SpringBoot的SpringMVC WEB模块的配置类：WebMvcAutoConfiguration中的addResourceHandlers方法中
    外部静态文件如jQuery：
        /webjars/**  --->   classpath:/META-INF/resources/webjars/
        所有webjars下的请求都去/META-INF/resources/webjars/下找资源，webjars是以jar包的方式引入静态资源。
            比如将jQuery.js以maven依赖的方式导入，其jquery.js在jar包中就是在/META-INF/resources/webjars/路径下。
        在访问时只需要写webjars下的路径即可。
    自己的静态文件：
        /**   --->  { "classpath:/META-INF/resources/", "classpath:/resources/", "classpath:/static/", "classpath:/public/" }
        其余所有请求，去上述数组中的路径还有当前项目根路径下寻找资源。
        上述都属于静态资源文件夹
#####2、欢迎页映射规则
规则写在SpringBoot的SpringMVC WEB模块的配置类：WebMvcAutoConfiguration中的welcomePageHandlerMapping方法中
    映射静态文件夹下的所有index.html页面
#####3、项目图标映射
规则
    映射静态文件夹下的所有favicon.ico

###2、模板引擎
常见有JSP、Velocity、Freemarker、Thymeleaf【SpringBoot推荐】
思想：将包含动态表达式的页面模板和数据组装起来，向前端返回
Thymeleaf特点是语法简单功能强大。
#####SpringBoot中使用Thymeleaf的方式
pom文件导入spring-boot-starter-thymeleaf依赖
SpringBoot中thymeleaf默认规则见autoconfigure包中的ThymeleafProperties：
    private static final Charset DEFAULT_ENCODING = StandardCharsets.UTF_8;
    public static final String DEFAULT_PREFIX = "classpath:/templates/";
    public static final String DEFAULT_SUFFIX = ".html";
html中导入名称空间，就会有提示：
    <html xmlns:th="www.thymeleaf.org">
使用示例见：
    java-springboot/src/main/resources/templates/base/thymeleafTest.html
    top.trial.springboot.controller.ThymeleafTestController
Thymeleaf语法规则参考官方文档

###3、MVC配置

#####SpringBoot对MVC做了哪些默认配置，怎样扩展和定制MVC配置
默认配置了两个视图解析器ViewResolver：
    ContentNegotiatingViewResolver：组合所有视图解析器。因此自定义视图解析器只要实现ViewResolver接口，就能添加进视图解析器集合
默认配置了静态首页index.html
默认配置了icon  favicon.ico
默认配置了静态文件夹路径和Webjars路径
自动注册了Converter、GenericConverter、Formatter，用来自动注入参数值
    Converter是转换器，用来类型转换，如把字符串转换成对象
    Formatter是格式化器，用来格式化数据
    自定义转换器和格式化器，同样放到容器中就能用
支持HttpMessageConverters消息转换器，
自动注册了MessageCodesResolver定义错误代码生成规则
自动使用了ConfigurableWebBindingInitializer

要【扩展】配置，比如原SpringMVC中XML配置文件的mvc:view-controller/mvc:interceptors等mvc标签
    需要定义一个配置类，@Configuration修饰，类型必须是WebMvcConfigurerAdapter，且不能有@EnableWebMvc注解修饰
    WebMvcConfigurerAdapter是抽象类，其所有方法都默认空实现，自己配置时用到哪个重写哪个
    @EnableWebMvc注解会将自动配置的所有MVC配置全部失效，由开发者接管
    注意：WebMvcConfigurerAdapter已废弃，SpringBoot新版本的方式：
        Spring 5.0后，WebMvcConfigurerAdapter被废弃，取代的方法有两种：
        1、implements WebMvcConfigurer（官方推荐）
        2、extends WebMvcConfigurationSupport