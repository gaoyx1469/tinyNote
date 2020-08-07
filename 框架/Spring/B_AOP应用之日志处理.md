#AOP日志处理
1. 自定义切面类、日志实体类，准备日志建表语句
2. 切面类使用反射获取类名、方法名、URL等
3. 自定义service接口、实现类、dao接口
4. 切面类调用service接口记录日志
5. 示例  
    web.xml  
    bean_aop_log.xml  
    springmvc_quickStart.xml  
    spring-security-db.xml  
    LogAop  
    LogDao  
    LogEntity