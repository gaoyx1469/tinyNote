# SpringBoot之数据访问



### 一、整合JDBC

1. 使用初始化向导新建项目，选择依赖SQL-MySQL和JDBC，分别是导入mysql驱动和原生JDBC依赖；
2. 在全局配置文件中配置数据库参数
   1. spring.datasource 下的用户名、密码、URL、驱动类名。SpringBoot将加载到DataSourceProperties类中
3. 在使用时，直接使用@AutoWire注入数据源即可，也可以注入JdbcTemplate
4. 默认使用tomcat数据源，若要修改数据源（改用C3P0/DBCP/DBCP2/druid等）：
   1. 在spring.datasource 下配置type属性，值就是想要用的数据源的全类名
   2. 若有数据源特有的属性要配置，可以新建一个@Configuration修饰的配置类，@Bean修饰方法返回自己new出来的数据源，方法上加@ConfigurationProperties(Prefix="spring.datasource")，即可将spring.datasource下的参数绑定到数据源的对应属性中。
   3. 当使用Druid数据源，如何配置监控？
      1. 配置Durid的管理后台的Servlet，方式是采用【A_嵌入式Servlet容器相关配置】中注册Servlet三大组件的Servlet的方式，注册进Durid的管理后台的Servlet类-StatViewServlet
      2. 配置Druid的web监控Filter，方式同样参考【A_嵌入式Servlet容器相关配置】，Druid的监控Filter是-WebStatFilter

### 二、整合Mybatis

1. 使用初始化向导新建项目，选择依赖SQL-MySQL和MyBatis
2. 在全局配置文件中配置数据库参数
3. 配置数据源
4. 编写JavaBean
5. 编写dao，直接使用接口，使用@Select、@Update、@Insert进行报数据操作，使用@Mapper修饰dao
6. 直接调用dao方法进行数据库CRUD
7. 如何使用注解方式开启驼峰命名规则映射？
   1. 新建配置类，新建方法返回ConfigurationCustomizer并使用@Bean修饰将返回值添加到Spring容器，方法体使用匿名内部类，实现customize方法，方法体直接使用configuration.setMapUnderscoreToCamelCase(true);即可
8. 可以在主配置类或MyBatis配置类上添加@MapperScan注解，指定扫描的包，这样就不用在所有dao接口上添加@Mapper了
9. XML配置文件方式如何兼容过来？
   1. 编写配置文件
   2. SpringBoot全局配置文件中，增加mybatis.config-location配置MyBatis主配置文件位置，增加mybatis.mapper-locations配置SQL映射文件位置

### 三、整合JPA

1. 使用初始化向导新建项目，选择依赖SQL-MySQL和JPA
2. 在全局配置文件中配置数据库参数
3. 配置数据源
4. 编写JavaBean，JPA在此处维护映射关系
   1. @Entity修饰JavaBean，告诉JPA是一个实体类
   2. @Table修饰JavaBean，name属性填写表名，告诉JPA映射的数据库表名，省略时表名默认为类名首字母小写
   3. @Id修饰主键属性
   4. @GeneratedValue修饰主键，strategy  = GenerationType.IDENTITY 告诉JPA主键自增
   5. @Column修饰非主键属性，name属性填写列名，省略时默认列名为属性名
5. 编写DAO接口操作实体类
   1. 自己写一个接口继承JpaRepository，两个泛型传入JavaBean类型和其主键类型，不用加任何注解，注解都在其父接口上
6. 编写Service，自动注入DAO，直接调用已有方法即可
7. 其他细节
   1. jpa.hibernate.ddl-auto = update，设置每次项目启动，自动创建或修改相关表结构
   2. jpa.show-sql = true，设置显示SQL

