# Mybatis使用

#### 1、现在pom.xml文件导入依赖

```xml
 <!--导入依赖-->
    <dependencies>
        <!--加入junit依赖-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
        <!--加入mybatis依赖-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <!--加入mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.11</version>
        </dependency>
    </dependencies>

    <!--把自己配置的文件写入mybatis
        写入这个的话，那么就可以搜索到自己写的UserMapper.xml
    -->
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
        </resources>
    </build>
```

#### 2、在resources下配置db.properties和mybatis-config.xml(mybatis的核心文件)

```xml
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8
user=root
password=123456
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--引入外部配置文件-->
    <properties resource="db.properties"/>

    <settings>
        <!--标准的日志工厂实现-->
<!--        <setting name="logImpl" value="STDOUT_LOGGING"/>-->
        <!--
            使用log4j：
            第一步：导入依赖包

        -->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
        <!--是否开启驼峰命名映射-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>

    <!--给实体类起别名-->
    <typeAliases>
        <package name="com.atlgq.pojo"/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${user}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
<!--    <mappers>-->
<!--        <mapper resource="com/atlgq/dao/UserMapper.xml"/>-->
<!--    </mappers>-->

    <!--绑定接口-->
<!--    <mappers>-->
<!--        <mapper class="com.atlgq.dao.UserMapper"></mapper>-->
<!--    </mappers>-->
    <!--通过package把dao包下面的xml文件注入到核心文件中-->
   <mappers>
        <mapper resource="com/atlgq/dao/UserMapper.xml"/>
    </mappers>
</configuration>
```

#### 3、写相对应的uitls包，返回sqlSession

```java
package com.atlgq.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class MybatisUtils {

    private static SqlSessionFactory sqlSessionFactory;
    static {
        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream;
            inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static SqlSession getSqlsession(){
        return sqlSessionFactory.openSession(true);
    }



}

```

#### 4、写好实体类和dao接口

#### 5、写好mapper文件，每个写好的mapper文件都需要在核心文件中映射

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.atlgq.dao.BlogMapper">

</mapper>

```

