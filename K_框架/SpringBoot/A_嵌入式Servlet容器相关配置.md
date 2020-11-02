# 嵌入式Servlet容器
SpringBoot默认使用Tomcat作为嵌入式Servlet容器

### 定制化配置

方式一：
    在全局配置文件中使用server.xxx   修改和server有关的默认配置，是绑定到ServerProperties配置类的
    ServerProperties配置类有个Tomcat的属性，因此，server.tomcat.xxx可以设置tomcat有关的默认配置。
方式二：
    编写一个嵌入式Servlet容器的定制器EmbeddedServletContainerCustomizer，来修改配置。
    写在自定义配置类中，方法由@Bean修饰，方法返回EmbeddedServletContainerCustomizer类型【返回匿名内部类对象，重写的方法中参数就是内置容器对象】
    比如container.setPort(8090);
    

### 注册Servlet三大组件Servlet、Filter、Listener

ServletRegistrationBean
    编写一个Servlet类，在自定义配置类中增加方法返回ServletRegistrationBean类型对象并由@Bean修饰，
    返回的ServletRegistrationBean类型对象直接new出来，参数第一个传入自定义的servlet对象，第二个传入想要拦截的路径。
FilterRegistrationBean
ServletListenerRegistrationBean

### 切换为其它内置Servlet容器

SpringBoot还支持Jetty和Undertow两种内置容器
    Undertow不支持JSP，并发性能好
    Jetty适合开发长连接应用，如聊天
如何切换呢？
    pom文件中排除web启动器依赖的tomcat启动器，自己引入Jetty或Undertow启动器即可
最新已支持Netty！

### 切换为外置Servlet容器

嵌入的Servlet容器的优化和定制比较复杂且不支持JSP，但是简单便携。
要切换成使用外置Servlet容器，将项目打成war包
    【   tomcat启动器指定为provided，
        增加一个ServletInitializer继承SpringBootServletInitializer，重写configure方法，方法体固定为application.source(主启动类的class)】
创建项目时选择war包方式，同时生成webapp目录，在其下的WEB-INF下创建web.xml文件即可

