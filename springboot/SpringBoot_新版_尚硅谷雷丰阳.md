# 02 SpringBoot2入门

## 1、系统要求

> Java 8
>
> Maven 3.3+
>
> Idea 2019.2

### 1.1 Maven设置

## 2、HelloWorld



```bash
#官方文档
https://spring.io/guides/gs/spring-boot/
```



### 2.1 创建父工程和子工程

![image-20201222153210098](Images\image-20201222153210098.png)

### 2.2 pom文件

父工程pom文件

```xml
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.3.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

子工程pom文件



### 2.3 主程序

### 2.4 业务

### 2.5 测试

![image-20201222154317694](Images\image-20201222154317694.png)

### 2.6 简化配置

application.yaml文件就解决了

### 2.7 简化部署

#### 1、引入pluign插件

```xml
  <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

#### 2、打包项目

![image-20201222154538024](Images\image-20201222154538024.png)

#### 3、在jar包目录下运行

```bash
java -jar boot-01-helloworld-1.0-SNAPSHOT.jar
```

![image-20201222154722989](Images\image-20201222154722989.png)





# 底层注解

## @Configuration

```java
/**
 * 1、配置类里面使用的@Bean标注的方法上给容器注册组件，默认也是单实例的。
 * 2、配置类本身也是一个组件
 * 3、proxyBeanMethods：代理bean的方法
 */
@Configuration(proxyBeanMethods = true) //告诉容器我们SpringBoot2这是一个配置类
public class MyConfig {
    /**
     * 外部无论对配置类中这个组件的注册的方法调用了多少次获取的都是之前注册的单实例对象
     * @return
     */
    @Bean(value = "user") //给容器注入Bean，这个user就是我们在容器中保存的实例了
    public User user_01(){
        return new User("zhangsan",23);
    }
}
```

在主程序中获取的到

```java
@SpringBootApplication
public class HelloWorldMain {
    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(HelloWorldMain.class, args);
		
        User user = run.getBean("user",User.class);
        System.out.println("user = " + user);

    }
}
```

![image-20201222155846938](Images\image-20201222155846938.png)

## @Import

> 导入组件

```java
 * 4、@Import({User.class, DBHelper.class})
 *      自动给容器中创建出这两个类型的组件
 */
@Import({User.class, DBHelper.class})
```

## @Conditional

> 满足@Conditiona中的条件后才生效配置

![image-20201222161152902](Images\image-20201222161152902.png)



## @ImportResource

> 允许引入springboot的配置文件让它们生效。
>
> ```java
> @ImportResource("classpath:bean.xml")
> ```

## @CongifurationProperties

> 绑定配置文件

配置文件中

```yaml
person:
  name: '里斯'
  age: '22'
```

在bean类中

```java
/**
 * 只有在IOC容器中的组件才可以想用IOC容器带来的便利
 * @ConfigurationProperties(prefix = "person")
 *      将这个类和配置文件中前缀为person的属性一一绑定
 */
@Component
@ConfigurationProperties(prefix = "person")
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String name;
    private Integer age;
}
```

![image-20201222162102733](Images\image-20201222162102733.png)





# SpringBoot自动配置原理

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
```

## @SpringBootConfiguration

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration //表明这就是一个配置类
public @interface SpringBootConfiguration {
```

## @ComponentScan

> 自动扫描包

## @EnableAutoConfiguration

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage 
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
```

### @AutoConfigurationPackage 

> 自动配置包
>
> ```java
> @Target(ElementType.TYPE)
> @Retention(RetentionPolicy.RUNTIME)
> @Documented
> @Inherited
> @Import(AutoConfigurationPackages.Registrar.class) //给容器中导入组件
> public @interface AutoConfigurationPackage {
> ```
>
> Registrar
>
> > 可以看到我们利用这个registrar批量往容器中注册东西。将指定的一个包下面的组件导入到IOC容器中。
> >
> > ```java
> > static class Registrar implements ImportBeanDefinitionRegistrar, DeterminableImports {
> > 
> > 		@Override
> > 		public void registerBeanDefinitions(AnnotationMetadata metadata, BeanDefinitionRegistry registry) {
> > 			register(registry, new PackageImports(metadata).getPackageNames().toArray(new String[0]));
> > 		}
> > 
> > 		@Override
> > 		public Set<Object> determineImports(AnnotationMetadata metadata) {
> > 			return Collections.singleton(new PackageImports(metadata));
> > 		}
> > 
> > 	}
> > ```



### @Import(AutoConfigurationImportSelector.class)

> ```java
> StringUtils.toStringArray(autoConfigurationEntry.getConfigurations())//给容器中导入组件
> ```
>
> 调用
>
> ```java
> List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes); //获取所有需要导入的容器组件
> ```
>
> 加载这个方法
>
> ```java
> Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader) //获取所有的组件
> ```
>
> 这个方法是默认从"META-INF/spring.factories"来加载一个文件
>
> 这个文件在spring-boot-autoconfigure-2.3.3.RELEASE.jar包下
>
> ![image-20201222163755220](Images\image-20201222163755220.png)
>
> 在这个文件里面有一个属性，开启自动配置，一旦spring-boot开启起来，就会加载进去127个组件
>
> ```xml
> # Auto Configure
> org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
> ...
> ```
>
> 但是springboot是不会默认开启所有自动配置的，是会按需配置。

# 数据访问

## 1、SQL

### 1、数据源的自动配置

#### 1、pom

导入starter

```xml
<!--jdbc-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jdbc</artifactId>
        </dependency>
```

![image-20201222165413653](Images\image-20201222165413653.png)

> 缺少驱动：
> 		因为官方不知道我们要用的数据库是什么，所以需要我们自己导入数据库。不写版本的话官方是会自动版本仲裁的

```xml
<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.41</version>
</dependency>
```

#### 2、配置文件

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver
```

#### 3、启动

### 2、使用Druid数据源

#### 1、pom

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.10</version>
</dependency>

```

#### 2、配置类

> 配置我们自己的数据源，并且把配置好的数据源里面的属性和配置文件中的属性绑定。

```java
@Configuration
public class MyDataSourceConfig {

    @ConfigurationProperties(prefix = "spring.datasource") //和配置文件的数据绑定
    @Bean
    public DataSource myDataSource(){
        return new DruidDataSource();
    }
}
```

> 成功配置数据源
>
> ![image-20201222171203148](Images\image-20201222171203148.png)

> 配置druid的监控页功能
>
> ```java
> @Configuration
> public class MyDataSourceConfig {
> 
>     @ConfigurationProperties(prefix = "spring.datasource") //和配置文件的数据绑定
>     @Bean
>     public DataSource myDataSource(){
>         return new DruidDataSource();
>     }
> 
>     /**
>      * 配置druid监控页功能
>      */
>     public ServletRegistrationBean statViewServlet(){
>         StatViewServlet statViewServlet = new StatViewServlet();
>         ServletRegistrationBean<StatViewServlet> registrationBean = new ServletRegistrationBean<StatViewServlet>(statViewServlet,"/druid/*");
>         return registrationBean;
>     }
> 
> }
> 
> ```

### starter方式

#### 1、引入starter

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/druid-spring-boot-starter -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.22</version>
</dependency>
```

#### 2、修改配置文件

```yaml
server:
  port: 8081

person:
  name: '里斯'
  age: '22'

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/test?useSSL=false
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver
    #开启监控
    druid:
      stat-view-servlet:
        enabled: true
        login-password: admin
        login-username: admin
        reset-enable: false
      web-stat-filter:
        enabled: true
```

开启了的监控

![image-20201222173310205](Images\image-20201222173310205.png)



### 3、整合mybatis

#### 注解方式

#### 1、pom

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.2</version>
</dependency>

```

#### 2、配置文件

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/test?useSSL=false
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver
```

#### 3、mapper、service、controller

```java
@Mapper
public interface UserMapper {

    @Select("select * from user where user_id = #{user_id}")
    User queryUserById(Integer user_id);
}
```

```java
public interface UserService {
    User queryUserById(Integer user_id);
}
```

```java
@Service
public class UserServiceImpl implements UserService {

    @Resource
    UserMapper userMapper;

    @Override
    public User queryUserById(Integer user_id) {
        return userMapper.queryUserById(user_id);
    }
}
```

```java
@ResponseBody
@Controller
public class UserController {

    @Resource
    UserService userService;

    @GetMapping(value = "/queryUserById/{user_id}")
    public User queryUserById(@PathVariable("user_id") Integer user_id){
        return userService.queryUserById(user_id);
    }
}
```

```bash
http://localhost:8081/queryUserById/2
```

![image-20201222174950456](Images\image-20201222174950456.png)

### 4、整合mybatis-plus

## 2、NOSQL

### 1、redis自动配置

> - RedisAutoConfiguration自动配置类。RedisProperties属性类
> - 连接工厂是准备好了的，LettuceConnectionConfiguration、JedisConnectionConfiguration
> - 自动注入了RedisTemplate<Object,Object>: xxxTemplate
> - 自动注入了StringTemplate<String,String>；k和v都是String
> - 底层我们只需要使用StringRedisTemplate和RedisTemplate就可以操作redis

### 2、RedisTemplate与Lettuce

#### 1、导入starter

```xml
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-data-redis -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
    <version>2.3.1.RELEASE</version>
</dependency>
```

#### 2、打开虚拟机的redis

#### 3、测试连接

![image-20201223091730692](Images\image-20201223091730692.png)

#### 4、Java连接redis

##### 1、配置文件

```yaml
  #redis
  #Connection URL. Overrides host, port, and password. User is ignored. Example:
  #redis://user:password@example.com:6379
spring:
  redis:
    url: redis://auth:123456@192.168.162.128:6379
```

##### 2、使用

```java
@Autowired
RedisTemplate redisTemplate;
@org.junit.jupiter.api.Test
    public void testRedis(){
        ValueOperations valueOperations = redisTemplate.opsForValue();
        valueOperations.set("k1","v1");
        System.out.println(valueOperations.get("k1"));
    }
```

##### 3、运行

![image-20201223092949965](Images\image-20201223092949965.png)





### 3、切换至jedis

#### 1、导入pom文件

```xml
<!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.3.0</version>
</dependency>

```

2、声明配置文件

```yaml
spring:
  redis:
  	client-type: jedis
```

# 7、单元测试

## 1、升级到了junit5

整合springboot之后，

- 编写测试类的方法： @Test
- Junit5具有spring的功能，@Autowired，@Transactional 标注的测试类方法，测试完成后自动回滚。

## 2、JUnit5常用注解

Junit5和Junit4有注解有些变化

> - @Test：表示这个方式是测试类，但是与junit4的@Test有很大的不同，他的职责非常单一不能声明任何的属性，扩展的测试将由Jupiter提供额外的测试
> - ParameterizedTest：表示方法是参数化测试，下方会有详细介绍
> - @RepeatedTest：表示方法可以重复执行，下方会有详细的介绍
> - @DispalyName：为测试类或者测试方法设置展示名称
> - @BeforeEach：表示在每个单元测试之前执行
> - @AfterEach：表示在每个测试方法之后执行
> - @AfterAll：表示在所有测试方法之后执行
> - @Tag：表示单元测试类别，类似于JUnit4中的@Categories
> - @Disabled：表示测试类或测试方法不执行，类似于Junit4中的@Ignore
> - @Timeout：表示测试方法运行如果超过了指定的时间将会返回错误
> - @ExtendWith：为测试类或测试方法提供扩展类引用

### @DispalyName

```java
@DisplayName(value = "测试DisplayName")
@Test
void test(){
    System.out.println(1);
}
```

![image-20201223095420027](Images\image-20201223095420027.png)

### @BeforeEach

```java
@BeforeEach
void beforeEach(){
    System.out.println("测试就要开始了");
}
```

![image-20201223095550590](Images\image-20201223095550590.png)

### @AfterEach

```java
@AfterEach
void afterEach(){
    System.out.println("测试结束了");
}
```

![image-20201223095744422](Images\image-20201223095744422.png)

### @BeforeAll和@AfterAll

要加静态方法，因为只调用一次

```java
@AfterAll
    static void afterAll(){
        System.out.println("所有的测试结束了.....");
    }

    @BeforeAll
    static void beforeAll(){
        System.out.println("所有的测试就要开始了.....");
    }
```

![image-20201223100116590](Images\image-20201223100116590.png)

### @Timeout

```java
/**
     * 在这里定义好超时的时间，如果超过时间就抛出异常
     * @throws InterruptedException
     */
    @Test
    @Timeout(value = 500,unit = TimeUnit.MILLISECONDS)
    void timeout() throws InterruptedException {
        Thread.sleep(600);
    }
```

![image-20201223100539023](Images\image-20201223100539023.png)

### @ExtendWith

### @RepeatedTest

```java
//会重复五次测试
@RepeatedTest(value = 5)
void test01(){
    System.out.println("hello");
}
```

![image-20201223101551327](Images/image-20201223101551327.png)

## 3、断言机制

> 断言是测试方法中的核心部分，用来对测试需要满足的条件进行验证。这些断言方法都是org.junit.jupiter.api.Assertions的静态方法。Junit5内置的断言可以分为以下几个类别：

### 1、简单的断言

![image-20201223102311764](Images/image-20201223102311764.png)

前面的断言失败，后面的断言不会执行。

### 2、数组断言

```java
assertArrayEquals(new int[2,2],new int [2,2]);

```



### 3、组合断言

```java
assertAll("tset",()->assertTrue(true && true),()->assertEquals(1,1));
```



### 4、异常断言

```java
@DisplayName(value = "异常断言")
@Test
void testException(){
    //判断这里一定会出现异常
    assertThrows(ArithmeticException.class,()->{
        int i = 10 / 2;
    },"居然没有出现异常");
}
```

![image-20201223103342709](Images/image-20201223103342709.png)



### 5、超时断言



### 6、快速失败

```java
@Test
    void testQuick(){
        if(1 == 1){
            fail("快速失败");
        }
    }
```

![image-20201223103516900](Images/image-20201223103516900.png)

## 4、前置条件

> Junit5中的前置条件，类似于断言。不同于断言的地方就是不满足的断言会使得测试方法失败，而不满足的前置条件只会使得测试方法的执行终止。前置条件可以看成是测试方法执行的前提，当该前提不满足时，就没有继续执行的必要了。

```java
@DisplayName(value = "前置条件测试")
@Test
void testAssumptions(){
    Assumptions.assumeTrue(false,"结果不是true");
    System.out.println(1111);
}
```

![image-20201223104240336](Images/image-20201223104240336.png)