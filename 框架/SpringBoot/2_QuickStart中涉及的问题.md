# QuickStart中涉及的几个问题

### 父项目问题

对于一个独立spring boot项目，在pom文件中引入父项目：spring-boot-starter-parent
原因和作用：
    spring-boot-starter-parent的父项目是spring-boot-dependencies；
    spring-boot-dependencies的pom文件中properties定义了依赖的jar包版本
    其是用来真正管理Spring Boot应用里所有的依赖版本的项目，称为版本仲裁中心【归于其管理的依赖，我们导入后可以不用声明版本号】

### web启动器问题

​    <dependency>
​        <groupId>org.springframework.boot</groupId>
​        <artifactId>spring-boot-starter-web</artifactId>
​        <version>2.3.4.RELEASE</version>
​    </dependency>
spring-boot-starter是spring boot场景启动器
spring-boot-starter-web帮我们导入了web模块正常运行所依赖的组件

### 主入口类问题

​    @SpringBootApplication
​    public class QuickStartMainApplication {
​        public static void main(String[] args) {
​            //将Spring应用启动起来
​            SpringApplication.run(QuickStartMainApplication.class, args);
​        }
​    }
SpringApplication.run()方法执行启动，传入由@SpringBootApplication修饰的类的字节码及参数args
@SpringBootApplication注解是一个组合注解：

    @SpringBootConfiguration
    @EnableAutoConfiguration
    @ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
    		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })

##### @SpringBootConfiguration注解

@SpringBootConfiguration注解之上有@Configuration注解，而@Configuration注解是修饰Spring中配置类的。

##### @EnableAutoConfiguration注解

@EnableAutoConfiguration注解是开启自动配置功能，【包扫描、mvc配置等】
@EnableAutoConfiguration也是一个组合注解

    @AutoConfigurationPackage
    @Import(AutoConfigurationImportSelector.class)

###### @AutoConfigurationPackage注解

@AutoConfigurationPackage【其上有@Import(AutoConfigurationPackages.Registrar.class)注解，导入了一个配置类】
AutoConfigurationPackages.Registrar类中有个方法registerBeanDefinitions。
    方法中调用register方法，第二个参数是扫描的包名，传入的是注解的主配置类所在包的第一级

###### @Import(AutoConfigurationImportSelector.class)注解

@Import(AutoConfigurationImportSelector.class)，是导入组件选择器，将需要导入的组件以全类名的方式返回。
最终是导入的是所有用到的组件的自动配置类。
需要导入的组件的获取方式是拿到类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值。
spring.factories中配置的配置类，都是主要来源于spring-boot-autoconfigure包