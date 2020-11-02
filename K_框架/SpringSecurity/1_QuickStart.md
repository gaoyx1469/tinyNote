# QuickStart

1. 使用配置文件认证
    * pom.xml导入依赖
        ```
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-web</artifactId>
            <version>${spring.security.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
            <version>${spring.security.version}</version>
        </dependency>
        ```
    * web.xml配置过滤器
        ```
        <!--配置读取spring-security.xml配置文件-->
        <context-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:top/trial/springsecurity/spring-security.xml</param-value>
        </context-param>
        <!--配置SpringSecurity的过滤器，配置在所有过滤器最前面-->
        <filter>
            <filter-name>springSecurityFilterChain</filter-name><!--这个name不能改-->
            <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
        </filter>
        <filter-mapping>
            <filter-name>springSecurityFilterChain</filter-name>
            <url-pattern>/*</url-pattern>
        </filter-mapping>
        <!--配置spring配置文件加载监听器-->
        <listener>
            <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
        </listener>
        ```
    * 核心配置文件spring-security.xml编写
        ```
        <security:http auto-config="true" use-expressions="false">
            <!--intercept-url定义过滤规则，pattern是对哪些url过滤，access是请求该url需要什么权限-->
            <security:intercept-url pattern="/**" access="ROLE_USER"/>
        </security:http>
      
        <security:authentication-manager>
            <security:authentication-provider>
                <security:user-service><!--配置用户名、密码和角色，密码前的{noop}表示不加密，其它还有{sha256}/{scrypt}/bcrypt}/{pbkdf2}等，不指定会报错-->
                    <security:user name="user" password="{noop}user" authorities="ROLE_USER"/>
                    <security:user name="admin" password="{noop}admin" authorities="ROLE_ADMIN"/>
                </security:user-service>
                <!--从配置文件配置用户角色，只有第一个security:user-service标签能载入
                <security:user-service properties="classpath:top/trial/springsecurity/users.properties"/>-->
            </security:authentication-provider>
        </security:authentication-manager>
        ```
    * 启动服务即进入登录页面
    
2. 自定义登录页和登录失败页
    * 核心配置文件spring-security.xml
    ```
    <!--配置不需要进行登录控制的页面-->
    <security:http security="none" pattern="/html/springsecurity/login.html"/>
    <security:http security="none" pattern="/html/springsecurity/failure.html"/>

    <security:http auto-config="true" use-expressions="false">
        <!--intercept-url定义过滤规则，pattern是对哪些url过滤，access是请求该url需要什么权限,作用和上面security="none"一样，选一种设置就行-->
        <!--设置放行权限，ROLE_ANONYMOUS也行-->
        <security:intercept-url pattern="/html/springsecurity/login.html" access="IS_AUTHENTICATED_ANONYMOUSLY"/>
        <!--设置放行权限，ROLE_ANONYMOUS也行-->
        <security:intercept-url pattern="/html/springsecurity/failure.html" access="IS_AUTHENTICATED_ANONYMOUSLY"/>
        <security:intercept-url pattern="/**" access="ROLE_USER"/>
        <!--配置自定义登录页面-->
        <security:form-login login-page="/html/springsecurity/login.html" 
                             login-processing-url="/html/springsecurity/login"
                             username-parameter="username" password-parameter="password"
                             authentication-failure-url="/html/springsecurity/failure.html"
                             default-target-url="/html/springsecurity/success.html"
                             authentication-success-forward-url="/html/springsecurity/success.html" />
        <!--关闭CSRF，默认是开启的-->
        <security:csrf disabled="true"/>
        <!--弹窗登录，当与form-login同时存在时，以form-login为主
        <security:http-basic/>-->
    </security:http>
    ```
   * 编写登录页、成功页、失败页配置进去