# springMVC

##### 1、创建父maven，删除src，在pom.xml导入依赖

```xml

    <!--导入依赖-->
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
    </dependencies>
```



##### 2、新建子项目，右键Add Frameworks Support，选择WebApplication，形成一个web项目

##### 3、在pom.xml导入servlet和jsp依赖

```xml
 <!--导入依赖-->
    <dependencies>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>
    </dependencies>
```

##### 至此，servlet项目导包完成，可以实现开发。



### 项目开始：

1、创建类，继承HttpServlet成为一个servlet

2、写好servlet之后要在web.xml注册上

```xml
 <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>com.atlgq.servlet.HelloServlet</servlet-class>
    </servlet>
    <!--请求hello1会访问到HelloServlet页面-->
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello1</url-pattern>
    </servlet-mapping>
```

3、点击运行配置tomocat

```xml
http://localhost:8080/springMVC_01_servlet_war_exploded/hello?method=add
```



# 开始学习springMVC

#### 1、在web.xml注册dispatcherServletr

```xml
  <!-- 注册DispatcherServlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- spring MVC的配置文件 -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!-- 启动级别，下面值小一点比较合适，会优先加载 -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    
    <!--映射：访问能够映射到springmvc中-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

<!--乱码解决-->
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

```

#### 2、编写在resources下编写springmvc-servlet.xml文件

```xml
<!--自动扫描包-->
    <context:component-scan base-package="com.atlgq.controller"/>
   <mvc:annotation-driven/>
    <!--视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>
```

#### 3、编写HelloController类

```java
public class HelloController implements Controller {
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        //ModelAndView模型和视图
        ModelAndView mv = new ModelAndView();

        //封装对象，放在ModeAndView中
        mv.addObject("msg","HelloSpringMVC");
        //封装要跳转的视图，放在ModelAndView中
        mv.setViewName("hello");
        return mv;
    }
}

```

#### 4、编写一个hello.jsp文件，并且把HelloController注入容器(在springmvc-servlet.xml)

```xml
  <!--把controller类注入到容器中-->
    <bean id="/hello" class="com.atlgq.controller.HelloController"/>
```

#### 5、在访问的时候可能会出现404错误

- 查看控制台是否少了jar包

- 

- #### 如果jar包存在，那么添加lib依赖（点击项目结构，右上角，点击artifacts，在对应的WEB-INF包下添加一个lib包，然后把jar包添加进去
  
  - #### 在导入jar包的时候，全部导入会出现问题，直接出现500-的错误界面，这个时候是jar包冲突出现的问题，此时仅需要把对应的jar包从lib移除就可以，我采用分半方式排查，先导入上半部分jar包，出错，然后导入下半部分，直接出错
  
- 重启tomcat

#### 6、存在找不到配置文件的情况下，在pom.xml配置

```xml
  <!--寻找配置文件-->
    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
        </resources>
    </build>
```



### springMVC运行流程

1、用户访问，dispatcherServlet收到请求

```xml
<servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- spring MVC的配置文件 -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!-- 启动级别，下面值小一点比较合适，会优先加载 -->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <!--映射：访问能够映射到springmvc中-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```



2、dispatcherServlet寻找对应的HanddleMapping并且调用-》

```xml
 <!--映射器-->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
```



3、handleMapping根据请求寻找Handle，寻找控制器-》

4、生成处理器对象和拦截器（如果有）一起返回给dispatcherServlet-》

```java
返回一个hello，这是浏览器访问的
```



5、dispatcherServlet调用HandlerAdapter去寻找相对应的处理器-》

```xml
 <!--处理器适配-->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
```



6、寻找到controller，让controller执行，返回一个ModeAndView-》

7、HandlerAdapter把ModeAndView返回给DispatcherServlet-》

8、把ModeAndView传递给ViewReslover-》

```xml
 <!--视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>
```



9、ViewReslover返回一个View-》

10、DispatcherServlet根据view渲染视图-》

11、DispatcherServlet响应用户



## 使用注解的方式开发springmvc

1、配置web.xml文件（和上面一样）

```xml
  <!-- 注册DispatcherServlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- spring MVC的配置文件 -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!-- 启动级别，下面值小一点比较合适，会优先加载 -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    
    <!--映射：访问能够映射到springmvc中-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
 <!--spring自定义的过滤器，乱码过滤-->
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```



2、配置spring-servlet.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--配置自动扫描包，让controller下的所有注解生效-->
    <context:component-scan base-package="com.atlgq.controller"/>
    <!--springmvc会自动加载静态文件，所以在这里把静态文件屏蔽掉-->
    <mvc:default-servlet-handler/>
    <!--利用这个生成HandleMapping和HandleAdapter-->
    <mvc:annotation-driven/>

    <!--视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

3、写好jsp文件

4、写controller文件，使用注解的方式

```xml
package com.atlgq.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class ControllerTest2 {

    @RequestMapping("/test2")
    public String hello(Model model){
        model.addAttribute("msg","hello,test2");
        return "test";

    }
}

```

### RestFUl风格使用

​	原本的传参风格为，浏览器的头部会显示这些信息。

```java
 @RequestMapping("/add")
    public String test1(int a, int b, Model model){
        int res = a + b;
        model.addAttribute("msg","结果等于" + res);

        return "test";

    }
```

![image-20201028103315086](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201028103315086.png)

##### RestFul风格为:

http://localhost:8080/springmvc_04_hellocontroller_war_exploded/add/1/4

```java
//@PathVariable会把地址栏上面的{a},{b}映射到test1里面的a和b
@RequestMapping("/add/{a}/{b}")
    public String test1(@PathVariable int a,@PathVariable int b, Model model){
        int res = a + b;
        model.addAttribute("msg","结果等于" + res);

        return "test";

    }
```

![image-20201028103615740](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201028103615740.png)

可以实现通过限定不同的请求方式而实现代码的复用

```java
 @GetMapping("/add/{a}/{b}")
    public String test1(@PathVariable int a,@PathVariable int b, Model model){
        int res = a + b;
        model.addAttribute("msg","结果等于" + res);

        return "test";

    }

    @PostMapping("/add/{a}/{b}")
    public String test2(@PathVariable int a,@PathVariable int b, Model model){
        int res = a + b;
        model.addAttribute("msg","结果2等于" + res);

        return "test";

    }
```



### springMVC的转发和重定向（把视图解析器注释掉）

转发：地址栏不会变

```java
 @RequestMapping("m1/t1")
    public String test(Model model){

        model.addAttribute("msg","ModelTest1");

        //转发
        return "/WEB-INF/jsp/test.jsp";
    }
```

重定向:地址栏会变（可以配合视图解析器使用）

```java
 @RequestMapping("m1/t1")
    public String test(Model model){

        model.addAttribute("msg","ModelTest1");

        //转发
//        return "/WEB-INF/jsp/test.jsp";
        //重定向
        return "redirect:/index.jsp";
    }

```



#### 接收前端传递的值

```java
http://localhost:8080/springmvc_04_hellocontroller_war_exploded/user/t2?id=1&name=lgq&age=23
//前端接收的是一个对象
    @GetMapping("/t2")
    public String test2(User user){
        System.out.println(user);
        return "test";
    }
```



### 乱码问题

表单提交的时候可能就出现乱码，采用过滤器，在web.xml插入

```xml
 <!--spring自定义的过滤器，乱码过滤-->
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

### JSON

1、在pom.xml导入jar包

```xml

    <dependencies>
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.12.0-rc1</version>
        </dependency>

    </dependencies>
```

​	乱码解决:在springmvc-servlet.xml文件中添加

```java
 <!--
        利用annotation-driven生成HandleMapping和HandleAdapter
        解决json乱码问题
        -->
    <mvc:annotation-driven>
        <mvc:message-converters register-defaults="true">
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <constructor-arg value="UTF-8"/>
            </bean>
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
                <property name="objectMapper">
                    <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                        <property name="failOnEmptyBeans" value="false"/>
                    </bean>
                </property>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>
```

在controller层面

```java
ObjectMapper mapper = new ObjectMapper();
		//创建对象
        User user = new User("林贵全",23,"男");
		//把对象转换成strin
        String s = mapper.writeValueAsString(user);
```

### 学会使用代码复用和uitls

​	在uitls里面编写一个utils工具类

```java
public static String getJson(Object object,String dateFormat) {
        ObjectMapper mapper = new ObjectMapper();

        mapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS,false);
        SimpleDateFormat sdf = new SimpleDateFormat(dateFormat);
        mapper.setDateFormat(sdf);
        try {
            return mapper.writeValueAsString(object);
        } catch (JsonProcessingException e) {
            return null;
        }
    }
```

但是有时候只传入一个参数，此时可以采用方法重载

```java
  //实现了代码的重载，避免重复代码,直接默认了时间的格式
    public static String getJson(Object object){
        return getJson(object, "yyyy-mm-dd HH:mm:ss");
    }

```

配置完工具之后，Json的使用变成

```java
 @RequestMapping("/j1")
    @ResponseBody//就不会走视图解析器,会直接返回一个字符串
    public String json1() throws JsonProcessingException {

        User user = new User("林贵全",23,"男");

        return JsonUtil.getJson(user);
    }
```



