# springBoot web开发

### 1、导入静态资源

#### 	webjars：

- 百度webjars，在官网获取maven下载，到项目内导入

  ```
  <dependency>
      <groupId>org.webjars</groupId>
      <artifactId>jquery</artifactId>
      <version>3.5.1</version>
  </dependency>
  ```

- ```java
  http://localhost:8080/webjars/jquery/3.5.1/jquery.js访问静态资源
  ```

  在

  ​		classpath:/resources

  ​		classpath:/public  放公共资源

  ​		classpath:/static	静态资源，如图片等
  
  ​		下的所有文件都能被springboot访问到，优先级：resources > static > public



### 2、首页

```
在templates下的所有文件只能通过controller访问
//这需要thymeleaf模板引擎的支持
```



### 3、Thymeleaf模板引擎

使用thymeleaf模板引擎

#### 1、在pom.xml加入依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.17.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.atlgq</groupId>
    <artifactId>springboot-03-web</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springboot-03-web</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <!--thymeleaf模板引擎，我们使用的都是3.x版本的-->
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring5</artifactId>
        </dependency>
        <dependency>
            <groupId>org.thymeleaf.extras</groupId>
            <artifactId>thymeleaf-extras-java8time</artifactId>
        </dependency>

        
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
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>RELEASE</version>
            <scope>test</scope>
        </dependency>
        <!--jquery-->
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>3.5.1</version>
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

</project>

```

#### 2、访问templates下的html

在resources下新建一个templates的包，然后在里面写html文件，通过control层的页面调用跳转到templates里面的网页

IndexController.java

```java
//在templates下的所有文件只能通过controller访问
//这需要thymeleaf模板引擎的支持
@Controller
public class IndexController {
    @RequestMapping("/test")
    public String index(){
        return "test";
    }
}
```

访问成功

![image-20201208142139211](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201208142139211.png)

#### 3、取值问题

##### 1、引入命名规范

```java
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```

##### 2、controller页面传值

```java
public String index(Model model){
        model.addAttribute("msg","hello spring boot");
        return "test";
    }
```

##### 3、html页面接收值

```html
<-- html的所有元素都能被thymeleaf接管，替换 -->
<div th:text="${msg}"></div>
```

#### 4、thymeleaf的基础语法



##### 1、th:utext 转义

controller

```java
model.addAttribute("msg","<h1>hello spring boot</h1>");
```

html

```html
<div th:text="${msg}"></div>
<div th:utext="${msg}"></div>
```

![image-20201208143205826](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201208143205826.png)

##### 2、th:each循环

html

```html
<div th:each="item:${users}" th:text="${item}"></div>
//行内写法
<div th:each="item:${users}" >[[${item}]]</div>
```





### 4、装配扩展SpringMVC

#### 1、MVC配置原理

我们自己建立mvc配置文件，新建config包

```java
/**
 * 我们要在这里配置我们自己定义的Mvc视图解析器
 * 如果我们想实现一些功能的话，我们只需要写这个组件，然后把它交给springboot就可以了，springboot会帮我们自动装配
 */
//第一步：设置为配置文件
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    
    //自定义视图跳转，访问lgq的时候跳转到test.html页面
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/lgq").setViewName("test");
    }

    //ViewResolver实现了视图解析器的接口的类，我们就可以把它当作一个视图解析器
    @Bean
    public MyViewResolver myViewResolver(){
        return new MyViewResolver();
    }

    //自定义一个自己的视图解析器
    public static class MyViewResolver implements ViewResolver{
        @Override
        public View resolveViewName(String s, Locale locale) throws Exception {
            return null;
        }
    }
}

```

在这里打上断点

![image-20201208153108330](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201208153108330.png)

使用debug模式可以看到我们配置的文件是有被springboot装配的
![image-20201208153154175](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201208153154175.png)



#### 2、扩展SpringMvc

MyMvcConfig.java

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    
    //自定义视图跳转，访问lgq的时候跳转到test.html页面
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/lgq").setViewName("test");
    }
}

```

在springboot里面，有非常多的xxxConfiguration帮助我们进行扩展配置，只要看见了这个东西，我们就要注意了。

### 到现在所有的准备都做好了



### 5、增删改查

#### 1、写java对象

deparments,employees

#### 2、写dao层，里面包含假数据

departmentDao.java

```java
package com.atlgq.dao;

import com.atlgq.pojo.Department;
import org.springframework.stereotype.Repository;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;

@Repository
public class DepartmentDao {

    private static Map<Integer, Department> departments = null;

    static {
        departments = new HashMap<Integer, Department>(); //创建一个部门表
        departments.put(101,new Department(101,"教学部"));
        departments.put(102,new Department(102,"市场部"));
        departments.put(103,new Department(103,"运营部"));
        departments.put(104,new Department(104,"教研部"));
        departments.put(105,new Department(105,"后勤部"));
    }


    //获得所有部门信息
    public Collection<Department> getDepartment(){
        return departments.values();
    }

    //通过id得到部门
    public Department getDepartmentById(Integer id){
        return departments.get(id);
    }
}

```

employeeDao.java

```java
package com.atlgq.dao;

import com.atlgq.pojo.Department;
import com.atlgq.pojo.Employee;
import org.springframework.stereotype.Repository;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;

@Repository
public class EmployeeDao {
    //模拟数据
    private static Map<Integer, Employee> employees = null;

    //需要获取到部门信息，引入dao，需要把两个对象都加入到springboot中
    private DepartmentDao departmentDao;

    static{
        employees = new HashMap<Integer, Employee>();
        employees.put(1001,new Employee(1001,"AA","AA@qq.com",1,new Department(101,"教学部")));
        employees.put(1002,new Employee(1002,"BB","BB@qq.com",0,new Department(102,"市场部")));
        employees.put(1003,new Employee(1003,"CC","CC@qq.com",1,new Department(103,"运营部")));
        employees.put(1004,new Employee(1004,"DD","DD@qq.com",0,new Department(104,"教研部")));
        employees.put(1005,new Employee(1005,"EE","EE@qq.com",1,new Department(105,"后勤部")));
    }

    //主键自增
    private static Integer initId = 1006;
    //增加一个员工
    public void save(Employee employee){
        //判断是否有这个员工，如果没有的话自增长一个
        if(employee.getId() == null){
            //这里自增长的是类似于逐渐的数值，而不是员工自己的id
            employee.setId(initId++);
        }
        //????
        //通过员工的部门id获取到部门信息，并且添加到，？员工本身不是携带这部门信息？？？
        employee.setDepartment(departmentDao.getDepartmentById(employee.getDepartment().getId()));


        //添加进去，增加主键和员工信息
        employees.put(employee.getId(),employee);

    }

    //查询全部的员工
    public Collection<Employee> getAll(){
        return employees.values();
    }

    //通过id获取员工
    public Employee getEmployeeById(Integer id){
        return employees.get(id);
    }

    //删除员工
    public void deleteById(Integer id){
        employees.remove(id);
    }
}

```





### 6、拦截器

### 7、国际化

