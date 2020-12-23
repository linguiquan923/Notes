# SSM框架整合

## 一、Mybatis层

#### 1、建立数据库

#### 2、在pom.xml导入依赖

```xml
  <!--依赖：junit，数据库驱动，连接池，servlet，jsp，mybatis,mybatis-spring,spring-->

    <dependencies>
        <!--Junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <!--数据库驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.11</version>
        </dependency>
        <!--数据库连接池-->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.2</version>
        </dependency>

        <!--Servlet JSP-->
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

        <!--mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.5</version>
        </dependency>

        <!--spring-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>

    </dependencies>

    <!--静态资源导出问题-->

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

#### 3、防止静态资源导出问题，在pom.xml文件里配置

```xml
 <!--静态资源导出问题-->

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

#### 4、至此，maven项目的配置基本成型

#### 5、建立相对应的pojo、dao、service等包

#### 6、在resources下建立mybatis-config.xml文件（mybatis核心文件），applicationContext.xml文件（spring核心文件）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

</configuration>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--spring核心文件-->
</beans>
```



#### 7、首先要链接数据库，所以我们先在resources下建立db.properties

```pro
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/ssmbuild?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8
user=root
password=123456
```

#### 8、编写mybatis-config.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--配置数据源，交给spring去做-->
    <!--配置自动命名包，比如com.atlgq.pojo.User可以直接用User使用-->
    <typeAliases>
        <package name="com.atlgq.pojo"/>
    </typeAliases>
</configuration>
```

#### 9、创建实体类Books，对应的接口BookMapper和Mapper映射文件BookMapper.xml

BookMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.atlgq.dao.BookMapper">
    <insert id="addBook" parameterType="Books">
        insert into ssmbuild.books (bookName, bookCounts, detail)
        values (#{bookName},#{bookCounts},#{detail});
    </insert>

    <delete id="deleteBook" >
        delete from ssmbuild.books where bookID = #{bookID}
    </delete>

    <update id="updateBook" parameterType="Books">
        update books set
        bookName = #{bookName},bookCounts = #{bookCounts},detai =#{detail}
        where bookID = #{bookID};
    </update>

    <select id="queryBookByID" resultType="Books">
        select * from books where bookID = #{bookID}
    </select>

    <select id="queryAllBooks" resultType="Books">
        select * from ssmbuild.books;
    </select>
</mapper>
```

#### 10、把BookMapper.xml绑定到mybatis-config.xml文件中

```xml
   <mappers>
        <mapper class="com.atlgq.dao.BookMapper"/>
    </mappers>
```

#### 11、编写service层面BookService

```java
package com.atlgq.service;

import com.atlgq.pojo.Books;

import java.util.List;

public interface BookService {
    //增加一本书
    int addBook(Books books);
    //删除一本书
    int deleteBook(int id);
    //修改一本书
    int updateBook(Books books);
    //查询一本书
    Books queryBookByID(int id);
    //查询全部的书
    List<Books> queryAllBooks();
}
```

#### 12、编写接口BookService的实现方法

```java
package com.atlgq.service;

import com.atlgq.dao.BookMapper;
import com.atlgq.pojo.Books;

import java.util.List;

public class BookServiceImpl implements BookService {
    //service调用dao层；组合Dao
    private BookMapper bookMapper;

    public void setBookMapper(BookMapper bookMapper) {
        this.bookMapper = bookMapper;
    }

    public int addBook(Books books) {
        return bookMapper.addBook(books);
    }

    public int deleteBook(int id) {
        return bookMapper.deleteBook(id);
    }

    public int updateBook(Books books) {
        return bookMapper.updateBook(books);
    }

    public Books queryBookByID(int id) {
        return bookMapper.queryBookByID(id);
    }

    public List<Books> queryAllBooks() {
        return bookMapper.queryAllBooks();
    }
}

```

## 二、spring层

#### 1、在resources编写spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <!--1、关联数据库配置文件-->
    <context:property-placeholder location="classpath:db.properties"/>

    <!--2、连接池-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${driver}"/>
        <property name="jdbcUrl" value="${url}"/>
        <property name="user" value="${user}"/>
        <property name="password" value="${password}"/>

        <!--c3p0连接池私有属性-->
        <!--最大最小连接数量-->
        <property name="maxPoolSize" value="30"/>
        <property name="minPoolSize" value="10"/>
        <!--关闭后不自动commit-->
        <property name="autoCommitOnClose" value="false"/>
        <!--获取连接超时时间-->
        <property name="checkoutTimeout" value="10000"/>
        <!--可以尝试的重连次数-->
        <property name="acquireRetryAttempts" value="2"/>
    </bean>

    <!--3、sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--绑定mybatis的核心文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
    </bean>

    <!--4、spring提供配置dao接口扫描包，动态的实现了Dao接口可以注入到容器中-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--注入sqlSessionFactory-->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <!--扫描Dao包-->
        <property name="basePackage" value="com.atlgq.dao"/>
    </bean>
</beans>
```

#### 2、在resources编写spring-service.xml层

```xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <!--1、扫描service下的包-->
    <context:component-scan base-package="com.atlgq.service"/>
    
    <!--2、将所有的业务类注入到spring，这边我采用了注释的方式去注入容器-->
<!--    <bean id = "BookServiceImpl" class="com.atlgq.service.BookServiceImpl">-->
<!--        <property name="bookMapper" ref="bookMapper"/>-->
<!--    </bean> -->
    
    <!--3、声明式事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--注入数据源-->
        <property name="dataSource" ref="dataSource"/>
    </bean>
    
    <!--4、aop事务支持-->
</beans>
```

#### 3、要在applicationContext.xml文件中把配置文件关联起来

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--spring核心文件-->
    <!--把spring的配置文件整合起来-->
    <import resource="classpath:spring-service.xml"/>
    <import resource="classpath:spring-dao.xml"/>
</beans>
```

## 三、springMVC层

#### 1、右键加入web框架，并且配置web.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--1、配置DispatcherServlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <!--这边需要导入applicationContext.xml，将所有的bean都注入-->
            <param-value>classpath:applicationContext.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!--2、乱码过滤-->
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

    <!--Session-->
    <session-config>
        <session-timeout>15</session-timeout>
    </session-config>
</web-app>
```

#### 2、在resources下创建spring-mvc.xml文件，并且在applicationContext.xml文件中把他加入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd">
    <!--spring核心文件-->
    <!--把spring的配置文件整合起来-->
    <import resource="classpath:spring-service.xml"/>
    <import resource="classpath:spring-dao.xml"/>
    <import resource="classpath:spring-mvc.xml"/>
</beans>
```

#### 3、编写spring-mvc.xml文件并且编写contrller包

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!--1、注解驱动,利用annotation-driven生成HandleMapping和HandleAdapter,解决json乱码问题
       -->
    <mvc:annotation-driven/>
    <!--2、静态资源过滤-->
    <mvc:default-servlet-handler/>
    <!--3、扫描包：controller-->
    <context:component-scan base-package="com.atlgq.controller"/>

    <!--4、视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

## 至此，ssm框架整合结束。可以写页面开始运行

### 代码出错

由于没有在这里导入lib包，导致出错。

**导入jar包的时候不能导入servlet-api:2.5否则会报错**

![image-20201028194138050](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201028194138050.png)





## 测试

#### 1、编写BookControlelr

```java
package com.atlgq.controller;

import com.atlgq.pojo.Books;
import com.atlgq.service.BookService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import java.util.List;

@Controller
@RequestMapping("/book")
public class BookController {
    //controller调用service层
    @Autowired
    private BookService bookService;

    @RequestMapping("/allBook")
    public String queryAllBooks(Model model){
        List<Books> books = bookService.queryAllBooks();
        model.addAttribute("list",books);
        return "allBook";
    }
}

```

#### 2、Junit调用单元测试(控制台能够输出)

```java
import com.atlgq.pojo.Books;
import com.atlgq.service.BookService;
import com.atlgq.service.BookServiceImpl;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.util.List;

public class MyTest {
    @Test
    public void test(){
       ApplicationContext con = new ClassPathXmlApplicationContext("applicationContext.xml");
        BookService bean = (BookService) con.getBean(BookServiceImpl.class);
        List<Books> books = bean.queryAllBooks();
        for (Books book : books) {
            System.out.println(book);
        }

    }
}

```

#### 3、在/WEB-INF/jsp/下编写相对应的jsp文件

![image-20201028221527406](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201028221527406.png)



#### 4、修改index.jsp，点击访问allBook.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  $END$
  <h1><a href="${pageContext.request.contextPath}/book/allBook}">访问所有书籍</a> </h1>
  </body>
</html>

```

# AOP事务（完成事务）

#### 1、在spring-service.xml文件下添加

```xml
xmlns:aop="http://www.springframework.org/schema/aop"
http://www.springframework.org/schema/aop
https://www.springframework.org/schema/aop/spring-aop.xsd">
 <!--4、aop事务支持-->
    <aop:config>

    </aop:config>
```

#### 2、在pom.xml导入aop的包

```xml
<!--aop-->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.13</version>
        </dependency>
```

#### 3、在{}把上面导入的包添加进去

![image-20201029100806337](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201029100806337.png)

#### 4、在spring-service.xml添加

```xml
xmlns:tx="http://www.springframework.org/schema/tx"
http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd

 	<!--4、aop事务支持-->
    <!--结合AOP实现事务的注入-->
    <!--配置事务的通知-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
    <!--配合事务的切入-->
    <aop:config>
        <aop:pointcut id="txPointCut" expression="execution(* com.atlgq.dao.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
    </aop:config>
```

