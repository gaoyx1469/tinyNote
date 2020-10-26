# Starter

SpringBoot的starter，若要自定义，需要包括：

1. 自定义场景所需的依赖
2. 自动配置类编写
3. 启动器组织形式



### 自定义自动配置类

```java
//类上
@Configuration//定义当前类为配置类
@ConditionalOnXXX//定义只有当满足条件时，自动配置类才加载
@AutoConfigureAfter//定义自动配置类加载顺序
@AutoConfigureOrder//自动配置类的顺序，这个干啥的？

//方法上
@Bean
@ConditionalOnXXX//定义只有当满足条件时，Bean才注入容器
@Import
@EnableConfigurationProperties//启用指定类的@ConfigurationProperties功能，使得properties类可以绑定到容器中
```



自动配置类编写完毕，将其配置到MATE-INF/spring.factories中的EnableAutoConfiguration中



### 自定义properties类

```java
//类上
@ConfigurationProperties//指定配置文件中的前缀，配置自动注入到该类对象中
```



### 启动器组织形式

启动器是一个空的jar文件 ，仅提供依赖管理，由其依赖另一个自动配置模块

通常，XXX-starter的依赖文件中包含XXX-starter-autoconfigurer

XXX-starter-autoconfigurer模块作为主体模块，包含自动配置类和properties类以及MATE-INF/spring.factories。