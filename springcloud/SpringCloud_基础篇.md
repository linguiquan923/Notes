# Springcloud 基础篇

![image-20201219153651599](Images\image-20201219153651599.png)



## 0,SpringCloud升级,部分组件停用:

1,Eureka停用,可以使用zk作为服务注册中心

2,服务调用,Ribbon准备停更,代替为LoadBalance

3,Feign改为OpenFeign

4,Hystrix停更,改为resilence4j

​		或者阿里巴巴的sentienl

5.Zuul改为gateway

6,服务配置Config改为  Nacos

7,服务总线Bus改为Nacos





# 环境搭建:



## 1,创建父工程,pom依赖

##### 	1、New Project	

##### 	2、聚合总父工程的名字

#####     3、Maven选版本

#####     4、工程名字

#####     5、字符编码



##### 6、注解生效激活



##### 7、java编译版本选8

##### 8、filetype文件过滤

##### 9、声明pom父工程，删掉src，在pom.xml文件里面添加 

#####  **<packaging>pom</packaging>**

这是示例添加后的

```xml
  <groupId>com.atlgq.springcloud</groupId>
  <artifactId>cloud2020</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>pom</packaging>
表示这个一个pom父工程
```

##### 10、在pom.xml添加一下内容

```xml
 <!--统一管理jar包版本-->
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>12</maven.compiler.source>
    <maven.compiler.target>12</maven.compiler.target>
    <junit.version>4.12</junit.version>
    <lombok.version>1.18.10</lombok.version>
    <log4j.version>1.2.17</log4j.version>
    <mysql.version>8.0.18</mysql.version>
    <druid.version>1.1.16</druid.version>
    <mybatis.spring.boot.version>2.1.1</mybatis.spring.boot.version>
  </properties>

  <!--子模块继承之后，提供作用：锁定版本+子moudle不用写groupId和version-->
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-project-info-reports-plugin</artifactId>
        <version>3.0.0</version>
      </dependency>
      <!--spring boot 2.2.2-->
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.2.2.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!--spring cloud Hoxton.SR1 ，springcloud使用H版的-->
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
        <version>Hoxton.SR1</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>

      <!--spring cloud 阿里巴巴-->
      <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-alibaba-dependencies</artifactId>
        <version>2.1.0.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!--mysql-->
      <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>${mysql.version}</version>
        <scope>runtime</scope>
      </dependency>
      <!-- druid-->
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>${druid.version}</version>
      </dependency>

      <!--mybatis-->
      <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>${mybatis.spring.boot.version}</version>
      </dependency>
      <!--junit-->
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>${junit.version}</version>
      </dependency>
      <!--log4j-->
      <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>${log4j.version}</version>
      </dependency>
    </dependencies>
  </dependencyManagement>
```

dependencyManagement这一般是父项目里面的，只是声明依赖，并不实现引入，子项目是不需要写版本号的，指定子项目的版本号，如果子项目需要其他的版本的时候，只需在<version></version>

## 2,创建子模块,pay8001模块

### 1,子模块名字:

```java
cloud-provider-payment8001
```

### 2,pom依赖

在pom.xml文件添加

```xml
 		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--监控-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>
        <!--mysql-connector-java-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!--jdbc-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <!--热部署-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
```



### 3,创建application.yml

```yml
server:
  port: 8081

spring:
  application:
    name: cloud-payment-service #服务名称
  datasource:
    # 当前数据源操作类型
    type: com.alibaba.druid.pool.DruidDataSource
    # mysql驱动类
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db2019?useUnicode=true&characterEncoding=UTF-8&useSSL=false&serverTimezone=UTC
    username: root
    password: 123456
    
mybatis:
  mapper-locations: classpath*:mapper/*.xml 配置扫描文件
  type-aliases-package: com.atlgq.springcloud.entities 给实体类起别名

```

### 4,主启动类    

```java
@SpringBootApplication
public class PaymentMain8081 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8081.class,args);
    }
}

```



### 5,业务类

#### 1,sql

```sql
 create table payment
 (id int NOT NULL primary key auto_increment comment 'ID',
  serial varchar(200)
 )ENGINE=InnoDB auto_increment=1 default charset=utf8;
```



#### 	2,实体类

Payment

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
/*Serializable是引入序列化*/
public class Payment implements Serializable
{
    private Long id;
    private String serial;
}

```



#### 3,.entity类

CommonResult是用来传递消息给前端的

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
/*前端通用消息*/
public class CommonResult<T> {
    private int code;
    private String message;
    private T data;

//    因为data可能为空，所以我们重载这个方法
    public CommonResult(int code,String message){
        this(code,message,null);
    }
}
```



#### 4,dao层:

#### 5,mapper配置文件类，记得修改namespace

​				**在resource下,创建mapper/PayMapper.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.atlgq.springcloud.alibaba.dao.PaymentDao">
    <!--useGeneratedKeys返回值-->
    <insert id="create" parameterType="Payment" useGeneratedKeys="true" keyProperty="id">
        insert into payment (serial) values (#{serial});
    </insert>
    <resultMap id="BaseResultMap" type="com.atlgq.springcloud.alibaba.entities.Payment">
        <id column="id" property="id" jdbcType="BIGINT"/>
        <id column="serial" property="serial" jdbcType="VARCHAR"/>
    </resultMap>
    <select id="getPaymentById" parameterType="Long" resultMap="BaseResultMap">
        select * from payment where id = #{id};
    </select>
</mapper>
```

#### 6,写service和serviceImpl

service

```java
public interface PaymentService {
    public int create(Payment payment);

    public Payment getPaymentById(Long id);
}

```

serviceImpl

```java
package com.atlgq.springcloud.service.impl;

import com.atlgq.springcloud.dao.PaymentDao;
import com.atlgq.springcloud.entities.Payment;
import com.atlgq.springcloud.service.PaymentService;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;

@Service
public class PaymentServiceImpl implements PaymentService {
    //跟autowired类似
    @Resource
    private PaymentDao paymentDao;

    @Override
    public int create(Payment payment) {
        return paymentDao.create(payment);
    }

    @Override
    public Payment getPaymentById(Long id) {
        return paymentDao.getPaymentById(id);
    }
}

```

#### 7,controller

```java
package com.atlgq.springcloud.controller;

import com.atlgq.springcloud.dao.PaymentDao;
import com.atlgq.springcloud.entities.CommonResult;
import com.atlgq.springcloud.entities.Payment;
import com.atlgq.springcloud.service.PaymentService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

@RestController
@Slf4j
public class PaymentController {

    @Resource
    private PaymentService paymentService;

    @PostMapping(value = "/payment/create")
    public CommonResult create(Payment payment){
        int result = paymentService.create(payment);
        log.info("**********返回结果为： " + result);

        if(result >  0){
            return new CommonResult(200,"插入数据库成功",result);
        }else{
            return new CommonResult(404,"插入数据库失败",null);
        }
    }

    @GetMapping("/payment/get/{id}")
    public CommonResult getPaymentById(@PathVariable("id") Long id){
        Payment paymentById = paymentService.getPaymentById(id);
        log.info("**********返回结果为： " + paymentById);

        if(paymentById != null){
            return new CommonResult(200,"查询成功",paymentById);
        }else{
            return new CommonResult(404,"查询失败，没有对应的记录 id = " + id,null);
        }
    }
}

```



## 3,热部署:只需在开发阶段使用

##### 1、把热部署需要的依赖添加到pom.xml文件中

```xml
 <!--热部署-->
<dependency>           	
    <groupId>org.springframework.boot</groupId>
  	 <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
   	<optional>true</optional>
</dependency>
```





##### 2、开启自动编译

![image-20201214155309320](Images\image-20201214155309320.png)

##### 3、ctrl shift alt / 

![image-20201214155332744](Images\image-20201214155332744.png)

##### 4、重启



## 4,order模块

```java
cloud-consumer-order80
```

### **1,pom**		

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
<parent>
    <artifactId>cloud2020</artifactId>
    <groupId>com.atlgq.springcloud</groupId>
    <version>1.0-SNAPSHOT</version>
</parent>
<modelVersion>4.0.0</modelVersion>

<artifactId>cloud-consumer-order80</artifactId>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--监控-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>

    <!--热部署-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <scope>runtime</scope>
        <optional>true</optional>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

</dependencies>
</project>
```



#### **2,yml配置文件**

配置端口即可80

#### **3,主启动类**

#### **4.复制pay模块的实体类,entity类**

#### **5,写controller类**，这里要调用8081的pay服务需要使用template

​		因为这里是消费者类,主要是消费,那么就没有service和dao,需要调用pay模块的方法

​		并且这里还没有微服务的远程调用,那么如果要调用另外一个模块,则需要使用基本的api调用

使用RestTemplate调用pay模块,

​	将restTemplate注入到容器

​	1、新写一个包config，里面建立类

```java
package com.atlgq.springcloud.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ApplicationContextConfig {
    @Bean
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
```

开启idea的run dashboard

在.idea文件下的workplace.xml下搜索dashboard

然后添加

```xml
<option name="configurationTypes">
	<set>
		<option value="SpringBootApplicationConfigurationType"/>
	</set>
</option>
3. 最终配置如下：最终配置如下：
<component name="RunDashboard">
  <option name="configurationTypes">
    <set>
      <option value="SpringBootApplicationConfigurationType" />
    </set>
  </option>
  <option name="ruleStates">
    <list>
      <RuleState>
        <option name="name" value="ConfigurationTypeDashboardGroupingRule" />
      </RuleState>
      <RuleState>
        <option name="name" value="StatusDashboardGroupingRule" />
      </RuleState>
    </list>
  </option>
  <option name="contentProportion" value="0.20013662" />
</component>
```

##### 重启

#### 6、编写controller类

```java
 @GetMapping("/consumer/payment/get/{id}")
    public CommonResult<Payment> getPayment(@PathVariable("id") Long id){
        return restTemplate.getForObject(PAYMENT_URL+"/payment/get/"+id, CommonResult.class);
    }
```



#### 7、这个时候使用

#### http://localhost/consumer/payment/create?serial=12345

#### 是有错误的，有主键生成但是内容却为空，此时应该在相对应的controller里面添加

@RequestBody

```java
 @PostMapping(value = "/payment/create")
    public CommonResult create(@RequestBody Payment payment)
```





## 5,重构,

新建一个模块,将重复代码抽取到一个公共模块中

### 1,创建commons模块，添加pox.xml

cloud-api-commons

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.1.0</version>
        </dependency>
    </dependencies>
```



### 2,抽取公共pom

### 3,entity和实体类放入commons中

### 4,使用mavne,将commone模块打包(install),

![image-20201219204459876](Images\image-20201219204459876.png)

​	首先我们应该先确定maven仓的配置是正确的

​	其他模块引入commons

### 5、我们删除8081和80下的entities，然后在pom.xml去引入它

```xml
 <!--把自己发布的类-->
        <dependency>
            <groupId>com.atlgq.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
```

### 6、我在这里错误卡了十分久，原因是commons里面的类没有加载进去，只需要重新粘贴一遍就可以

