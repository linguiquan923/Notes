# 学习springboot

## 6、使用spring Initializer快速创建Spring Boot项目

IDE都支持使用Spring的项目创建想到快速创建一个Spring Boot项目；

选择我们需要的模块：需要联网

默认生成的Spring Boot项目：

- 主程序已经生成好了，我们只需要有自己的逻辑

- resource文件夹中的目录

  - static：保存所有静态资源：js css images
  - templates:保存所有模块页面，
  - application.properties：SpringBoot应用的配置文件，如：改端口号为8081访问

  ![image-20201021105844877](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20201021105844877.png)



## 二、配置文件

#### 1、配置文件

​	Spring boot使用一个全局的配置文件，配置文件名是固定的

- application.properties
- application.yml

配置文件的作用：修改spring boot自动配置的默认值；Spring Boot在底层都给我们配置好了



YAML（YAML Aint't Markup Language）

​	YAML A Markup Language:是一个标记语言

​	YAML isnot Markup Language:不是一个标记语言

标记语言：

​	以前的配置文件：大多使用xxx.xml文件

​	YAML：以数据为中心，比json、xml更适合做配置文件

​	YAML:配置例子

​	server:

​		port:8081

#### 2、yaml语法

k:(空格)v :表示一队键值对（空格必须有）

#### 以空格的缩进来控制层级关系；只要是左对齐的一列数据，都是同一个层级的

```yaml
server:
    port:8081
    path:/hello
```

属性和值也是大小写敏感的；



#### 2、值的写法

字面量：普通的值

k: v : 字面直接来写；

​	字符串默认不用加上单引号和双引号；

​	"":双引号：不会转义字符里面的特殊字符

​					name: "zhangsan \n lisi "  输出：zhangsan 换行 lisi

​	''：单引号：会转义特殊字符

​					name: 'zhangsan \n lisi '  输出：zhangsan \n lisi



对象、Map（属性和值）（键值对）

​	k: v ：在下一行来写对象的属性和值；注意缩进

​			对象还是k: v方式

```yaml
friiengds:
		lastName: zhangsan
		age: 20


```

行内写法：

```yaml
friiengds: {lastName: zhangsan,age: 20}			
```

数组（List、Set）

用-值标识数组中的一个元素

```yaml
pets:
	- cat
	- dog
	- pig
```

行内写法

```yam
pets: [cat,dog,pig]
```

#### 3、配置文件值注入

配置文件

```yaml
server:
  port: 8081

person:
  lastName: zhangsan
  age: 18
  birth: 2017/12/12
  maps: {k1: v1,k2: v2}
  lsit:
    - lisi
    - maliu
  dog:
    name: 小狗
    age: 2
```

javaBean:

```java
/**
 * 将配置文件中配置的每一个值映射到这个组件当中
 * @ConfigurationProperties:告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定
 *              prefix = "person"配置文件中哪个下面的所有属性进行一一映射
 * 只有这个组件是容器中的组件，才可以使用@ConfigurationProperties(prefix = "person")功能，用@Component注入容器
 */

@Component
@ConfigurationProperties(prefix = "person")
public class Person {

    private String lastName;
    private int age;
    private boolean boss;
    private Date birth;

    private Map<String,Object> maps;
    private List<String> list;
    private Dog dog;
```

我们可以导入配置文件处理器，以后编写配置就有提示

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
    		<optional>true</optional>
</dependency>
```

#### 2、@Value和@ConfigurationProperties获取值区别

|                      | @ConfigurationProperties | @Value     |
| -------------------- | ------------------------ | ---------- |
| 功能                 | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定（松散语法） | 支持                     | 不支持     |
| SpEL                 | 不支持                   | 支持       |
| JSR303               | 支持                     | 不支持     |
| 复杂类型封装         | 支持                     | 不支持     |

配置文件yml还是properties他们都能获取到值

如果说，我们只是在某个业务逻辑中需要获取一下配置文件的某项值，使用@Value

如果说，我们专门编写了一个javaBean来和配置文件进行映射，我们直接使用@ConfigurationProperties

#### 3、配置文件注入值数据校验

```java
@Component
@ConfigurationProperties(prefix = "person")
/**
 * @Validated数据校验模块
 */
//@Validated
public class Person {

    /***
     * <bean class="Person">
     *  <property name="lastName" value="字面量/${从环境变量、配置文件中取值}/#{SpEL}"></property>
     * </bean>
     */
//    @Value("${person.last-name}"
//    lastName必须为email格式，否则报错
//     @Email
    private String lastName;
//    @Value("#{11*2}")
    private int age;
//    @Value("true")
    private boolean boss;
    private Date birth;

```

#### 4、@PropertySource&@ImportResource

@PropertySource:加载指定的配置文件

```java
/**
 * 将配置文件中配置的每一个值映射到这个组件当中
 * @ConfigurationProperties:告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定
 *              prefix = "person"配置文件中哪个下面的所有属性进行一一映射
 * 只有这个组件是容器中的组件，才可以使用@ConfigurationProperties(prefix = "person")功能，用@Component注入容器
 * 默认从全局配置文件中获取，使用@PropertySource(value = "classpath:person.properties")可以指定文件
 */
@PropertySource(value = "classpath:person.properties")
@Component
@ConfigurationProperties(prefix = "person")
/**
 * @Validated数据校验模块
 */
//@Validated
public class Person {

    /***
     * <bean class="Person">
     *  <property name="lastName" value="字面量/${从环境变量、配置文件中取值}/#{SpEL}"></property>
     * </bean>
     */
//    @Value("${person.last-name}"
//    lastName必须为email格式，否则报错
//     @Email
    private String lastName;
//    @Value("#{11*2}")
    private int age;
//    @Value("true")
    private boolean boss;
    private Date birth;

    private Map<String,Object> maps;
    private List<String> list;
    private Dog dog;
```

@ImportResource：导入Spring的配置文件，让配置文件里面的内容生效；

SpringBoot里面没有Spring的配置文件，我们自己编写的配置文件，也不能自动识别

想让Spring的配置文件生效，加载进来；@ImportResource标注在一个配置类上

```
@ImportResource(locations = {"classpath:beans.xml"})
导入配置文件让其生效
```

不来编写spring的配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="helloService" class="com.atlgq.springboot02config.service.HelloService"></bean>
</beans
```

Spring Boot 推荐给容器中添加组件的方式；推荐使用全注解的方式

1. 配置类=====Spring配置文件

2. 使用@Bean给容器添加组件

   ```java
   package com.atlgq.springboot02config.config;
   
   import com.atlgq.springboot02config.service.HelloService;
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   
   //告诉Springboot这是一个配置类；就是用来代替之前的Spring配置文件
   @Configuration
   public class MyConfig {
       @Bean
       public HelloService helloService(){
           System.out.println("配置类成功");
           return new HelloService();
       }
   }
   ```

   

### 4、配置文件占位符

#### 1、随机数



2、占位符获取之前配置的值，如果没有可以使用：指定默认值

```pr
#idea配置文件默认utf-8编码
#配置person文件
person.last-name=张三${random.uuid}
person.age=${random.int}
person.birth=2017/12/12
person.boss=true
person.maps.k1=v1
person.maps.k2=v2
person.list=a,b,c
person.dog.name=${person.hello:hello}_小狗
person.dog.age=2
```

## 5、Profile

#### 1、多profile文件

我们在配置文件编写的时候，文件名可以是application-{profile}.profile/yml

默认使用application.properties



#### 2、yml支持多文档块方式

```yam
server:
  port: 8081
spring:
  profiles:
    active: dev
---
server:
  port: 8083
spring:
  profiles: dev
---
server:
  port: 8084
spring:
  profiles: prod #指定属于哪个环境

```

#### 3、激活指定profile

1、在配置文件中指定：spring.profiles.active=dev

2、命令行：

​		--spring.profiles.active=dev

3、虚拟机参数



## 6、配置文件加载位置

file:/config/

file:/

classpath:/config/

classpath:/

优先级从高到低，高优先级覆盖低优先级

四个加载文件会互补，互补配置

## 7、外部文件加载顺序



## 8、自动配置原理

配置文件到底能写什么？怎么写？自动配置原理



##### 自动配置原理

1）、SpringBoot启动的时候加载主配置类，开启了自动配置功能@EnaAutoConfiguration

2）、@EnaAutoConfiguration的作用：



# 三、日志

## 1、日志框架

​	日记门面：SLF4j

​	日记实现：Logback

​			







​	



