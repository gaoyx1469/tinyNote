#SSM Demo
1. 基础搭建  
    + 数据库及测试表准备
    + 目录规整
    + pom文件规整
2. 配置文件  
    + log4j配置文件
    + SpringMVC配置文件 spring-web.xml  
        * 配置Controller扫描目录  
        * 配置视图解析器
        * 配置SpringMVC注解支持
        * 可选-配置自定义类型转换器
        * 可选-配置SpringMVC拦截器
        * 可选-配置文件解析器
        * 可选-配置资源目录【取消前端控制器对静态文件的拦截】
    + 业务层配置文件   spring-service.xml
        * 配置Service扫描目录
        * 配置事务管理器
        * 配置基于注解的声明式事务控制
    + DAO层及事务配置文件   spring-dao.xml
        * 配置数据源【来自数据库连接池】
        * 配置SqlSessionFactory对象【属性有数据源和Mybatis主配置文件】
        * 配置MapperScannerConfigurer对象【属性有sqlSessionFactoryBeanName和basePackage】
    + AOP配置文件   spring-aop.xml
        * 配置AOP注解支持
    + MyBatis配置文件 mybatis.xml
        * 配置驼峰命名转换
        * 配置其它全局配置【全局懒加载、立即加载所有的延迟加载属性、二级缓存等】
    + 数据源配置文件【可选DBCP或C3P0】
    + web.xml配置
        * 配置context-param标签，加载所有Spring配置文件
        * 配置Spring提供的字符编码过滤器
        * 配置前端控制器，加载所有Spring配置文件
3. 编写domain、dao、service、serviceImpl、controller代码