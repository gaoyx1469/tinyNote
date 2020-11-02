# QuickStart

### 环境准备

JDK14
Maven3.6.3
IDEA2020.1

### QuickStart

1、导入SpringBoot依赖

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.3.4.RELEASE</version>
        </dependency>
    </dependencies>  

如果是一个项目而不是一个module，可以配置parent父项目

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
    </parent>

2、创建主程序
    创建@SpringBootApplication修饰的主程序类，main方法中调用SpringApplication.run()实现启动
3、编写相关Controller、Service

    @Controller
    public class QuickStartController {
        @RequestMapping("/hello")
        @ResponseBody
        public String hello() {
            return "Hello Spring Boot";
        }
    }
4、运行
    直接执行main方法，浏览器中访问localhost:8080/hello即可

5、部署
    在pom.xml中增加打包插件
    
    <build>
        <plugins>
            <!--将应用打成可执行jar包的插件-->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>2.3.4.RELEASE</version>
                <!--加入下面两项配置后，maven打包时才会把第三方jar包一起打入-->
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <includeSystemScope>true</includeSystemScope>
                </configuration>
            </plugin>
        </plugins>
    </build>
    
    这样打出的jar包将是可执行文件，使用java -jar jar包名  即可执行