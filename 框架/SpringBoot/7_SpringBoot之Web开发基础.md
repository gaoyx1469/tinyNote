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
    
