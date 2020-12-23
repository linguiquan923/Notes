SpringCloud:



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

![image-20201106085837608](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201106085837608.png)

##### 6、注解生效激活

![image-20201106085948057](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201106085948057.png)

##### 7、java编译版本选8

![image-20201106090025264](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201106090025264.png)

##### 8、filetype文件过滤

##### 9、删掉src，在pom.xml文件里面添加 

#####  **<packaging>pom</packaging>**

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
      <!--spring cloud Hoxton.SR1-->
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

## 2,创建子模块,pay模块

​	1、建module

​	2、改pom

​	3、写YML

​	4、主启动

​	5、业务类

### 1,子模块名字:

​		cloud-provider-payment8001

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
  port: 8001

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
public class PaymentMain8001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8001.class,args);
    }
}

```



### 5,业务类

#### 1,sql

```sql
 create table paymen
 (id int NOT NULL primary key auto_increment comment 'ID',
  serial varchar(200)
 )ENGINE=InnoDB auto_increment=1 default charset=utf8;
```



#### 	2,实体类

Payment

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

<mapper namespace="com.atlgq.dao.">


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
<dependency>           	<groupId>org.springframework.boot</groupId>
  	 <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
   	<optional>true</optional>
</dependency>
```





##### 2、开启自动编译

![image-20201214155309320](E:\StudyNote\Images\image-20201214155309320.png)

##### 3、ctrl shift alt / 

![image-20201214155332744](E:\StudyNote\Images\image-20201214155332744.png)

##### 4、重启



## 4,order模块

​	新建模块

#### **1,pom**		

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

#### **5,写controller类**，这里要调用8001的pay服务需要使用template

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
package com.atlgq.springcloud.controller;

        import com.atlgq.springcloud.entities.CommonResult;
        import com.atlgq.springcloud.entities.Payment;
        import lombok.extern.slf4j.Slf4j;
        import org.springframework.web.bind.annotation.GetMapping;
        import org.springframework.web.bind.annotation.PathVariable;
        import org.springframework.web.bind.annotation.RestController;
        import org.springframework.web.client.RestTemplate;

        import javax.annotation.Resource;

@RestController
@Slf4j
public class OrderController {

//    private  static final String PAYMENT_URL="http://localhost:8001";
    private  static final String PAYMENT_URL="http://CLOUD-PAYMENT-SERVICE";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping("consumer/payment/create")
    public CommonResult<Payment> create(Payment payment){
        return restTemplate.postForObject(PAYMENT_URL+"/payment/create",payment, CommonResult.class);
    }

    @GetMapping("consumer/payment/get/{id}")
    public CommonResult<Payment> getPayment(@PathVariable("id") Long id){
        return restTemplate.getForObject(PAYMENT_URL+"/payment/get/"+id, CommonResult.class);
    }
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

### 4,使用mavne,将commone模块打包(install),这时候clean的要看是否成功，如果有提醒或者报错，那么应该修正，否则是无法发布出去的



​	首先我们应该先确定maven仓的配置是正确的

​	其他模块引入commons

### 5、我们删除8001和80下的entities，然后在pom.xml去引入它

```xml
 <!--把自己发布的类-->
        <dependency>
            <groupId>com.atlgq.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
```

### 6、我在这里错误卡了十分久，原因是commons里面的类没有加载进去，只需要重新粘贴一遍就可以





#### ############################零基础分割线#######################



# 2,服务注册与发现



## 6,Eureka:

前面我们没有服务注册中心,也可以服务间调用,为什么还要服务注册?

当服务很多时,单靠代码手动管理是很麻烦的,需要一个公共组件,统一管理多服务,包括服务是否正常运行,等

Eureka用于**==服务注册==**,目前官网**已经停止更新**

### **单机版eureka:**

#### **1,创建项目cloud_eureka_server_7001**

#### **2,引入pom依赖**

```xml
<dependencies>
        <!-- eureka-server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
        <!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
        <dependency>
            <groupId>com.atlgq.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--监控-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!-- 一般通用配置 -->
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
```



#### 3,配置文件:

```yml
server:
  port: 7001

eureka:
  instance:
    hostname: localhost #eureka服务端的实例名称
  client:
    register-with-eureka: false
    #false表示不向服务中心注册自己
    fetch-registry: false
    #false表示自己就是服务中心，我的职责是维护服务实例，并不需要去检索服务
    service-url:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
      #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址

```



#### 4,主启动类	

```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaMain7001 {
    public static void main(String[] args) {
        SpringApplication.run(EurekaMain7001.class,args);
    }
}

```



#### **5,此时就可以启动当前项目了**

```java
http://localhost:7001/
```

#### **6,其他服务注册到eureka:**

比如此时pay模块加入eureka:

##### 1.主启动类上,加注解,表示当前是eureka客户端

```
@EnableEurekaClient
```

##### 2,修改pom,引入

```xml
 <!-- eureka-server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
```



##### 3,修改配置文件:

```xml
eureka:
  client:
    register-with-eureka: true
    #表示向服务总心注册自己
    fetch-registry: true
    #true表示从eurekaServer注册中心抓取已有信息，默认为true，单节点是无所谓的，集群必须设置为true的话，才能够配合ribbon使用负载均衡
    service-url:
      defaultZone: http://localhost:7001/eureka
      #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址
```



##### 4,pay模块重启,就可以注册到eureka中了

```xml
eureka:
  instance:
    hostname: localhost #eureka服务端的实例名称
很重要，是入住服务中心的名称，要统一
```





**==order模块的注册是一样的==**

1、order的注册是需要添加名字的，在yml文件添加

```xm
eureka:
  instance:
    hostname: cloud-order-service
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:7001/eureka
```



### 集群版eureka:

#### 集群原理:

 ```java
1,就是pay模块启动时,注册自己,并且自身信息也放入eureka
2.order模块,首先也注册自己,放入信息,当要调用pay时,先从eureka拿到pay的调用地址
3.通过HttpClient调用
 	并且还会缓存一份到本地,每30秒更新一次
 ```

**集群构建原理:**

​		互相注册,互相观望

#### **构建新erueka项目**

名字:cloud_eureka_server_7002

##### 1,pom文件:

​		粘贴7001的即可

##### 2,配置文件:

​		在写配置文件前,修改一下主机的hosts文件（）

![image-20201214155714871](E:\StudyNote\Images\image-20201214155714871.png)

首先修改之前的7001的eureka项目,因为多个eureka需要互相注册，修改主机名和注册地址，向7002注册自己

```yml
server:
  port: 7001

eureka:
  instance:
    hostname: eureka7001.com #eureka服务端的实例名称
  client:
    register-with-eureka: false
    #false表示不向服务中心注册自己
    fetch-registry: false
    #false表示自己就是服务中心，我的职责是维护服务实例，并不需要去检索服务
    service-url:
      defaultZone: http://eureka7002.com:7002/eureka/
      #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址

```



然后修改7002，向7001注册自己

```yml
server:
  port: 7002

eureka:
  instance:
    hostname: eureka7002.com #eureka服务端的实例名称
  client:
    register-with-eureka: false
    #false表示不向服务中心注册自己
    fetch-registry: false
    #false表示自己就是服务中心，我的职责是维护服务实例，并不需要去检索服务
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/
      #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址

```



​			**7002也是一样的,只不过端口和地址改一下**

##### 3,主启动类:

```java
package com.atlgq.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer
public class EurekaMain7002 {
    public static void main(String[] args) {
        SpringApplication.run(EurekaMain7002.class,args);
    }
}

```



##### 4,然后启动7001,7002即可

![image-20201214155849906](E:\StudyNote\Images\image-20201214155849906.png)





#### 将pay,order模块注册到eureka集群中:

##### 1,只需要修改配置文件即可:

```xml
service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
      #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址
```

##### 2,两个模块都修改上面的都一样即可

​			然后启动两个模块

​			要先启动7001,7002,然后是pay模块8001,然后是order(80)



### 微服务集群配置

#### 0,创建新模块,8002

​	名称: cloud_pay_8002

#### 1,pom文件,复制8001的

#### 2,pom文件复制8001的

#### 3,配置文件复制8001的

​		端口修改一下,改为8002

​		服务名称不用改,用一样的

#### 4.主启动类,复制8001的名称改为8002

#### 5,mapper,service,controller都复制一份

​		在controller中获取到端口号，然后



可以用来看访问的是哪一个端口

​		然后就启动服务即可

​		此时访问order模块,发现并没有负载均衡到两个pay,模块中,而是只访问8001

​		虽然我们是使用RestTemplate访问的微服务,但是也可以负载均衡的

#### 6、负载均衡

**注意这样还不可以,需要让RestTemplate开启负载均衡注解,还可以指定负载均衡算法,默认轮询**

因为我们在这里把OrderMain里面的数据写死，所以无法负载均衡，要修改

```java
//    private  static final String PAYMENT_URL="http://localhost:8001";
    private  static final String PAYMENT_URL="http://CLOUD-PAYMENT-SERVICE";
```

还要在config中添加@LoadBalanced



​						如果Ribbon和Eureka整合后，Consumer可以直接调用服务而不用关心地址和端口号，且该服务还有负载功能了。



### ############################2020年11月6日##################################





### 4,修改服务主机名和ip在eureka的web上显示

比如修改pay模块，修改服务名称

#### 1,修改配置文件:instance是和client对齐的

```yml
instance:
    instance-id: payment8001
```

![image-20201107085903284](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201107085903284.png)

```java
http://localhost:8002/actuator/health
用来检查服务是否成功挂载
```

#### 2、显示ip地址

![image-20201107090449427](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201107090449427.png)

![image-20201107090506415](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201107090506415.png)



### 5,eureka服务发现:



以pay模块为例

#### 1,首先添加一个注解,在8001的controller中

​	在controller添加

```java
 @Resource
    private DiscoveryClient discoveryClient;

```

然后在controller里面添加一个方法

```java
 @GetMapping(value = "/payment/discover")
    public Object discover(){
        List<String> services = discoveryClient.getServices(); //能够查到所注册的微服务的名称
        for (String service : services) {
            log.info("************services  = " + service ) ;
        }

        List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
        //能够查到微服务的各种信息
        for (ServiceInstance instance : instances) {
            log.info(instance.getServiceId() + "\t" + instance.getHost() + "\t" + instance.getPort() + "\t" + instance.getUri()) ;
        }
        return this.discoveryClient;
    }
```

#### 2,在主启动类上添加一个注解

```
@EnableDiscoveryClient

```

**然后重启8001.访问http://localhost:8001/payment/discover**

![image-20201107092223639](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201107092223639.png)![image-20201107092232187](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201107092232187.png)





### 6,Eureka自我保护:

​	

- Eureka自我保护的产生原因：

Eureka在运行期间会统计心跳失败的比例，在15分钟内是否低于85%,如果出现了低于的情况，Eureka Server会将当前的实例注册信息保护起来，同时提示一个警告，一旦进入保护模式，Eureka Server将会尝试保护其服务注册表中的信息，不再删除服务注册表中的数据。也就是不会注销任何微服务。因为可能是因为网络延迟的原因导致没有规定时间内发送心跳，eureka为了保护微服务，并不会马上把这个微服务给清除出去，属于CAP的AP分支



#### 	1、怎么禁止自我保护

**eureka服务端配置:**		**设置接受心跳时间间隔**

> ```yml
> eureka:
>   instance:
>     hostname: eureka7001.com #eureka服务端的实例名称
>   client:
>     register-with-eureka: false
>     #false表示不向服务中心注册自己
>     fetch-registry: false
>     #false表示自己就是服务中心，我的职责是维护服务实例，并不需要去检索服务
>     service-url:
>       defaultZone: http://eureka7002.com:7002/eureka/
>       #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址
>   server:
>     enable-self-preservation: false #这是出厂默认开启保护机制，这次为false是关闭保护机制
>     eviction-interval-timer-in-ms: 2000 #设置心跳时间间隔
> ```

修改成功后访问http://eureka7001.com:7001/会出现

![image-20201107093702466](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201107093702466.png)

​	



**客户端(比如pay模块):**

```yml
eureka:
  client:
    register-with-eureka: true
    #true表示向服务总心注册自己
    fetch-registry: true
    #true表示从eurekaServer注册中心抓取已有信息，默认为true，单节点是无所谓的，集群必须设置为true的话，才能够配合ribbon使用负载均衡
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
      #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址
  instance:
    instance-id: payment8001
    prefer-ip-address: true #这是显示ip地址的
    lease-renewal-interval-in-seconds: 1 #Eureka客户端向服务端发送心跳的时间间隔，单位时间为秒（默认30秒）
    lease-expiration-duration-in-seconds: 2 #Eureka服务端在收到最后一次心跳等待之后的时间上限，单位为秒（默认90），超过时间久剔除服务
```



**此时启动erueka和pay.此时如果直接关闭了pay8001,那么erueka会直接删除其注册信息**

![image-20201107094235941](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201107094235941.png)







## 7,Zookeeper服务注册与发现:

### 1,启动zk,到linux上

```java
export ZOOKEEPER_HOME=/home/ubuntu/usr/local/service/zookeeper/zookeeper-3.4.9/
export PATH=$ZOOKEEPER_HOME/bin:$PATH
export PATH
```

​	在配置zookeeper的时候，我只配置了一个服务节点，这是无法启动服务的，因为根据zookeeper的选举法得知，整个集群超过一半机器宕机，zookeeper是认为服务不可用状态，所以这是无法连接的。得启动两台以上的服务，那么才可以使用。

### 2,创建新的pay模块,

单独用于注册到zk中  

名字 : cloud-provider-payment8004

#### 1,pom依赖

```xml
<dependencies>
    <dependency><!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.atlgq.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--监控-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--SpringBoot整合Zookeeper客户端-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
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
```



#### 2,配置文件application.xml

```yml
server:
  port: 8004

spring:
  application:
    name: cloud-provider-payment
  cloud:
    zookeeper:
      connect-string: 192.168.0.11:2181 
```



#### 3,主启动类

```xml
package com.atlgq.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class PaymentMain8004 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8004.class,args);
    }
}

```



#### 4,8004的controller

```java
package com.atlgq.springcloud.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.UUID;

@RestController
@Slf4j
public class PaymentController {

    @Value("${server.port}")
    private String serverPort;
    @GetMapping(value = "/payment/zk")
    public String paymentzk(){
        return "springcloud with zookeeper : " + serverPort + "\t" + UUID.randomUUID().toString();
    }
}

```



#### 5,然后就可以启动

**此时启动,会报错,因为jar包与我们的zk版本不匹配**

解决:
		修改pom文件,改为与我们zk版本匹配的jar包

**此时8004就注册到zk中了**

```java
我们在zk上注册的node是临时节点,当我们的服务一定时间内没有发送心跳
  	那么zk就会`将这个服务的node删除了
```



**这里测试,就不写service与dao什么的了**





### 3,创建order消费模块注册到zk

#### 1,创建项目

名字: cloud_order_zk_80

#### 2,pom（和8004一样）

#### 3,配置文件（和8004一样，但是端口号改成80，spring.applciation.name=cloud-consumer-order）

#### 4主启动类:8004一致

#### 5,RestTemolate

1、在config里面写一个ApplicationContextConfig

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



#### 6,controller



**然后启动即可注册到zk**

#### 8,集群版zk注册:

只需要修改配置文件:

![](.\图片\zookeeper的5.png)

这个connect-string指定多个zk地址即可

connect-string: 1.2.3.4,2.3.4.5







## 8,Consul:

![consul的1](D:\Desktop\studyDocument\springcloud下载的笔记\图片\consul的1.png)

![consul的2](D:\Desktop\studyDocument\springcloud下载的笔记\图片\consul的2.png)

### 1,按照consul

需要下载一个安装包

官网搜索下载

启动是一个命令行界面,需要输入consul agen-dev启动，这是启动视频的网址

```java
https://learn.hashicorp.com/tutorials/consul/get-started-install
```

在windows环境下添加path，添加consul的环境变量，在cmd输入consul如果有输出，那么正确

在cmd输入

```java
consul agent -dev可以启动服务，开发模式启动
    localhost:8500
        可以访问
```



### 2,创建新的pay模块,8006

#### 1,项目名字

cloud-providerconsule-payment8006

#### 2,pom依赖

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

    <artifactId>cloud-providerconsule-payment8006</artifactId>

    <dependencies>
        <dependency><!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.atlgq.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--监控-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--SpringCloud consul-server-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
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

#### 3,配置文件

```yml
server:
  port: 8500

spring:
  application:
    name: consul-provider-payment
############consul注册中心地址
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```



#### 4,主启动类

![image-20201107185124554](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201107185124554.png)

#### 5,controller

![image-20201107185252631](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201107185252631.png)

#### 6,启动服务



#### 



### 3,创建新order模块

cloud-comsumerconsul-order80

#### 1,pom文件(和payment一样)

#### 2,配置文件

```yml
server:
  port: 80

spring:
  application:
    name: consul-consumer-order
  ############consul注册中心地址
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```



#### 3,主启动类（一样）

#### 4,RestTemplate注册，配种Bean

配置类注册

#### 5,controller

```java
package com.atlgq.springcloud.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;

@RestController
@Slf4j
public class OrderConsulController {

    public static final String INVOKE_URL = "http://consul-provider-payment";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping(value = "/consumer/consul")
    public String paymentInfo(){
        String result = restTemplate.getForObject(INVOKE_URL + "/payment/consul", String.class);
        return result;
    }


}

```



#### 6,启动服务,测试

![image-20201107191410608](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201107191410608.png)



## 9,三个注册中心的异同:

CAP:CAP理论关注的是数据，而不是整体系统的设计，三者只能满足两者

C:Consistency（强一致性）

A:Availability（可用性）

P:Partition tolerance（分区容错性）

![consul的9](D:\Desktop\studyDocument\springcloud下载的笔记\图片\consul的9.png)



![img](file:///C:\Users\Administrator\Documents\Tencent Files\1446070006\Image\C2C\1CDFE9FC39616FEA99A1DF230507AF32.jpg)





![consul的10](D:\Desktop\studyDocument\springcloud下载的笔记\图片\consul的10.png)

![consul的11](D:\Desktop\studyDocument\springcloud下载的笔记\图片\consul的11.png)

# 3,服务调用



## 10,Ribbon负载均衡:

![Ribbon](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Ribbon.png)

**Ribbon目前也进入维护,基本上不准备更新了**

![Ribbon的2](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Ribbon的2.png)

**进程内LB(本地负载均衡)**

![Ribbon的3](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Ribbon的3.png)



**集中式LB(服务端负载均衡)**

![Ribbon的4](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Ribbon的4.png)

**区别**

![Ribbon的5](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Ribbon的5.png)



**Ribbon就是负载均衡+RestTemplate**

![Ribbon的6](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Ribbon的6.png)

![Ribbon的8](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Ribbon的8.png)

![Ribbon的9](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Ribbon的9.png)



### 使用Ribbon:

ribbon是一个软负载均衡的客户端组件

#### 1,默认我们使用eureka的新版本时,它默认集成了ribbon，所以我们使用eureka的时候是不需要引入ribbon的

```xml
 <!-- eureka-server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
```

**==这个starter中集成了reibbon了==**



#### 2,我们也可以手动引入ribbon

**放到order模块中,因为只有order访问pay时需要负载均衡**

```java
 @GetMapping("/consumer/payment/getForEntity/{id}")
    public CommonResult<Payment> getPayment2(@PathVariable("id") Long id){
        ResponseEntity<CommonResult> forEntity = restTemplate.getForEntity(PAYMENT_URL + "/payment/get/" + id, CommonResult.class);
        if(forEntity.getStatusCode().is2xxSuccessful()){
            return forEntity.getBody();
        }else{
            return new CommonResult<>(444,"操作失败");
        }
    }
```



#### 3,RestTemplate类:



```java
package com.atlgq.springcloud.controller;

import com.atlgq.springcloud.entities.CommonResult;
import com.atlgq.springcloud.entities.Payment;
import lombok.extern.slf4j.Slf4j;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;

@RestController
@Slf4j
public class OrderController {

//    private  static final String PAYMENT_URL="http://localhost:8001";
    private  static final String PAYMENT_URL="http://CLOUD-PAYMENT-SERVICE";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping("/consumer/payment/create")
    public CommonResult<Payment> create(Payment payment){
        return restTemplate.postForObject(PAYMENT_URL+"/payment/create",payment, CommonResult.class);
    }

    @GetMapping("/consumer/payment/get/{id}")
    public CommonResult<Payment> getPayment(@PathVariable("id") Long id){
        return restTemplate.getForObject(PAYMENT_URL+"/payment/get/"+id, CommonResult.class);
    }

    @GetMapping("/consumer/payment/getForEntity/{id}")
    public CommonResult<Payment> getPayment2(@PathVariable("id") Long id){
        ResponseEntity<CommonResult> forEntity = restTemplate.getForEntity(PAYMENT_URL + "/payment/get/" + id, CommonResult.class);
        if(forEntity.getStatusCode().is2xxSuccessful()){
            return forEntity.getBody();
        }else{
            return new CommonResult<>(444,"操作失败");
        }
    }
}

```

```java
RestTemplate的:
	xxxForObject()方法,返回的是响应体中的数据
    xxxForEntity()方法.返回的是entity对象,这个对象不仅仅包含响应体数据,还包含响应体信息(状态码等)
```





#### Ribbon常用负载均衡算法:

**IRule接口,Riboon使用该接口,根据特定算法从所有服务中,选择一个服务,**

**Rule接口有7个实现类,每个实现类代表一个负载均衡算法**

#### 负载均衡算法

![image-20201107201450310](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201107201450310.png)

![img](file:///C:\Users\Administrator\Documents\Tencent Files\1446070006\Image\C2C\28952DA71D967FF0B85B5C91ED7ABB33.jpg)





#### 使用Ribbon:

**==这里使用eureka的那一套服务，在Eureka的80包下操作==**

**==也就是不能放在主启动类所在的包及子包下，官网里面说不能放在@ComponentScan扫描的包下面，就是不能放在主启动类的包里面，所以我们就新建一个包com.atlgq.myrule==**

##### 1,修改order模块

##### 2,额外创建一个包，包下有一个MyRule的类，配置类

```


```

##### 3,创建配置类,指定负载均衡算法

```java
package com.atlgq.myrule;

import com.netflix.loadbalancer.IRule;
import com.netflix.loadbalancer.RandomRule;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MySelRule {

    @Bean
    public IRule myRule(){
        //我们这里采用的是随机
        return new RandomRule();
    }
}

```

##### 4,在主启动类上加一个注解@RibbonClient(name = "CLOUD-PAYMENT-SERVICE",configuration = MySelRule.class)，name指名自己要访问的服务，然后configuration指定的是自己自定义的轮转规则，默认的负载均衡算法是轮转

```java
package com.atlgq.springcloud;

import com.atlgq.myrule.MySelRule;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.cloud.netflix.ribbon.RibbonClient;

@SpringBootApplication
@EnableEurekaClient
@RibbonClient(name = "CLOUD-PAYMENT-SERVICE",configuration = MySelRule.class)
public class OrderMain {
    public static void main(String[] args) {
        SpringApplication.run(OrderMain.class,args);
    }
}

```

**表示,访问CLOUD_pAYMENT_SERVICE的服务时,使用我们自定义的负载均衡算法**

测试，端口号是随即出现的

```java
访问：http://localhost/consumer/payment/get/1 
```





#### 手写负载均衡算法:

##### 1,ribbon的轮询算法原理

![Ribbon的19](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Ribbon的19.png)

![Ribbon的21](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Ribbon的21.png)

原理重点：CAS底层原理和java自旋锁，B站，尚硅谷周阳，大厂高额重点面试题



##### 2,自定义负载均衡算法:

**1,给**pay模块(8001,8002),的controller方法添加一个方法,返回当前节点端口

```java
  @GetMapping(value = "/payment/lb")
    public String getPaymentLB(){
        return serverPort;
    }
```



**2,修改order模块**

在eureka的80的config下去掉@LoadBalanced



##### 3,自定义接口，在springcloud下新建lb包，新建接口

```java
package com.atlgq.springcloud.lb;

import org.springframework.cloud.client.ServiceInstance;

import java.util.List;

public interface LoadBalancer {
    //通过这一个可以获取所有服务，并且加入到list中
    ServiceInstance instances(List<ServiceInstance> serviceInstances);
}

```



##### 4,接口实现类

```java
package com.atlgq.springcloud.lb;

import org.springframework.cloud.client.ServiceInstance;
import org.springframework.stereotype.Component;

import java.util.List;
import java.util.concurrent.atomic.AtomicInteger;

@Component //加入注解，把这个加入到ioc容器中才能被自动扫描
public class MyLB implements LoadBalancer {


    //AtomicInteger这是一个具有原子性能返回Integer的操作类，具有线程安全的方式操作加减，十分适合高并发的情况下使用
    //定义一个值为0的初始值
    private AtomicInteger atomicInteger = new AtomicInteger(0);

    //方法很重要，所以直接写死，在这里获取访问的次数并且返回
    private final int getAndIncrement(){
        int current;
        int next;
        do{
            //获取当前访问的次数,为0
            current = atomicInteger.get();
            //如果next大于整数的最大值，那么为0，否则为current + 1，此时next为1，即第一次
            next = current > Integer.MAX_VALUE ? 0 : current + 1;
            //此时current为0，next为1，atomicInter.get()与current比较值相同，所以把current设置为1，返回true,如果数值被修改了，那么返回false，这是一种类似于锁的操作，所以用!true跳出循环
            //此时return 1 ， 下一次以此类推，
        }while (!this.atomicInteger.compareAndSet(current,next));
        System.out.println("next = " + next);
        return next;
    }

    @Override
    public ServiceInstance instances(List<ServiceInstance> serviceInstances) {
        //通过除整取余的方法获取调用服务下标
        int index = getAndIncrement() % serviceInstances.size();
        //返回调用的服务
        return serviceInstances.get(index);
    }
}

```





##### 5,修改order下面的controller:

```java
@GetMapping("/comsumer/payment/lb")
    public String getPaymentLB(){
        //获取服务器上所有的服务，通过discoveryClient发现
        List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
        if(instances == null || instances.size() <= 0 ){
            return null;
        }
        //把这些服务注入到自定义的负载中
        ServiceInstance serviceInstance = loadBalancer.instances(instances);
        //获取返回的服务地址
        URI uri = serviceInstance.getUri();
        //最终返回要访问的地址
        return restTemplate.getForObject(uri + "/payment/lb",String.class);

        }
```





##### 6,启动服务,测试即可

```java
http://localhost/comsumer/payment/lb
```



### #################2020.11.07#########################















## 11,OpenFeign

![](.\图片\Feign的1.png)

**是一个声明式的web客户端,只需要创建一个接口,添加注解即可完成微服务之间的调用**



![Feign的2](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Feign的2.png)



==就是A要调用B,Feign就是在A中创建一个一模一样的B对外提供服务的的接口,我们调用这个接口,就可以服务到B==



### **Feign与OpenFeign区别**

![Feign的3](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Feign的3.png)





### 使用OpenFeign

```java
之前的服务间调用,我们使用的是ribbon+RestTemplate
		现在改为使用Feign
```

#### 1,新建一个order项目,用于feign测试

名字cloud-consumer-feign-order80

#### 2,pom文件

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

    <artifactId>cloud-consumer-feign-order80</artifactId>
    <dependencies>
        <!-- openfeign -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <!--eureka client-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        
        <dependency><!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.atlgq.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
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



#### 3,配置文件yml

```yml
server:
  port: 80

eureka:
  client:
    register-with-eureka: false
    #表示不想集群中心注册自己
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
```

#### 4,主启动类，要添加上@EnableFeignClients注解

```java
@SpringBootApplication
@EnableFeignClients
public class OrderFeignMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderFeignMain80.class,args);
    }
}
```



#### 5,fegin需要调用的其他的服务的接口，新建service包

```java
package com.atlgq.springcloud.service;

import com.atlgq.springcloud.entities.CommonResult;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@Component
@FeignClient(value = "CLOUD-PAYMENT-SERVICE") //表示调用集群里面的CLOUD-PAYMENT-SERVICE的服务
public interface PaymentFeignService {
    //这些是服务里面对应的方法
    @GetMapping("/payment/get/{id}")
    public CommonResult getPaymentById(@PathVariable("id") Long id);
}

```





#### 6,controller

```java
@RestController
@Slf4j
public class OrderFeignController {
    @Resource
    private PaymentFeignService paymentFeignService;

    @GetMapping(value = "/consumer/payment/get/{id}")
    public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id)
    {
        System.out.println("#################controller");
        return paymentFeignService.getPaymentById(id);
    }
}

```



#### 7测试:

启动两个erueka(7001,7002)

启动两个pay(8001,8002)

启动当前的order模块

```java
http://localhost/consumer/payment/get/2
```





**Feign默认使用ribbon实现负载均衡**



### OpenFeign超时机制:

==OpenFeign默认等待时间是1秒,超过1秒,直接报错==

#### 1,设置超时时间,修改配置文件:

**因为OpenFeign的底层是ribbon进行负载均衡,所以它的超时时间是由ribbon控制**

​	1、现在controller里面写一个超时的访问，8001

```java

    @GetMapping(value = "/payment/timeout")
    public String getTimeOut(){
        try {
            TimeUnit.SECONDS.sleep(3);
        }catch (InterruptedException e){
            e.printStackTrace();
        }
        return serverPort;
    }
```

​	2、，此时访问是出错的，修改80的yml文件

```yml
#设置客户超时时间
ribbon:
  #指的是建立连接两端所用的时间，适用于网络正常的情况下，两端连接所用的时间
  ReadTimeout: 5000
  #建立连接后从服务器读取到可用资源的所用时间
  ConnectTimeout: 5000
```

即可访问





### OpenFeign日志:

![Feign的9](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Feign的9.png)



**OpenFeign的日志级别有:**
![Feign的10](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Feign的10.png)





#### 	1,使用OpenFeign的日志:

**实现在配置类中添加OpenFeign的日志类**

```java
package com.atlgq.springcloud.config;
import feign.Logger;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class FeignConfig {

    @Bean
    Logger.Level feignLoggerLevel(){
        //FULL是最全面的
        return Logger.Level.FULL;
    }
}

```



#### 2,为指定类设置日志级别:

**配置文件中:**

```yml
logging:
  level:
    com.atlgq.springcloud.service.PaymentFeignService: debug
```



#### 	3,启动服务即可

![image-20201108104250889](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201108104250889.png)



#### ######################基础分割线#####################

# 4,服务降级:



## 12,Hystrix服务降级

![Hystrix的2](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Hystrix的2.png)



![Hystrix的3](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Hystrix的3.png)

![Hystrix的4](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Hystrix的4.png)











### hystrix中的重要概念:

#### 1,服务降级

**比如当某个服务繁忙,不能让客户端的请求一直等待,应该立刻返回给客户端一个备选方案**FallBack

![image-20201108105048139](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201108105048139.png)

#### 2,服务熔断

**当某个服务出现问题,卡死了,不能让用户一直等待,需要关闭所有对此服务的访问**

​			**然后调用服务降级**

![image-20201108105404479](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201108105404479.png)

#### 3,服务限流

**限流,比如秒杀场景,不能访问用户瞬间都访问服务器,限制一次只可以有多少请求**





### 使用hystrix,服务降级:

##### 1,创建带降级机制的pay模块 :

名字: cloud-provider-hystrix-payment8001

##### 2,pom文件

```xml
<dependencies>
        <dependency><!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.atlgq.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--监控-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!-- hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
        <!--eureka client-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
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
```



##### 3,配置文件

```yml
server:
  port: 8001

spring:
  application:
    name: cloud-provider-hystrix-payment

eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka

```



##### 4,主启动类

```java
@SpringBootApplication
@EnableEurekaClient
public class PaymentHystrixMain8001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentHystrixMain8001.class,args);
    }
}
```

##### 5,service

创建接口和实现类

```java
package com.atlgq.springcloud.service;

import org.springframework.stereotype.Service;

import java.util.concurrent.TimeUnit;

@Service
public class PaymentServiceImpl implements PaymentService {
    @Override
    public String payment_ok(Integer id) {

        return "线程池： " + Thread.currentThread().getName() + "  payment_ok,id: " + id;
    }

    @Override
    public String payment_timeout(Integer id) {
        int timeoutNumber = 3;
        try {
            TimeUnit.SECONDS.sleep(timeoutNumber);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "线程池： " + Thread.currentThread().getName() + "  payment_timeout,id: " + id;
    }
}

```





##### 6controller

```java
package com.atlgq.springcloud.controller;

import com.atlgq.springcloud.service.PaymentService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

@RestController
@Slf4j
public class PaymentController {

    @Resource
    private PaymentService paymentService;

    @GetMapping(value = "/payment/hystrix/ok/{id}")
    public String payment_ok(@PathVariable("id") Integer id){
        return  paymentService.payment_ok(id);
    }

    @GetMapping(value = "/payment/hystrix/timeout/{id}")
    public String payment_timeout(@PathVariable("id") Integer id){
        return  paymentService.payment_timeout(id);
    }
}

```



##### 7,先测试:以上为跟基平台，从正确->错误->降级熔断->恢复

```java
http://localhost:8001/payment/hystrix/ok/2
http://localhost:8001//payment/hystrix/timeout/2
```

​	

JMETER工具

![image-20201108132139330](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201108132139330.png)

![image-20201108132218047](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201108132218047.png)



```java
此时使用压测工具,并发20000个请求,请求会延迟的那个方法,
		压测中,发现,另外一个方法并没有被压测,但是我们访问它时,却需要等待
		这就是因为被压测的方法它占用了服务器大部分资源,导致其他请求也变慢了，tomcat的默认工作线程被打满了，没有多余的线程可以分解压力和处理
```



##### 8,先不加入hystrix,



#### 2,创建带降级的order模块:

##### 1,名字:  cloud-consumer-feign-hystrix-order80

##### 2,pom

```xml
<dependencies>
        <!-- openfeign -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <!--hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
        <!--eureka client-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>


        <!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
        <dependency>
            <groupId>com.atlgq.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
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
```



##### 3,配置文件，这里有个坑，如果没有配置hystrix和ribbon的时间，会自动报错

```yml
server:
  port: 80

eureka:
  client:
    register-with-eureka: false
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka

#hystrix的超时时间
hystrix:
  command:
    default:
      execution:
        timeout:
          enabled: true
        isolation:
          thread:
            timeoutInMilliseconds: 30000
#ribbon的超时时间
ribbon:
  ReadTimeout: 30000

```



##### 4,主启动类

```java
package com.atlgq.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.openfeign.EnableFeignClients;

@SpringBootApplication
@EnableFeignClients
public class OrderHystrixMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderHystrixMain80.class,args);
    }
}

```



##### 5,远程调用pay模块的接口:

```java
package com.atlgq.springcloud.service;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@Component
@FeignClient(value = "CLOUD-PROVIDER-HYSTRIX-PAYMENT")
public interface PaymentHystrixService {

    @GetMapping(value = "/payment/hystrix/ok/{id}")
    public String payment_ok(@PathVariable("id") Integer id);

    @GetMapping(value = "/payment/hystrix/timeout/{id}")
    public String payment_timeout(@PathVariable("id") Integer id);
}

```



##### 6,controller:

```java
package com.atlgq.springcloud.controller;

import com.atlgq.springcloud.service.PaymentHystrixService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

@RestController
@Slf4j
public class OrderHystrixController {

    @Resource
    private PaymentHystrixService paymentHystrixService;

    @GetMapping(value = "/consumer/payment/hystrix/ok/{id}")
    public String payment_ok(@PathVariable("id") Integer id){
        return  paymentHystrixService.payment_ok(id);
    }

    @GetMapping(value = "/consumer/payment/hystrix/timeout/{id}")
    public String payment_timeout(@PathVariable("id") Integer id){
        return  paymentHystrixService.payment_timeout(id);
    }
}

```

##### 7,测试

​			启动order模块,访问pay，这里有个坑，由于ribbon会带一个访问的时间限制，所以说我们要在配置文件里面配置好ribbon的访问时间限制，否则会一直访问超时 报错

```java
http://localhost/consumer/payment/hystrix/timeout/2
```

​			再次压测2万并发,发现order访问也变慢了

**解决:**

![image-20201108142052340](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201108142052340.png)





#### 3,配置服务降级:

##### 1,修改pay模块，配置coud-provider-hystrix-payment8001

###### 1,为service的指定方法(会延迟的方法)添加@HystrixCommand注解，这个只能在实现类加，不能在接口上接，接口也要写上方法

```java
 @Override
    @HystrixCommand(fallbackMethod = "paymentInfo_TimeoutHandle",commandProperties = {
            @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "3000")
    })
    public String payment_timeout(Integer id) {
        int timeoutNumber = 5;
        try {
            TimeUnit.SECONDS.sleep(timeoutNumber);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
//        int a = 10 / 0;
        return "线程池： " + Thread.currentThread().getName() + "  payment_timeout,id: " + id;
    }

    @Override
    public String paymentInfo_TimeoutHandle(Integer id) {
        return "线程池： " + Thread.currentThread().getName() + "  paymentInfo_TimeoutHandle 运行报错或访问超时";
    }
```



###### 2,主启动类上,添加激活hystrix的注解@EnableCircuitBreaker

```java
@SpringBootApplication
@EnableEurekaClient
@EnableCircuitBreaker
public class PaymentHystrixMain8001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentHystrixMain8001.class,args);
    }
}

```



###### 3,触发异常

![image-20201108144230255](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201108144230255.png)



##### 2,修改order模块,进行服务降级

一般服务降级,都是放在客户端(order模块),

![Hystrix的21](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Hystrix的21.png)

###### 1,修改配置文件:

```yml
feign:
  hystrix:
    enabled: true
```



###### **2,主启动类添加直接,启用hystrix:**

```java
@SpringBootApplication
@EnableFeignClients
@EnableHystrix
public class OrderHystrixMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderHystrixMain80.class,args);
    }
}
	
```

###### 3,修改controller,添加降级方法什么的

```java
 @GetMapping(value = "/consumer/payment/hystrix/timeout/{id}")
    @HystrixCommand(fallbackMethod = "paymentInfo_TimeoutHandle",commandProperties = {
            @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "5000")
    })
    public String payment_timeout(@PathVariable("id") Integer id){
        return  paymentHystrixService.payment_timeout(id);
    }

    public String paymentInfo_TimeoutHandle(@PathVariable("id") Integer id){
        return  "80访问失败，对方支付系统繁忙，访问超时或者请检查自己";
    }
```





###### 4,测试

启动pay模块,order模块,

```java
http://localhost/consumer/payment/hystrix/timeout/2
```



**注意:,这里pay模块和order模块都开启了服务降级**

​			但是order这里,设置了1.5秒就降级,所以访问时,一定会降级

 

##### 4,重构:

**上面出现的问题:**
		1,降级方法与业务方法写在了一块,耦合度高

​		2.每个业务方法都写了一个降级方法,重复代码，代码膨胀

##### **解决重复代码的问题**:

**配置一个全局的降级方法,所有方法都可以走这个降级方法,至于某些特殊创建,再单独创建方法**

###### 1,创建一个全局方法，在80的controller的方法下

```java
 //这边是定义的全局fallback的方法
    public String payment_Global_FallbackMethod(){
        return "Global异常处理信息";
    }
```



###### 2,使用注解指定其为全局降级方法(默认降级方法)

```java
@RestController
@Slf4j
@DefaultProperties(defaultFallback = "payment_Global_FallbackMethod")
public class OrderHystrixController {}
```

###### 3,业务方法使用默认降级方法:

```java
 @HystrixCommand
    public String payment_timeout(@PathVariable("id") Integer id){
		int a = 10 / 0 ;
        return  paymentHystrixService.payment_timeout(id);
    }
```

###### 4,测试:

```java
http://localhost/consumer/payment/hystrix/timeout/2
```

![image-20201108155645121](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201108155645121.png)







##### 解决代码耦合度的问题:

修改order模块,这里开始,pay模块就不服务降级了,服务降级写在order模块即可

###### 1,PaymentHystrixService接口是远程调用pay模块的,我们这里创建一个类实现service接口,在实现类中统一处理异常 

###### PaymentFallbackService

```java
package com.atlgq.springcloud.service;

import org.springframework.stereotype.Component;

@Component
public class PaymentFallbackService implements  PaymentHystrixService {
    @Override
    public String payment_ok(Integer id) {
        return "--------PaymentFallbackService fall back payment_ok , (╥╯^╰╥)";
    }

    @Override
    public String payment_timeout(Integer id) {
        return "--------PaymentFallbackService fall back payment_timeout,(╥╯^╰╥)";
    }
}

```



###### 2,修改配置文件:添加:

```yml
feign:
    hystrix:
      enabled: true

```



###### 	3,让PaymentFallbackService的实现类生效:

加上注解

@FeignClient(value = "CLOUD-PROVIDER-HYSTRIX-PAYMENT",fallback = PaymentFallbackService.class)

```java
package com.atlgq.springcloud.service;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@Component
@FeignClient(value = "CLOUD-PROVIDER-HYSTRIX-PAYMENT",fallback = PaymentFallbackService.class)
public interface PaymentHystrixService {

    @GetMapping(value = "/payment/hystrix/ok/{id}")
    public String payment_ok(@PathVariable("id") Integer id);

    @GetMapping(value = "/payment/hystrix/timeout/{id}")
    public String payment_timeout(@PathVariable("id") Integer id);
}

```



```java
它的运行逻辑是:
	当请求过来,首先还是通过Feign远程调用pay模块对应的方法，但是如果pay模块报错,调用失败,那么就会调PaymentFallbackService类的，当前同名的方法,作为降级方法
```

###### 4,启动测试

http://localhost/consumer/payment/hystrix/timeout/2

启动order和pay正常访问--ok

==此时将pay服务关闭,order再次访问==

可以看到,并没有报500错误,而是降级访问==实现类==的同名方法

这样,即使服务器挂了,用户要不要一直等待,或者报错

问题:

​		**这样虽然解决了代码耦合度问题,但是又出现了过多重复代码的问题,每个方法都有一个降级方法**





### 使用服务熔断:

![Hystrix的34](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Hystrix的34.png)

**比如并发达到1000,我们就拒绝其他用户访问,在有用户访问,就访问降级方法**

![Hystrix的35](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Hystrix的35.png)

#### 1,修改前面的pay模块

##### **1,修改Payservice接口的实现方法类PaymentServiceImpl,添加服务熔断相关的方法:**

```java
   //================服务熔断================
    @HystrixCommand(fallbackMethod = "paymentCircuitBreak_fallback" ,commandProperties = {
            @HystrixProperty(name = "circuitBreaker.enabled",value = "true"),//是否开启断路器
            @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold",value = "10"),//请求次数
            @HystrixProperty(name = "circuitBreaker.sleepWindowInMilliseconds",value = "10000"),//时间窗口期
            @HystrixProperty(name = "circuitBreaker.errorThresholdPercentage",value = "60"),//失败率达到多少开启跳闸
    })
    public String paymentCircuitBreak(@PathVariable("id") Integer id){
        if(id < 0){
            throw new RuntimeException("**********id不能为负数");

        }
        String serialNumber = IdUtil.simpleUUID(); //这是采用cloud-api-commons里面的hutool工具包

        return Thread.currentThread().getName()+"\t" + "调用成功,流水号" + serialNumber;
    }
    public String paymentCircuitBreak_fallback(@PathVariable("id") Integer id){
        return "id 不能为负数，请稍候再试,(╥╯^╰╥)";
    }

```



这里属性整体意思是:
			10秒之内(窗口,会移动),如果并发==超过==10个,或者10个并发中,失败了6个,就开启熔断器

IdUtil是Hutool包下的类,这个Hutool就是整合了所有的常用方法,比如UUID,反射,IO流等工具方法什么的都整合了



```java
断路器的打开和关闭,是按照一下5步决定的
  	1,并发此时是否达到我们指定的阈值
  	2,错误百分比,比如我们配置了60%,那么如果并发请求中,10次有6次是失败的,就开启断路器
  	3,上面的条件符合,断路器改变状态为open(开启)
  	4,这个服务的断路器开启,所有请求无法访问
  	5,在我们的时间窗口期,期间,尝试让一些请求通过(半开状态),如果请求还是失败,证明断路器还是开启状态,服务没有恢复
  		如果请求成功了,证明服务已经恢复,断路器状态变为close关闭状态
```



##### 2,修改controller

添加一个测试方法;

```java
  //#################服务熔断##################
    @GetMapping(value = "/payment/circuit/{id}")
    public String paymentCircuitBreaker(@PathVariable("id") Integer id){
        String result = paymentService.paymentCircuitBreak(id);
        log.info("*********result = " + result);
        return result;

    }
```





##### 3,测试:

启动pay7001,7002,order8001模块

==多次访问,并且错误率超过60%:==

这是请求成功的

![image-20201108195620997](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201108195620997.png)

此时服务熔断,此时即使访问正确的也会报错:连续点击错误访问数据，然后再访问成功的数据

![image-20201108195726726](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201108195726726.png)

**但是,当过了几秒后,又恢复了**

​				因为在10秒窗口期内,它自己会尝试接收部分请求,发现服务可以正常调用,慢慢的当错误率低于60%,取消熔断



### Hystrix所有可配置的属性:

**全部在这个方法中记录,以成员变量的形式记录,**

​		以后需要什么属性,查看这个类即可，搜索

​		HystrixCommandProperties.class

```java
circuitBreakerEnabled
	->    circuitBreaker.enabled
circuitBreakerErrorThresholdPercentage
    ->circuitBreaker.errorThresholdPercentage
```



### 总结:

![Hystrix的42](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Hystrix的42.png)

**==当断路器开启后:==**

​	![Hystrix的44](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Hystrix的44.png)



**==其他参数:==**

![Hystrix的45](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Hystrix的45.png)

![Hystrix的46](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Hystrix的46.png)

![Hystrix的47](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Hystrix的47.png)

![Hystrix的48](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Hystrix的48.png)

![Hystrix的49](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Hystrix的49.png)





**熔断整体流程:**

```java
1请求进来,首先查询缓存,如果缓存有,直接返回
  	如果缓存没有,--->2
2,查看断路器是否开启,如果开启的,Hystrix直接将请求转发到降级返回,然后返回
  	如果断路器是关闭的,
				判断线程池等资源是否已经满了,如果已经满了
  					也会走降级方法
  			如果资源没有满,判断我们使用的什么类型的Hystrix,决定调用构造方法还是run方法
        然后处理请求
        然后Hystrix将本次请求的结果信息汇报给断路器,因为断路器此时可能是开启的
          			(因为断路器开启也是可以接收请求的)
        		断路器收到信息,判断是否符合开启或关闭断路器的条件,
				如果本次请求处理失败,又会进入降级方法
        如果处理成功,判断处理是否超时,如果超时了,也进入降级方法
        最后,没有超时,则本次请求处理成功,将结果返回给controller
         
 
```







### Hystrix服务监控:

#### HystrixDashboard

![Hystrix的51](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Hystrix的51.png)

#### 2,使用HystrixDashboard:

##### 1,创建项目:

名字: cloud-consumer-hystrix-dashboard9001

##### 2,pom文件

```xml
 <dependencies>
        <!--hystrix dashboard-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
        </dependency>

        <dependency><!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.atlgq.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
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
```



##### 3,配置文件

配置9001端口

##### 4,主启动类，添加@EnableHystrixDashboard注解

```java
@SpringBootApplication
@EnableHystrixDashboard
public class HystrixDashboardMain9001 {
    public static void main(String[] args) {
        SpringApplication.run(HystrixDashboardMain9001.class,args);
    }
}
```



##### 5,修改所有pay模块(8001,8002,8003...)，给需要监控的模块添加

**他们都添加一个pom依赖:**

```xml
<!--监控-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
```

之前的pom文件中都添加过了,==这个是springboot的监控组件==

##### 6,启动9001即可

​			访问: **localhost:9001/hystrix**

可以看到豪猪哥

##### 7,注意,此时仅仅是可以访问HystrixDashboard,并不代表已经监控了8001,8002

​							如果要监控,还需要配置:(8001为例)

==8001的主启动类添加:== @Bean

```java
public class PaymentHystrixMain8001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentHystrixMain8001.class,args);

    }
    @Bean
    public ServletRegistrationBean getServlet(){
        HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
        ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);
        registrationBean.setLoadOnStartup(1);
        registrationBean.addUrlMappings("/hystrix.stream");
        registrationBean.setName("HystrixMetricsStreamServlet");
        return registrationBean;

    }
}
```

**其他8002,8003都是一样的**

##### 8,到此,可以启动服务

启动7001,8001,9001

测试:看是否访问通过

```java
http://localhost:8001/payment/circuit/2
http://localhost:8001/payment/circuit/-2
```



**然后在web界面,指定9001要监控8001:**

```java
http://localhost:8001/hystrix.stream
```

![image-20201108211010728](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201108211010728.png)



![image-20201108211100121](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201108211100121.png)



### ########################2020.11.08#########################





# 5,服务网关:

zuul停更了,

## 13,GateWay

![gateway的1](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的1.png)

![gateway的2](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的2.png)

**gateway之所以性能号,因为底层使用WebFlux,而webFlux底层使用netty通信(NIO)**



![gateway的3](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的3.png)



### GateWay的特性:

![gateway的4](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的4.png)



### GateWay与zuul的区别:

![gateway的5](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的5.png)



### zuul1.x的模型:

![gateway的6](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的6.png)

![gateway的7](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的7.png)



### 什么是webflux:

**是一个非阻塞的web框架,类似springmvc这样的**

![gateway的8](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的8.png)

d

### GateWay的一些概念:

#### 1,路由:



就是根据某些规则,将请求发送到指定服务上

![gateway的9](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的9.png)

#### 2,断言:

![gateway的10](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的10.png)

就是判断,如果符合条件就是xxxx,反之yyyy



#### 3,过滤:

![gateway的11](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的11.png)

​	**路由前后,过滤请求**





### GateWay的工作原理,GateWay的核心是路由转发+执行过滤链

![gateway的12](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的12.png)

![gateway的13](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的13.png)





### 使用GateWay:

想要新建一个GateWay的项目

名字: 	cloud-gateway-gateway9527

#### 1,pom

```xml
  <dependencies>
        <dependency><!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.atlgq.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
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

```



#### 2,配置文件，(一定要注意格式，我在这里- Path 中间缺少空格就报错了)

```yml
server:
  port: 9527
spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      routes:
        - id: payment_routh #路由的id，没有固定的要求，但是要唯一
          uri: http://localhost:8001 #匹配后提供服务的地址
          predicates:
            - Path=/payment/get/** #断言，路径相匹配的进行路由

        - id: payment_routh2
            #路由的id，没有固定的要求，但是要唯一
          uri: http://localhost:8001
            #匹配后提供服务的地址
          predicates:
              - Path=/payment/lb/**
            #断言，路径相匹配的进行路



eureka:
  instance:
    hostname: cloud-gateway-service
  client:
    register-with-eureka: true
      #true表示向服务总心注册自己
    fetch-registry: true
      #true表示从eurekaServer注册中心抓取已有信息，默认为true，单节点是无所谓的，集群必须设置为true的话，才能够配合ribbon使用负载均衡
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
        #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址


```



#### 3,主启动类

```
@SpringBootApplication
@EnableEurekaClient
```

#### 4,针对pay模块,设置路由:在yml文件里面修改的，一定要注意缩进和空格

```yml

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      routes:
        - id: payment_routh #路由的id，没有固定的要求，但是要唯一
          uri: http://localhost:8001 #匹配后提供服务的地址
          predicates:
            - Path=/payment/get/** #断言，路径相匹配的进行路由

        - id: payment_routh2
            #路由的id，没有固定的要求，但是要唯一
          uri: http://localhost:8001
            #匹配后提供服务的地址
          predicates:
              - Path=/payment/lb/**
            #断言，路径相匹配的进行路
```

**==修改GateWay模块(9527)的配置文件==:**

这里表示,

​			当访问localhost:9527/payment/get/1时,     

​			路由到localhost:8001/payment/get/1



#### 5,开始测试

**启动7001，7002,8001,9527**

```java
如果启动GateWay报错
  	可能是GateWay模块引入了web和监控的starter依赖,需要移除，因为gateway是自带web模块的
```

访问:

​		localhost:9527/payment/get/1

![image-20201109095707969](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201109095707969.png)





#### 6,GateWay的网关配置,

​		**GateWay的网关配置,除了支持配置文件,还支持硬编码方式**

#### 7使用硬编码配置GateWay:

##### 创建配置类:

```java
@Configuration
public class GateWayConfig {
    /**
     * 配置一个id为route-name的路由规则，
     * 当访问地址http://localhost:9527/guonei时会自动转发到地址：http://news.baidu.com/guonei
     * @param routeLocatorBuilder
     * @return
     */
    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder routeLocatorBuilder){
        RouteLocatorBuilder.Builder routes = routeLocatorBuilder.routes();

        routes.route("path_route_atguigu",
                r -> r.path("/guonei")
                        .uri("http://news.baidu.com/guonei")).build();
        return routes.build();
    }
}
```



#### 8,然后重启服务即可

 



### 重构:

上面的配置虽然首先了网关,但是是在配置文件中写死了要路由的地址

现在需要修改,不指定地址,而是根据微服务名字进行路由,我们可以在注册中心获取某组微服务的地址

需要:

​		1个eureka,2个pay模块

#### 修改GateWay模块的配置文件:

![gateway的21](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的21.png)

1、修改yml文件，增加

gateway:
      discovery:
        locator:
          enabled: true  #开启从注册中心动态创建路由的功能，利用微服务名称进行路由

把uri换成微服务名称

````yml
spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true  #开启从注册中心动态创建路由的功能，利用微服务名称进行路由
      routes:
        - id: payment_routh #路由的id，没有固定的要求，但是要唯一
#          uri: http://localhost:8001 #匹配后提供服务的地址
          uri: lb://CLOUD-PAYMENT-SERVICE
          predicates:
            - Path=/payment/get/** #断言，路径相匹配的进行路由

        - id: payment_routh2
            #路由的id，没有固定的要求，但是要唯一
#          uri: http://localhost:8001  #匹配后提供服务的地址
          uri: lb://CLOUD-PAYMENT-SERVICE
          predicates:
              - Path=/payment/lb/**
            #断言，路径相匹配的进行路

````



#### 然后就可以启动微服务.测试







### Pridicate断言:

![image-20201109102948371](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201109102948371.png)

**我们之前在配置文件中配置了断言:**



**这个断言表示,如果外部访问路径是指定路径,就路由到指定微服务上**

可以看到,这里有一个Path,这个是断言的一种,==断言的类型==:

时间可以通过java获得

```java
public class Main
{
    public static void main(String[] args) {
        ZonedDateTime time = ZonedDateTime.now();
        System.out.println(time);
        //2020-11-09T11:00:35.773+08:00[Asia/Shanghai]
    }
}

```



```java
After:
		可以指定,只有在指定时间后,才可以路由到指定微服务
```

```yml
 predicates:
              - Path=/payment/lb/**
              - After=2020-11-09T11:00:35.773+08:00[Asia/Shanghai]
```



​				这里表示,只有在==2020年的2月21的15点51分37秒==之后,访问==才可以路由==

​				在此之前的访问,都会报404



```java
before:
		与after类似,他说在指定时间之前的才可以访问
between:
		需要指定两个时间,在他们之间的时间才可以访问
```

```yml
#              - After=2020-11-09T11:00:35.773+08:00[Asia/Shanghai]
#              - Before=2020-11-09T10:00:35.773+08:00[Asia/Shanghai]
#              - Between=2020-11-09T11:00:35.773+08:00[Asia/Shanghai],2020-11-09T11:07:35.773+08:00[Asia/Shanghai]
```



```java
cookie:
		只有包含某些指定cookie(key,value),的请求才可以路由
```

![image-20201109111051316](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201109111051316.png)

```java
Header:
		只有包含指定请求头的请求,才可以路由
curl http://localhost:9527/payment/lb -H "X-Request-Id:123"
```

```java
host:
		只有指定主机的才可以访问,
		比如我们当前的网站的域名是www.aa.com
    那么这里就可以设置,只有用户是www.aa.com的请求,才进行路由
```

可以看到,如果带了域名访问,就可以,但是直接访问ip地址.就报错了

```java
method:
		只有指定请求才可以路由,比如get请求...
```

```java
path:
		只有访问指定路径,才进行路由
     比如访问,/abc才路由
```

```java
Query:
		必须带有请求参数才可以访问
```

```yml
#              - After=2020-11-09T11:00:35.773+08:00[Asia/Shanghai]
#              - Before=2020-11-09T10:00:35.773+08:00[Asia/Shanghai]
#              - Between=2020-11-09T11:00:35.773+08:00[Asia/Shanghai],2020-11-09T11:07:35.773+08:00[Asia/Shanghai]
#              - Cookie=username,zzyy
#              - Header=X-Request-Id, \d+ #请求头必须包含这个，而且为正数
#              - Method=GET
#              - Query=username, \d+ #传入必须要有username，而且为整数
```









### Filter过滤器:

![gateway的41](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的41.png)



#### 生命周期:

**在请求进入路由之前,和处理请求完成,再次到达路由之前**



#### 种类:

![gateway的42](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的42.png)

GateWayFilter,单一的过滤器

**与断言类似,比如闲置,请求头,只有特定的请求头才放行,反之就过滤**:

![gateway的43](D:\Desktop\studyDocument\springcloud下载的笔记\图片\gateway的43.png)



GlobalFilter,全局过滤器:





#### **自定义过滤器:**

实现两个接口

```java
@Component
@Slf4j
public class MyLogGatewayFilter implements GlobalFilter, Ordered
{
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        log.info("*************************this is MyLogGatewayFilter,date = " + new Date());
        String uname = exchange.getRequest().getQueryParams().getFirst("uname");
        if(uname == null){
            log.info("the uname is null (╥╯^╰╥)");
            //访问不被接受
            exchange.getResponse().setStatusCode(HttpStatus.NOT_ACCEPTABLE);
            return exchange.getResponse().setComplete();
        }
        //过滤链 chain 通过exchange这条数据
        return chain.filter(exchange);
    }

    @Override
    public int getOrder() {
        //这是决定过滤器的顺序的，数值越小，等级越高
        return 0;
    }
}

```



​	**然后启动服务,即可,因为过滤器通过@COmponet已经加入到容器了**

![image-20201109115446642](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201109115446642.png)

![image-20201109115457348](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201109115457348.png)









# 6,服务配置:

## Spring Config分布式配置中心:

==微服务面临的问题==

```java
可以看到,每个微服务都需要一个配置文件,并且,如果有几个微服务都需要连接数据库
		那么就需要配4次数据库相关配置,并且当数据库发生改动,那么需要同时修改4个微服务的配置文件才可以
```

所以有了springconfig配置中心

![springconfig的1](D:\Desktop\studyDocument\springcloud下载的笔记\图片\springconfig的1.png)

![springconfig的2](D:\Desktop\studyDocument\springcloud下载的笔记\图片\springconfig的2.png)

![springconfig的3](D:\Desktop\studyDocument\springcloud下载的笔记\图片\springconfig的3.png)

![springconfig的4](D:\Desktop\studyDocument\springcloud下载的笔记\图片\springconfig的4.png)





### 使用配置中心:

#### 0,使用github作为配置中心的仓库:

配置仓库的时候切记名字起为master

**初始化git环境:**

```java
https://www.cnblogs.com/lyr2015/p/6730476.html
根据这篇文章去配置git,git下载文件在QQ百度云
把git上面的文件克隆到本地命令，springcloud-config文件下的文件
	git clone git@github.com:linguiquan923/springcloud-config.git
```

#### 1,新建config模块

名字:   cloud-config-center-3344

#### 2,pom

```xml
  <dependencies>
        <!--config server-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
        </dependency>
        <!--eureka client-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>


        <dependency><!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.atlgq.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
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
```



#### 3,配置文件

```yml
server:
  port: 3344
spring:
  application:
    name: cloud-config-center #注册进Eureka服务器的微服务名
  cloud:
    config:
      server:
        git:
           uri: https://github.com/linguiquan923/springcloud-config.git #Github上的仓库名称
          #搜索目录
          search-paths:
            - springcloud-config
      #读取分支，指定是master还是别的分支
      label: master

#服务器注册进去到Eureka
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
```



#### 4,主启动类

```java
@SpringBootApplication
@EnableConfigServer
public class ConfigCenterMain3344 {
    public static void main(String[] args) {
        SpringApplication.run(ConfigCenterMain3344.class,args);
    }
}

```



#### 5,修改hosts:

```java
127.0.0.1       config-3344.com
```

#### 6,配置完成

测试,3344是否可以从github上获取配置

启动3344	(要先启动eureka)7001,7002

```java
config-3344.com:3344/master/config-dev.yml
```



它实际上就是,读取到配置文件中的GitHub的地址,然后拼接上/master/config-dev.yml

#### 7,读取配置文件的规则:

![springconfig的10](D:\Desktop\studyDocument\springcloud下载的笔记\图片\springconfig的10.png)



==2,==

![springconfig的11](D:\Desktop\studyDocument\springcloud下载的笔记\图片\springconfig的11.png)

**这里默认会读取master分支,因为我们配置文件中配置了**



==3==



注意,这个方式读取到的配置是==json格式==的

**所有规则:**

![springconfig的14](D:\Desktop\studyDocument\springcloud下载的笔记\图片\springconfig的14.png)



### 2,创建配置中心客户端:

#### 1,创建config客户端项目

名字: 	cloud-config-client-3355

#### 2,pom

```xml
<dependencies>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
        <dependency><!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.atlgq.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--监控-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--eureka client-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
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
```



#### 3,配置文件

注意这个配置文件就不是application.yml,application是用户级别的配置文件，而是bootstrap.yml，boostrap是系统级别的配置文件

这个配置文件的作用是,先到配置中心加载配置,然后加载到application.yml中

```yml
#bootstrap.yml文件是系统级别的，比application的级别高，所以会先加载这个文件里里面的信息
server:
  port: 3355

spring:
  application:
    name: config-client
  cloud:
    config:
      label: master #所要扫描的分支
      name: config #配置文件的名字
      profile: dev #读取名称的后缀
      #上面三个综合，会拼接成在master下的config-dev.yml，配置文件被读取http://config-3344.com:3344/master/config-dev.yml
      uri: http://localhost:3344 #配置中心的地址

eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
```





#### 4,主启动类:

```java
@SpringBootApplication
@EnableEurekaClient
public class ConfigClientMain3355 {
    public static void main(String[] args) {
        SpringApplication.run(ConfigClientMain3355.class,args);
    }
}
```



#### 5,controller类

就是上面提到的,以rest风格将配置对外暴露

```java
public class ConfigClientContrller {

    @Value("${config.info}")
    private String configInfo;

    @GetMapping(value = "/configInfo")
    private String configInfo(){
        return configInfo;
    }
}

```





**如果客户端运行正常,就会读取到github上配置文件的,config.info下的配置**

#### 6,测试:

启动3344,3355，7001,7002

​	访问3355的  /configInfo

```java
localhost:3355/configInfo
```

修改了yml文件下的信息，换成获取别的仓库的文件，照常访问![image-20201109161910378](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201109161910378.png)





#### 7,问题::

```java
上面3355确实获取到了配置文件,但是如果此时配置文件修改了,3355是获取不到的
		3344可以实时获取到最新配置文件,但是3355却获取不到
  	除非重启服务
```

#### **8,实现动态刷新:**

##### 1,修改3355,添加一个pom依赖:添加一个监控的依赖，（但是我们的文件已经添加了）

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
```



##### 2,修改配置文件,添加一个配置:暴露端口

```yml
#暴露监控端点
management:
  endpoints:
    web:
      exposure:
        include: "*"
```



##### 3,修改controller:

加一个注解@RefreshScope，把这个注解直接配置到controller上的时候出错了，因为controller和@RefreshScope一起使用的话会导致controller获取不到值。

所以我们可以专门写一个配置类来获取这个git 上面的configInfo信息

```java
@Component
@RefreshScope
public class ConfigData {
    @Value("${config.info}")
    private String configInfo;

    public String getConfigInfo(){
        return this.configInfo;
    }
    public void setConfigInfo(String configInfo){
        this.configInfo = configInfo;
    }
}
```

然后在controller引入使用即可

```java
@RestController
@Slf4j
public class ConfigClientContrller {

   @Resource
   private ConfigData configInfo;

    @GetMapping(value = "/configInfo")
    private String configInfo(){
        return "this is " + configInfo.getConfigInfo();
    }
}
```



##### 4,此时重启服务，利用curl返送POST请求

**此时3355还不可以动态获取**

因为此时,还需要==外部==发送post请求通知3355，用管理员打开cmd才可以使用curl否则的话会显示curl不是内部命令

```java
curl -X POST "http://localhost:3355/actuator/refresh"
```

**此时在刷新3355,发现可以获取到最新的配置文件了,这就实现了动态获取配置文件,因为3355并没有重启**



具体流程就是:

​			我们启动好服务后

​			运维人员,修改了配置文件,然后发送一个post请求通知3355

​			3355就可以获取最新配置文件





**问题:**

​		如果有多个客户端怎么办(3355,3356,3357.....)

​						虽然可以使用shell脚本,循环刷新

​		但是,可不可以使用广播,一次通知??

​					这些springconfig做不到,需要使用springcloud Bus消息总线







# 消息总线:

## SpringCloud Bus:

![springconfig的26](D:\Desktop\studyDocument\springcloud下载的笔记\图片\springconfig的26.png)



![springconfig的27](D:\Desktop\studyDocument\springcloud下载的笔记\图片\springconfig的27.png)

![springconfig的31](D:\Desktop\studyDocument\springcloud下载的笔记\图片\springconfig的31.png)







注意,这里年张图片,就代表两种广播方式

​			图1:		**它是Bus直接通知给其中一个客户端,由这个客户端开始蔓延,传播给其他所有客户端**

​			图2:		它**是通知给配置中心的服务端,有服务端广播给所有客户端**





**为什么被称为总线?**

```java
就是通过消息队列达到广播的效果
  		我们要广播每个消息时,主要放到某个topic中,所有监听的节点都可以获取到
```





### 使用Bus:

#### 1,配置rabbitmq环境:

下载好这两个软件并且安装好

![image-20201109172029724](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201109172029724.png)

通过管理员打开cmd，进入rabbitmq的sbin，在里面执行

```java
rabbitmq_server-3.7.14\sbin>rabbitmq-plugins enable rabbitmq_management
```

![image-20201109172125906](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201109172125906.png)

在图形化界面启动即可

```java
http://localhost:15672
```



#### **2,之前只有一个配置中心客户端,这里在创建一个**

​		==**复制3355即可,创建为3366**==

全部复制3355的即可

#### 2,使用Bus实现全局广播

**Bus广播有两种方式:**

​		==就是上面两个图片的两种方式==

![springconfig的32](D:\Desktop\studyDocument\springcloud下载的笔记\图片\springconfig的32.png)

**这两种方式,第二种跟合适,因为:**

​			==第一种的缺点:==

![springconfig的33](D:\Desktop\studyDocument\springcloud下载的笔记\图片\springconfig的33.png)

#### **配置第二种方式:**

##### **1,配置3344(配置中心服务端):**

###### 1,修改配置文件:注意空格缩进

```xml
#rabbitmp的默认设置
rabbitmq:
    host: 5672
    username: guest
    password: guest


##rabbitmp的相关配置，暴露bus刷新配置的端点
management:
  endpoints:
    web:
      exposure:
        include: 'bus-refresh'
```



###### 2,添加pom

```xml
  <!-- 添加消息总线RabbitMQ支持 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-bus-amqp</artifactId>
        </dependency>
```



**springboot的监控组件,和消息总线**

##### 2,修改3355(配置中心的客户端)

###### 1,pom:

```xml
  <!-- 添加消息总线RabbitMQ支持 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-bus-amqp</artifactId>
        </dependency>
<!--监控-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
```



###### 2,配置文件:应该也要添加暴露点，但是前面已经添加过了，这是添加过了的

```yml
#暴露监控端点
management:
  endpoints:
    web:
      exposure:
        include: "*"
```



==注意配置文件的名字,要改为bootstrap.yml==

```yml
#rabbitmp的默认设置
rabbitmq:
    host: 5672
    username: guest
    password: guest
```





##### 3,修改3366(也是配置中心的客户端)

​			修改与3355是一模一样的



##### 4,测试

启动7001,7002，3344,3355,3366

此时修改GitHub上的配置文件

==此时只需要刷新3344,即可让3355,3366动态获取最新的配置文件==

```java
用管理员打开cmd，向3344发送post请求
    curl -X POST "http://localhost:3344/actuator/bus-refresh"
```







其原理就是:

![Bus的7](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Bus的7.png)

**所有客户端都监听了一个rabbitMq的topic,我们将信息放入这个topic,所有客户端都可以送到,从而实时更新**









#### 配置定点通知

​		就是只通知部分服务,比如只通知3355,不通知3366

![Bus的8](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Bus的8.png)

![Bus的9](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Bus的9.png)



**只通知3355**

```java
curl -X POST "http://localhost:3344/actuator/bus-refresh/config-client:3355"
```

config-client:3355其实是微服务名称和端口号的结合

![image-20201109214612365](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201109214612365.png)

**可以看到,实际上就是通过==微服务的名称+端口号==进行指定**







#### ########################2020.11.09#######################

# **可能存在版本问题，无法运行**

# 8,消息驱动:

## Spring Cloud Stream:

```java
现在一个很项目可能分为三部分:
			前端--->后端---->大数据
			而后端开发使用消息中间件,可能会使用RabbitMq
			而大数据开发,一般都是使用Kafka,
			那么一个项目中有多个消息中间件,对于程序员,因为人员都不友好
```

而Spring Cloud Stream就类似jpa,屏蔽底层消息中间件的差异,程序员主要操作Spring Cloud Stream即可

​			不需要管底层是kafka还是rabbitMq

![SpringCloudStream的1](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的1.png)

### ==什么是Spring Cloud Stream==

![SpringCloudStream的2](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的2.png)

![SpringCloudStream的3](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的3.png)

![SpringCloudStream的4](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的4.png)



![SpringCloudStream的5](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的5.png)





### ==**Spring Cloud Stream是怎么屏蔽底层差异的?**==

![SpringCloudStream的6](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的6.png)





**绑定器:**

![SpringCloudStream的7](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的7.png)

![SpringCloudStream的8](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的8.png)

![SpringCloudStream的9](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的9.png)

### **Spring Cloud Streamd 通信模式:**



![SpringCloudStream的10](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的10.png)

![SpringCloudStream的11](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的11.png)

### Spring Cloud Stream的业务流程:

![SpringCloudStream的12](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的12.png)

![SpringCloudStream的13](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的13.png)

![SpringCloudStream的14](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的14.png)



```java
类似flume中的channel,source,sink  估计是借鉴(抄袭)的
  	source用于获取数据(要发送到mq的数据)
  	channel类似SpringCloudStream中的中间件,用于存放source接收到的数据,或者是存放binder拉取的数据	
```







### 常用注解和api:

![SpringCloudStream的15](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的15.png)





### 使用SpringCloudStream:

需要创建三个项目,一个生产者,两个消费者



### 1,创建生产者

```jav
cloud-stream-rabbitmp-provider8801
```

#### 1,pom



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

    <artifactId>cloud-stream-rabbitmp-provider8801</artifactId>

    <dependencies>
        <!--引入springcloud stream - rabbitmp-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
            <version>3.0.1.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
        <dependency><!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.atlgq.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--监控-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--eureka client-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
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



#### 2,配置文件

```yml
server:
  port: 8801

spring:
  application:
    name: stream-provider
  cloud:
    stream:

        binders: # 在此处配置要绑定的rabbitmq的服务信息；
          defaultRabbit: # 表示定义的名称，用于于binding整合
            type: rabbit # 消息组件类型
            environment: # 设置rabbitmq的相关的环境配置
              spring:
                rabbitmq:
                  host: localhost
                  port: 5672
                  username: guests
                  password: guest
        bindings: # 服务的整合处理
          output: # 这个名字是一个通道的名称
            destination: studyExchange # 表示要使用的Exchange名称定义
            content-type: application/json # 设置消息类型，本次为json，文本则设置“text/plain”
            binder: defaultRabbit  # 设置要绑定的消息服务的具体设置




eureka:
  client:

    register-with-eureka: true
    #true表示向服务总心注册自己,默认值是true
    fetch-registry: true
    #true表示从eurekaServer注册中心抓取已有信息，默认为true，单节点是无所谓的，集群必须设置为true的话，才能够配合ribbon使用负载均衡
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
      #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址

  instance:
    lease-expiration-duration-in-seconds: 5 #如果现在超过了5秒钟
    lease-renewal-interval-in-seconds: 2  #设置心跳时间，默认是30秒
    instance-id: send-8801.com #在信息列表显示主机名称
    prefer-ip-address: true #访问路径变ip地址s
```



#### 3,主启动类

```java
package com.atlgq.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class StreamMQMain8801 {
    public static void main(String[] args) {
        SpringApplication.run(StreamMQMain8801.class,args);
    }
}

```



#### 4,service和实现类

在这里我引错了Source.class的包，导致我一直出错，浪费了一个早上！！

正常来说，service层是调用dao层的，但是在这里我们是调用@EnableBinding(Source.class) 来发送消息，首先自定义一个接口，在实现类实现以下内容，service定义发送消息

```java
package com.atlgq.springcloud.service.impl;

import com.atlgq.springcloud.service.IMessageProvider;
import org.springframework.cloud.stream.annotation.EnableBinding;
import org.springframework.cloud.stream.messaging.Source;
import org.springframework.integration.support.MessageBuilder;
import org.springframework.messaging.MessageChannel;

import javax.annotation.Resource;
import java.util.UUID;

@EnableBinding(Source.class)
public class MessageProviderImpl implements IMessageProvider {

    @Resource
    private MessageChannel output;

    @Override
    public String send() {
        String serial = UUID.randomUUID().toString();
        output.send(MessageBuilder.withPayload(serial).build());

        System.out.println("this is =========   " + serial);

        return null;
    }
}


```



**这里,就会调用send方法,将消息发送给channel,**

​				**然后channel将消费发送给binder,然后发送到rabbitmq中**

#### 5,controller

```java
package com.atlgq.springcloud.controller;

import com.atlgq.springcloud.service.IMessageProvider;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;

@RestController
public class SendMessageController {

    @Resource
    private IMessageProvider messageProvider;

    @GetMapping(value = "/sendMessage")
    public String sendMessage(){
        return messageProvider.send();
    }

}

```



#### 6,可以测试

**启动rabbitmq**

**启动7001,7002,8801**

​		确定8801后,会在rabbitmq中创建一个Exchange,就是我们配置文件中配置的exchange

**访问localhost:8801/sendMessage**







### 创建消费者:

```jav
cloud-stream-tabbitmp-comsumer8802
```



#### 1,pom文件



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

    <artifactId>cloud-stream-rabbitmp-provider8801</artifactId>

    <dependencies>
        <!--引入springcloud stream - rabbitmp-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-stream-rabbit</artifactId>
            <version>3.0.1.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
        <dependency><!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.atlgq.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--监控-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--eureka client-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
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



#### 2,配置文件

==**这里排版一点问题**==

**==input==就表示,当前服务是一个消费者,需要消费消息,下面就是指定消费哪个Exchange中的消息**



```yml
server:
  port: 8802

spring:
  application:
    name: cloud-stream-consumer
  cloud:
    stream:
      rabbit:
        binders: # 在此处配置要绑定的rabbitmq的服务信息；
          defaultRabbit: # 表示定义的名称，用于于binding整合
            type: rabbit # 消息组件类型
            environment: # 设置rabbitmq的相关的环境配置
              spring:
                rabbitmq:
                  host: localhost
                  port: 5672
                  username: guest
                  password: guest
        bindings: # 服务的整合处理
          input: # 这个名字是一个通道的名称
            destination: studyExchange # 表示要使用的Exchange名称定义
            content-type: application/json # 设置消息类型，本次为json，文本则设置“text/plain”
            binder: defaultRabbit  # 设置要绑定的消息服务的具体设置

eureka:
  client:

    register-with-eureka: true
    #true表示向服务总心注册自己,默认值是true
    fetch-registry: true
    #true表示从eurekaServer注册中心抓取已有信息，默认为true，单节点是无所谓的，集群必须设置为true的话，才能够配合ribbon使用负载均衡
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
      #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址

  instance:
    lease-expiration-duration-in-seconds: 5 #如果现在超过了5秒钟
    lease-renewal-interval-in-seconds: 2  #设置心跳时间，默认是30秒
    instance-id: receive-8802.com #在信息列表显示主机名称
    prefer-ip-address: true #访问路径变ip地址
```



#### 3,主启动类

![SpringCloudStream的25](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的25.png)

#### 4,业务类(消费数据)

![SpringCloudStream的26](D:\Desktop\studyDocument\springcloud下载的笔记\图片\SpringCloudStream的26.png)

**生产者发送消息时,使用send方法发送,send方法发送的是一个个Message,里面封装了数据**

#### 5,测试:

启动7001.7002.8801.8802

**此时使用生产者生产消息**

![](.\图片\SpringCloudStream的27.png)

==可以看到,消费者已经接收到消息了==





### 创建消费者2

创建8803,

==与8802创建一模一样,就不写了==

**创建8803主要是为了演示重复消费等问题**

...

....

...





### ==重复消费问题:==

此时启动7001.8801.8802.8803

此时生产者生产一条消息

但是此时查询消费者,发现8802,8803==都消费到了同一条数据==

![](.\图片\SpringCloudStream的28.png)

![](.\图片\SpringCloudStream的29.png)

#### 1,自定义分组

**修改8802,8803的配置文件**

![](.\图片\SpringCloudStream的30.png)

![](.\图片\SpringCloudStream的31 - 副本.png)

**现在将8802,8803都分到了A组**

然后去重启02,03

**然后此时生产者生产两条消息**

![](.\图片\SpringCloudStream的33.png)

![](.\图片\SpringCloudStream的34.png)

![](.\图片\SpringCloudStream的35.png)

**可以看到,每人只消费了一条消息,并且没有重复消费**







### 持久化问题:

就是当服务挂了,怎么消费没有消费的数据??



这里,先将8802移除A组,

​		然后将02,03服务关闭

此时生产者开启,发送3条消息

​		此时重启02,03

​		可以看到,当02退出A组后,它就获取不到在它宕机的时间段内的数据

​		但是03重启后,直接获取到了宕机期间它没有消费的数据,并且消费了

总结:
		也就是,当我们没有配置分组时,会出现消息漏消费的问题

​		而配置分组后,我们可以自动获取未消费的数据





# **可能存在问题，无法运行**









# 9,链路追踪:

## Spring Cloud Sleuth

**sleuth要解决的问题:**

![sleuth的1](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sleuth的1.png)

**而来sleuth就是用于追踪每个请求的整体链路**

![sleuth的2](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sleuth的2.png)



### 使用sleuth:

#### 1,安装zipkin:

![sleuth的3](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sleuth的3.png)

**运行jar包**

在安装目录下打开cmd

```java
java -jar zipkin-server-2.12.9-exec.jar
```



**然后就可以访问web界面,  默认zipkin监听的端口是9411**

​			localhost:9411/zipkin/

**一条链路完整图片:**

![sleuth的5](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sleuth的5.png)

**精简版:**

![sleuth的5](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sleuth的5.png)

**可以看到,类似链表的形式**







#### 2,使用sleuth:

不需要额外创建项目,使用之前的8001和order的80即可



##### 1,修改cloud-provider-payment8001和cloud-consumer-fegin-order80

**引入pom:**

```xml
 <!--包含了sleuth + zipkin-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zipkin</artifactId>
        </dependency>
```



这个包虽然叫zipkin但是,里面包含了zpikin与sleuth

**修改配置文件:**增加下面的zipkin

```yml
spring:
  application:
    name: cloud-payment-service #服务名称
    
    
  zipkin:
    base-url: http://localhost:9411
  sleuth:
    sampler:
      #这是采样率，介于0~1之间，1则表示全部采集
      probability: 1

```

##### 2,修改cloud-consumer-fegin-order80

**添加pom**

与上面是一样的

```xml
 <!--包含了sleuth + zipkin-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zipkin</artifactId>
        </dependency>
```



**添加配置**:

与上面也是一样的

```yml
spring:
  application:
    name: cloud-payment-service #服务名称
    
    
  zipkin:
    base-url: http://localhost:9411
  sleuth:
    sampler:
      #这是采样率，介于0~1之间，1则表示全部采集
      probability: 1

```



##### 3,测试:

启动7001,7002,8001,80,9411







# **#中级篇分割线#######################**





# 10,Spring CloudAlibaba:

**之所以有Spring CloudAlibaba,是因为Spring Cloud Netflix项目进入维护模式**

​		**也就是,就不是不更新了,不会开发新组件了**

​		**所以,某些组件都有代替版了,比如Ribbon由Loadbalancer代替,等等**

==支持的功能==

![Alibaba的1](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Alibaba的1.png)

几乎可以将之前的Spring Cloud代替



==具体组件==:

![Alibaba的2](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Alibaba的2.png)



![image-20201110162244100](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201110162244100.png)



## Nacos:

**服务注册和配置中心的组合**

​			Nacos=erueka+config+bus

### 安装Nacos:

需要java8  和 Mavne

**1,到github上下载安装包**,Nacos官方文档

```java
Nacos下载地址，手册
https://nacos.io/zh-cn/index.html
Nacos官方文档
https://spring-cloud-alibaba-group.github.io/github-pages/greenwich/spring-cloud-alibaba.html#_spring_cloud_alibaba_nacos_discovery
```

​		解压安装包

**2,启动Nacos**

​		在bin下,进入cmd

​		./startup.cmd

​	如果安装失败，那么尝试换一下jdk的版本，我就是版本出问题

**3,访问Nacos**

​		Nacos默认监听8848

​		localhost:8848/nacos

​		账号密码:默认都是nacos





### 使用Nacos:

新建pay模块

​		**现在不需要额外的服务注册模块了,Nacos单独启动了**

名字: cloudalibaba-provider-payment9001

#### 1,pom

父项目管理alibaba的依赖:,添加

```xml
 <!--spring cloud 阿里巴巴-->
      <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-alibaba-dependencies</artifactId>
        <version>2.1.0.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
```







==9001的pom==:

​			另外一个文件.....

```xml


    <dependencies>
        <!-- SpringCloud ailibaba nacos-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!--springcloud整合web组件-->
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


```





#### 2,配置文件，官网拿

```yml


server:
  port: 8081
spring:
  application:
    name: nacos-payment-provider
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
management:
  endpoints:
    web:
      exposure:
        include: '*'

```



#### 3,启动类

```java


@SpringBootApplication
@EnableDiscoveryClient
public class NacosProviderDemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(NacosProviderDemoApplication.class, args);
    }

}
```



#### 4,controller:

```java


@RestController
@Slf4j
public class NacosPaymentController {

    @Value("${server.port}")
    private String serverPort;

    @GetMapping(value = "/payment/lb")
    public String getServerPort(){
        return "this is " + serverPort;
    }
}

```



#### 5,测试

启动9001

然后查看Nacos的web界面,可以看到9001已经注册成功

```java
http://localhost:8081/payment/lb
```



### 创建其他Pay模块

​		额外在创建

cloudalibaba-provider-payment9002,

​		直接复制上面的即可

### 创建order模块

名字:  cloudalibaba-consumer-order83

#### 1,pom

**为什么Nacos支持负载均衡?**

​				Nacos直接集成了Ribon,所以有负载均衡

```xml


    <dependencies>
        <!-- SpringCloud ailibaba nacos-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!--springcloud整合web组件-->
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


```



#### 2,配置文件

```yml
server:
  port: 83
spring:
  application:
    name: nacos-order-consumer
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848

#这是给controller提供的地址
server-url:
  nacos-user-service: http://nacos-payment-provider

```



**这个server-url的作用是,我们在controller,需要使用RestTempalte远程调用9001,**

​		**这里是指定9001的地址**

#### 3,主启动类

```java

@SpringBootApplication
@EnableDiscoveryClient
public class OrderNacosMain83 {
    public static void main(String[] args) {
        SpringApplication.run(OrderNacosMain83.class,args);
    }
}

```





#### 4,编写配置类

​	==因为Naocs要使用Ribbon进行负载均衡,那么就需要使用RestTemplate==

```java
@Configuration
public class ApplicationContextConfig {

    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
```



#### 5,controller:

```java
@RestController
@Slf4j
public class OrderNacosController {
    @Value("${server-url.nacos-user-service}")
    private String serverUrl;

    @Resource
    private RestTemplate restTemplate;

    @GetMapping(value = "/consumer/payment/lb")
    public String paymentInfo(){
        log.info("this is order83 ");
        return restTemplate.getForObject(serverUrl+"/payment/lb",String.class);
    }
}

```



#### 6,测试

```java
http://localhost:83/consumer/payment/lb
```



启动83,7001,7002访问9001,9002,可以看到,实现了负载均衡





### Nacos与其他服务注册的对比

Nacos它既可以支持CP,也可以支持AP,可以切换

![Alibaba的12](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Alibaba的12.png)

![Alibaba的13](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Alibaba的13.png)

==下面这个curl命令,就是切换模式==





### 使用Nacos作为配置中心:

![Alibaba的14](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Alibaba的14.png)



**==需要创建配置中心的客户端模块==**

cloudalibaba-config-nacos-client3377

#### 1,pom

```xml
 <dependencies>
        <!-- nacos config-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        </dependency>

        <!-- SpringCloud ailibaba nacos-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!--springcloud整合web组件-->
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

```



#### 2,配置文件

这里需要配置两个配置文件,application.ymk和bootstarp.yml

```yaml
#${spring.application.name}+${spring.profile.active}+${spring.cloud.nacos.configfile-extension}
#nacos-config-client-dev.yaml
```





​			主要是为了可以与spring clodu config无缝迁移

bootstrapl.yaml

```yaml
server:
  port: 3377
spring:
  application:
    name: nacos-config-client

  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848 #向nacos服务注册中心注册地址
      config:
        server-addr: localhost:8848 #声明自己为nacos的配置中心地址
        file-extension: yaml #指定向nacos读取yaml类型的文件

```

aaplication.yaml

```yml
spring:
  profiles:
    active: dev #表示这是开发版本
```



```java
可以看到  nacos-config-client-dev.yaml
```

#### 3,主启动类

```
@SpringBootApplication
@EnableDiscoveryClient
```

#### 4,controller

```java
@RestController
@RefreshScope
public class ConfigCenterClientController {
    @Value("${config.info}")
    private String configInfo;

    @GetMapping(value = "/config/info")
    public String getConfigInfo(){
        return configInfo;
    }
}

```



```java
可以看到,这里也添加了@RefreshScope，不能和Resource一起使用
  		之前在Config配置中心,也是添加这个注解实现动态刷新的	
```

#### 5、测试，启动3377

访问,可以实现实时动态刷新，只要nacos上面有修改，那么3377也会修改

```java
http://localhost:3377/config/info
```





#### 5,在Nacos添加配置信息:

==**Nacos的配置规则:**==

**配置规则,就是我们在客户端如何指定读取配置文件,配置文件的命名的规则**

默认的命名方式:



```java
prefix:
		默认就是当前服务的服务名称
 		也可以通过spring.cloud.necos.config.prefix配置
spring.profile.active:
		就是我们在application.yml中指定的,当前是开发环境还是测试等环境
    这个可以不配置,如果不配置,那么前面的 -  也会没有
file-extension
     就是当前文件的格式(后缀),目前只支持yml和properties
```

==在web UI上创建配置文件:==

注意,DataId就是配置文件名字:

​		名字一定要按照上面的==规则==命名,否则客户端会读取不到配置文件



#### 6,注意默认就开启了自动刷新

此时我们修改了配置文件

客户端是可以立即更新的

​			因为Nacos支持Bus总线,会自动发送命令更新所有客户端





### Nacos配置中心之分类配置:

![Alibaba的27](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Alibaba的27.png)



![image-20201110210620499](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201110210620499.png)



![Alibaba的28](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Alibaba的28.png)

![Alibaba的29](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Alibaba的29.png)





NameSpace默认有一个:public名称空间

这三个类似java的: 包名 + 类名 + 方法名

![Alibaba的30](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Alibaba的30.png)



![Alibaba的31](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Alibaba的31.png)





#### 1,配置不同DataId:

修改配置中心3377下的application.yaml里面的

```yaml
spring:
  profiles:
    active: dev #表示这是开发版本
```

根据拼接规则会有不同的访问结果



​	==通过配置文件,实现多环境的读取:==

```java
此时,改为dev,就会读取dev的配置文件,改为test,就会读取test的配置文件
```





#### 2,配置不同的GroupID:

直接在新建配置文件时指定组，在config下面增加group的分组，指定要访问的分组

![image-20201110211801894](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201110211801894.png)



==在客户端配置,使用指定组的配置文件:==

![image-20201110211814288](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201110211814288.png)

**这两个配置文件都要修改**

![image-20201110211834734](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201110211834734.png)

​	

重启服务,即可

访问：

```java
http://localhost:3377/config/info
```





#### 配置不同的namespace:

![image-20201110212739394](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201110212739394.png)



==客户端配置使用不同名称空间:==

!![image-20201110212655969](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201110212655969.png)

**要通过命名空间id指定**

访问

```java
http://localhost:3377/config/info
```



# #####################2020.11.10#######################





### Nacos集群和持久化配置:

![Alibaba的45](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Alibaba的45.png)

**Nacos默认有自带嵌入式数据库,derby,但是如果做集群模式的话,就不能使用自己的数据库**

​			**不然每个节点一个数据库,那么数据就不统一了,需要使用外部的mysql**





#### 1,windows环境下，单机版,切换mysql数据库:

​					**将nacos切换到使用我们自己的mysql数据库:**

**1,nacos默认自带了一个sql文件,在nacos安装目录下conf里面，nacos-mysql.sql**

​		将它放到我们的mysql执行,先创建一个数据库

```sql
create database nacos_config;
```

登录mysql，执行，在刚建好的数据库里面

```sql
source D:/studySoft/springcloud/Nacos/nacos-server-1.2.1/nacos/conf/nacos-mysql.sql
```

![Alibaba的43](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Alibaba的43.png)

**2,修改Nacos安装目录下的安排application.properties,添加:**

```properties
####################################2020.11.11 nacos的的derby转移到mysql#############
spring.datasource.platform=mysql

db.num=1
db.url.0=jdbc:mysql://127.0.0.1:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=123456
```



**3,此时可以重启nacos,那么就会改为使用我们自己的mysql**







#### Linux上配置Nacos集群+Mysql数据库

==官方架构图:==

![Alibaba的45](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Alibaba的45.png)

**需要一个Nginx作为VIP**



1,下载安装Nacos的Linux版安装包

linux下启动后默认为集群模式，要通过**nginx代理才能进到nacos的登录界面**。
默认启动直接进入到解压后的bin目录执行
不走代理的话，启动时添加参数
也就是进到解压后的bin目录中，单机版，执行``
登录用户名：nacos 密码：nacos

单机启动nacos

```java
bash startup.sh -m standalone
```

Nacos安装包在我百度网盘，解压在linux的usr/local/springcloud/nacos里面

```java
tar -zvxf 文件名
```

安装好mysql

​		配置好相关的账号密码root 123456

创建数据库

```java
create database nacos_config;
```

2,进入安装目录,现在执行自带的sql文件

​			进入mysql,执行sql文件，使用刚创建好的数据库

```java
source /home/ubuntu/usr/local/springcloud/nacos/conf/nacos-mysql.sql
```



3.修改配置文件,切换为我们的mysql/conf文件下的

application.properties

​			就是上面windos版要修改的几个属性

```java
####################################2020.11.11 nacos的的derby转移到mysql#############
spring.datasource.platform=mysql

db.num=1
db.url.0=jdbc:mysql://127.0.0.1:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=123456
```



4,修改cluster.conf,指定哪几个节点是Nacos集群，要先创建一个文件出来

```java
cp cluster.conf.example cluster.conf
vim cluster.conf
```



​			这里使用3333,4444,5555作为三个Nacos节点监听的端口

​	通过**hostname -i** 可以获取127.0.1.1，每台机子的都不一样

```java
127.0.1.1:3333
127.0.1.1:4444
127.0.1.1:5555

```



5,我们这里就不配置在不同节点上了,就放在一个节点上，修改startup.sh，先复制一份备份.bk

​			既然要在一个节点上启动不同Nacos实例,就要修改startup.sh,使其根据不同端口启动不同Nacos实例

![image-20201111095659734](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201111095659734.png)

修改startup.sh，增加p，修改nohup

![image-20201111102909005](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201111102909005.png)



![image-20201111102931136](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201111102931136.png)



然后我们可以通过

```java 
./startup.sh -p 3333
```

来启动3333端口，但是这个时候先不启动

可以看到,这个脚本就是通过jvm启动nacos，先添加一个**p** 

​		所以我们最后修改的就是,**nohup java -Dserver.port=${PORT}**





6,配置Nginx:

​			先安装nginx，在我的博客有下载的教程

```
nginx文件安装完成之后的文件位置：

/usr/sbin/nginx：主程序
/etc/nginx：存放配置文件
/usr/share/nginx：存放静态文件
/var/log/nginx：存放日志
```

修改/etc/nginx/nginx.conf的文件

```java
	#gzip on;

	upstream cluster{
		server 127.0.1.1:3333;
		server 127.0.1.1:4444;
		server 127.0.1.1:5555;

	}

	server {
        	listen 		1111;
        	server_name localhost;
     	
	    	location / {
		    #root  html;
		    #index index.html index.hml;
		    proxy_pass http://cluster;
		}	
		
    }
```





7,启动Nacos:在nacos的bin目录下，得用单实例的方法去启动才可以注册进去集群，否则报错

```java
bash startup.sh -p 3333 -m standalone
bash startup.sh -p 4444 -m standalone
bash startup.sh -p 5555 -m standalone
```



8,启动nginx,在usr/sbin下执行

```java
./nginx
```



9,测试:

​		在windows访问192.168.116.128:1111

​		如果可以进入nacos的web界面,就证明安装成功了





9,将微服务9002注册到Nacos集群:

把注册的网址改了就行

![image-20201111161211985](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201111161211985.png)

10,进入Nacos的web界面

​		可以看到,已经注册成功

```java
http://192.168.116.128:1111/nacos/
```









## Sentinel:

实现熔断与限流,就是Hystrix

![Alibaba的53](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Alibaba的53.png)

![Alibaba的54](D:\Desktop\studyDocument\springcloud下载的笔记\图片\Alibaba的54.png)





### ==使用sentinel:==



1,下载sentinel的jar包

```java
https://github.com/alibaba/Sentinel/releases/tag/1.7.0
```

​	

2,运行sentinel

​		由于是一个jar包,所以可以直接java -jar运行	

```java
java -jar *.jar
```





​		注意,默认sentinel占用8080端口

3,访问sentinel

​		localhost:8080





### 微服务整合sentinel:

##### 1,启动Nacos，要把微服务注册进去Nacos

##### 2,新建一个项目,8401,主要用于配置sentinel,

##### cloudalibaba-sentinel-service8401

1.  pom

```xml
<dependencies>
        <!-- SpringCloud ailibaba nacos-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
        <!-- SpringCloud ailibaba sentinel-datasource-nacos 持久化需要用到-->
        <dependency>
            <groupId>com.alibaba.csp</groupId>
            <artifactId>sentinel-datasource-nacos</artifactId>
        </dependency>
        <!-- SpringCloud ailibaba sentinel-->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
        </dependency>

        
        <dependency><!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.atlgq.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
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

```

1. 配置文件

   ```yml
   server:
     port: 8401
   spring:
     application:
       name: cloudalibaba-sentinel-service
     cloud:
       nacos:
         discovery:
           #向Nacos注册自己
           server-addr: localhost:8848
       sentinel:
         transport:
           #配置Sentinel dashboard的地址，默认8080向dashboard注册自己
           dashboard: localhost:8080
           port: 8719
           #默认8719端口，假如被占用自动从8719开始一次+1扫描，直到找到未被占用的端口
   
   #暴露自己的端口
   management:
     endpoints:
       web:
         exposure:
           include: '*'
   
   ```

   

2. 主启动类

   ```java
   @SpringBootApplication
   @EnableDiscoveryClient
   public class FollowLimitMain8401 {
       public static void main(String[] args) {
           SpringApplication.run(FollowLimitMain8401.class,args);
       }
   }
   
   ```

   

3. controller

   ```java
   @RestController
   public class FlowLimitController {
   
       @GetMapping(value = "/testA")
       public String testA(){
           return "testA";
       }
       @GetMapping(value = "/testB")
       public String testB(){
           return "testB";
       }
   }
   
   ```

   

4. 到这里就可以启动

5. 启动Nacos，sentinel,8401

   ​	此时我们到sentinel中查看,发现并8401的任何信息

   ​	是因为,sentinel是懒加载,需要我们执行一次访问,才会有信息

   ​	访问

   ```java
   localhost/8401/testA
   ```

   

   

6. 可以看到.已经开始监听了

​    



### sentinel的流控规则

流量限制控制规则

![sentinel的的7](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的的7.png)

![sentinel的3](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的3.png)

![sentinel的4](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的4.png)





==流控模式==:

1.   直接快速失败

       ==直接失败的效果:==

2. 线程数:

   ​		![sentinel的8](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的8.png)

   

   ![sentinel的10](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的10.png)

   

   

   ```
   比如a请求过来,处理很慢,在一直处理,此时b请求又过来了,此时因为a占用一个线程,此时要处理b请求就只有额外开启一个线程,那么就会报错
   ```

   ![sentinel的11](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的11.png)

   

3.   关联:

     ![sentinel的12](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的12.png)

     ==应用场景:  比如**支付接口**达到阈值,就要限流下**订单的接口**,防止一直有订单==

     ![sentinel的13](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的13.png)

     **当testB达到阈值,QPS大于1,就让testA之后的请求直接失败**

     可以使用postman压测

​    

4.   链路:
     多个请求调用同一个微服务

5. 预热Warm up:

   ![sentinel的14](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的14.png)

   ​	 ![sentinel的15](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的15.png)

   ![sentinel的16](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的16.png)

   

   

    ==应用场景==

   ![sentinel的17](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的17.png)

6. 排队等待:

   ![sentinel的18](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的18.png)




​				![sentinel的18](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的18.png)









### 降级规则:

**就是熔断降级**

![sentinel的20](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的20.png)

![sentinel的21](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的21.png)



![sentinel的23](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的23.png)







#### 1,RT配置:

新增一个请求方法用于测试

![sentinel的24](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的24.png)

==配置RT:==

​				这里配置的PT,默认是秒级的平均响应时间

![sentinel的25](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的25.png)

默认计算平均时间是: 1秒类进入5个请求,并且响应的平均值超过阈值(这里的200ms),就报错]

​			1秒5请求是Sentinel默认设置的

==测试==

![sentinel的26](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的26.png)

![sentinel的27](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的27.png)





**默认熔断后.就直接抛出异常**







#### 2,异常比例:

![sentinel的28](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的28.png)

修改请求方法

![sentinel的29](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的29.png)

配置:

![sentinel的31](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的31.png)



==如果没触发熔断,这正常抛出异常==:

![sentinel的32](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的32.png)

==触发熔断==:

![sentinel的33](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的33.png)









#### 3, 异常数:

![sentinel的34](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的34.png)

![sentinel的35](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的35.png)

一分钟之内,有5个请求发送异常,进入熔断









### 热点规则:

![sentinel的36](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的36.png)

![sentinel的37](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的37.png)





比如:

​			localhost:8080/aa?name=aa

​			localhost:8080/aa?name=b'b

​			加入两个请求中,带有参数aa的请求访问频次非常高,我们就现在name==aa的请求,但是bb的不限制



==如何自定义降级方法,而不是默认的抛出异常?==



**使用@SentinelResource直接实现降级方法,它等同Hystrix的@HystrixCommand**

![sentinel的39](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的39.png)



==定义热点规则:==

在controller的代码

```java
@GetMapping(value = "/testHotKey")
    @SentinelResource(value = "testHotKey",blockHandler = "deal_testHotKey")
    public String testHotKey(@RequestParam(value = "p1",required = false) String p1,
                             @RequestParam(value = "p2",required = false) String p2)
    {
        return "-----------testHotKey";
    }

    public String deal_testHotKey(String p1, String p2, BlockException exception){
        return "deal_testHotKey，/(ㄒoㄒ)/~~";
    }
```

在sentinel配置热点

![image-20201111201850485](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201111201850485.png)





**此时我们访问/testHotkey并且带上才是p1**

​			如果qps大于1,就会触发我们定义的降级方法

![sentinel的41](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的41.png)

**但是我们的参数是P2,就没有问题**

![image-20201111201957714](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201111201957714.png)



只有带了p1,才可能会触发热点限流

#### 2,设置热点规则中的其他选项:

![sentinel的45](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的45.png)

**需求:**

![sentinel的46](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的46.png)

![sentinel的47](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的47.png)



==测试==

![sentinel的48](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的48.png)

![sentinel的49](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的49.png)



**注意:**

参数类型只支持,8种基本类型+String类





==注意:==

如果我们程序出现异常,是不会走blockHander的降级方法的**,因为这个方法只配置了热点规则**,没有配置限流规则

我们这里配置的降级方法是sentinel针对热点规则配置的

只有触发热点规则才会降级

![sentinel的50](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的50.png)



### 3,系统规则:

系统自适应限流:
			**从整体维度对应用入口进行限流**

对整体限流,比如设置qps到达100,这里限流会限制整个系统不可以

![sentinel的51](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的51.png)

![sentinel的52](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的52.png)



==测试==:

![sentinel的53](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的53.png)

![sentinel的54](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的54.png)









### @SentinelResource注解:

**用于配置降级等功能**

1,环境搭建

1. 为8401添加依赖

   添加我们自己的commone包的依赖

   ```xml
   <dependency><!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
               <groupId>com.atlgq.springcloud</groupId>
               <artifactId>cloud-api-commons</artifactId>
               <version>${project.version}</version>
           </dependency>
   ```

   

2. 额外创建一个controller类

   ```java
   @RestController
   public class RateLimitController {
   
       @GetMapping(value = "/byResource")
       @SentinelResource(value = "byResource", blockHandler = "handleException")
       public CommonResult byResource(){
           return new CommonResult(200,"byResource测试成功",new Payment(2020L,"serial20201112"));
       }
       public CommonResult handleException(BlockException e){
           return new CommonResult(404,e.getClass().getCanonicalName() + "\t 服务不可用");
       }
   
       @GetMapping(value = "/byUrl")
       @SentinelResource(value = "byUrl")
       public CommonResult byUrl(){
           return new CommonResult(200,"byUrl测试成功",new Payment(2020L,"serial20201112"));
       }
   }
   ```

   

    

3. 配置限流

   **注意,我们这里配置规则,资源名指定的是@SentinelResource注解value的值,**

   **这样也是可以的,也就是不一定要指定访问路径**

   

   ![image-20201112091434666](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201112091434666.png)

   

   

4.   测试.

    可以看到已经进入降级方法了

    

5.   ==此时我们关闭8401服务==

    在sentinel上定义的规则都是临时的

    可以看到,这些定义的规则是临时的,关闭服务,规则就没有了
    
    



**可以看到,上面配置的降级方法,又出现Hystrix遇到的问题了**

​			1、系统默认的，没有体现我们自己的业务要求

​		 	2、依照现有的条件，我们自定义的处理方法又和业务代码耦合在一起，不直观

​			3、每个业务方法都要添加一个兜底，代码膨胀

​			4、全局统一的处理方法没有体现

#### 自定义限流处理逻辑:

1. ==单独创建一个类,用于处理限流==，里面的方法需要为静态方法

   ```java
   public class CustomerBlockHandler {
       public static CommonResult handleException1(BlockException e){
           System.out.println("this is handleException1handleException1handleException1");
           return new CommonResult(404,"客户自定义的handleException1111111111111");
       }
   
       public static CommonResult handleException2(BlockException e){
           System.out.println("this is handleException1handleException1handleException1");
           return new CommonResult(404,"客户自定义的handleException22222222222");
       }
   }
   
   ```

   

2. ==在controller中,指定使用自定义类中的方法作为降级方法==

   ```java
     @GetMapping(value = "/byUrl")
       @SentinelResource(value = "byUrl",
               blockHandlerClass = CustomerBlockHandler.class,
               blockHandler = "handleException1")
       public CommonResult byUrl(){
           return new CommonResult(200,"byUrl测试成功",new Payment(2020L,"serial20201112"));
       }
   
       @GetMapping(value = "/byUrl1")
       @SentinelResource(value = "byUrl1",
               blockHandlerClass = CustomerBlockHandler.class,
               blockHandler = "handleException2")
       public CommonResult byUrl1(){
           return new CommonResult(200,"byUrl11111测试成功",new Payment(2020L,"serial20201112"));
       }
   ```

   

3. ==Sentinel中定义流控规则==:

   这里资源名,是以url指定,也可以使用@SentinelResource注解value的值指定

   

   

4. ==测试==:

   

5. ==整体==:

   

6. 





#### @SentinelResource注解的其他属性:

![sentinel的的7](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的的7.png)

![sentinel的的6](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的的6.png)





### 服务熔断:

1.  **启动nacos和sentinel**

2. **新建两个pay模块  9003和9004**

    ```java
   cloudalibaba-provider-payment9003
       cloudalibaba-provider-payment9004
   ```

   

   1. pom

      ```xml
      <dependencies>
          <!-- SpringCloud ailibaba nacos-->
          <dependency>
              <groupId>com.alibaba.cloud</groupId>
              <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
          </dependency>
          <!-- SpringCloud ailibaba sentinel-->
          <dependency>
              <groupId>com.alibaba.cloud</groupId>
              <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
          </dependency>
          <dependency><!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
              <groupId>com.atlgq.springcloud</groupId>
              <artifactId>cloud-api-commons</artifactId>
              <version>${project.version}</version>
          </dependency>
          <dependency>
              <groupId>org.springframework.cloud</groupId>
              <artifactId>spring-cloud-starter-openfeign</artifactId>
          </dependency>
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
       
      
      ```

      

   2. 配置文件

      ```yaml
      server:
        port: 9003
      spring:
        application:
          name: nacos-payment-provider
        cloud:
          nacos:
            discovery:
              server-addr: localhost:8848 #注册自己
      
      management:
        endpoints:
          web:
            exposure:
              include: "*"
      
      ```

      

   3. 主启动类 

      ```java
      @SpringBootApplication
      @EnableDiscoveryClient
      public class PaymentMain9003 {
      
          public static void main(String[] args) {
              SpringApplication.run(PaymentMain9003.class,args);
          }
      }
       
      
      ```

   4. controller

      ```java
      
      
      @RestController
      public class PaymentController {
      
          @Value("${server.port}")
          private String serverPort;
      
          private static HashMap<Long, Payment> hashMap = new HashMap<>();
          static {
              hashMap.put(1L,new Payment(1L,"2dfadfadgadfadfasdfad"));
              hashMap.put(2L,new Payment(2L,"2dfadfadgadfadfasdfad"));
              hashMap.put(3L,new Payment(3L,"2dfadfadgadfadfasdfad"));
              hashMap.put(4L,new Payment(4L,"2dfadfadgadfadfasdfad"));
          }
      
          @GetMapping("/paymentSQL/{id}")
          public CommonResult<Payment> paymentSQL(@PathVariable("id") Long id){
              Payment payment = hashMap.get(id);
              return (CommonResult<Payment>) new CommonResult(200,"serverPort " + serverPort,payment);
          }
      }
      
      ```

      

       **然后启动9003.9004**

3. **新建一个order-84消费者模块:**

   ```java
   cloudailbaba-comsumer-nacos-order84
   ```

   

   

   1. pom

      ```xml
      <dependencies>
          <!-- SpringCloud ailibaba nacos-->
          <dependency>
              <groupId>com.alibaba.cloud</groupId>
              <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
          </dependency>
          <!-- SpringCloud ailibaba sentinel-->
          <dependency>
              <groupId>com.alibaba.cloud</groupId>
              <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
          </dependency>
          <dependency><!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
              <groupId>com.atlgq.springcloud</groupId>
              <artifactId>cloud-api-commons</artifactId>
              <version>${project.version}</version>
          </dependency>
          <dependency>
              <groupId>org.springframework.cloud</groupId>
              <artifactId>spring-cloud-starter-openfeign</artifactId>
          </dependency>
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
       
      
      ```

      

   2. 配置文件

      ```yaml
      server:
        port: 84
      
      
      spring:
        application:
          name: nacos-order-consumer
        cloud:
          nacos:
            discovery:
              server-addr: localhost:8848
          sentinel:
            transport:
              dashboard: localhost:8080
              port: 8719
      
      server-url:
        nacos-user-service: http://nacos-payment-provider
      
      ```

      

   3. 主启动类

      ```java
      @SpringBootApplication
      @EnableDiscoveryClient
      ```

      

   4. 配置类

      ```java
      @Configuration
      public class ApplicationContextConfig {
      
          @Bean
          @LoadBalanced
          public RestTemplate getRestTemplate(){
              return new RestTemplate();
          }
      }
      
      ```

      

   5. controller

      ```java
      
      @RestController
      @Slf4j
      public class CirlceBreakerController {
          public static final String SERVER_URL = "http://nacos-payment-provider";
      
          @Resource
          private RestTemplate restTemplate;
      
          @RequestMapping(value = "/consumer/fallback/{id}")
          @SentinelResource(value = "fallback")
          public CommonResult<Payment> fallback(@PathVariable("id") Long id){
              CommonResult<Payment> result = restTemplate.getForObject(SERVER_URL+"/paymentSQL/"+id,CommonResult.class,id);
      
              if(id == 4){
                  throw  new IllegalArgumentException("IllegalArgumentException，参数异常");
              }else if(result.getData() == null){
                  throw  new NullPointerException("NullPointerException异常");
              }
              return result;
          }
      }
      
      ```

      

      

   6. **==为业务方法添加fallback来指定降级方法==**:

      ```java
       @RequestMapping(value = "/consumer/fallback/{id}")
          @SentinelResource(value = "fallback" , fallback = "handleFallback")
          public CommonResult<Payment> fallback(@PathVariable("id") Long id){
              CommonResult<Payment> result = restTemplate.getForObject(SERVER_URL+"/paymentSQL/"+id,CommonResult.class,id);
      
              if(id == 4){
                  throw  new IllegalArgumentException("IllegalArgumentException，参数异常");
              }else if(result.getData() == null){
                  throw  new NullPointerException("NullPointerException异常");
              }
              return result;
          }
      
          public CommonResult handleFallback(@PathVariable("id") Long id ,Throwable e){
              Payment payment = new Payment((long) 1,null);
              return new CommonResult(404,"兜底的方法异常，"+e.getMessage(),payment);
          }
      ```

      

      ​	==重启order==

      测试:

      ![image-20201112114935399](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201112114935399.png)

       

       ==所以,fallback是用于管理异常的,当业务方法发生异常,可以降级到指定方法==

      ​			注意,我们这里==并没有使用sentinel配置任何规则==,但是却降级成功,就是因为

      ​			fallback是用于管理异常的,当业务方法发生异常,可以降级到指定方法==

      

   7. **==为业务方法添加blockHandler,看看是什么效果==**

      ```java
       @RequestMapping(value = "/consumer/fallback/{id}")
      //    @SentinelResource(value = "fallback")
      //    @SentinelResource(value = "fallback" , fallback = "handleFallback")
          @SentinelResource(value = "fallback" , blockHandler = "blockHandler")
          public CommonResult<Payment> fallback(@PathVariable("id") Long id){
              CommonResult<Payment> result = restTemplate.getForObject(SERVER_URL+"/paymentSQL/"+id,CommonResult.class,id);
      
              if(id == 4){
                  throw  new IllegalArgumentException("IllegalArgumentException，参数异常");
              }else if(result.getData() == null){
                  throw  new NullPointerException("NullPointerException异常");
              }
              return result;
          }
      
          public CommonResult handleFallback(@PathVariable("id") Long id ,Throwable e){
              Payment payment = new Payment((long) 1,null);
              return new CommonResult(404,"兜底的方法异常，"+e.getMessage(),payment);
          }
      
          public CommonResult blockHandler(@PathVariable("id") Long id ,Throwable e){
              Payment payment = new Payment((long) 1,null);
              return new CommonResult(404,"blockHandler的方法异常，"+e.getMessage(),payment);
          }
      ```

      

      **重启84,访问业务方法:**

      ![image-20201112115337235](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201112115337235.png)

       可以看到.,直接报错了,并没有降级

      ​				也就是说,blockHandler==只对sentienl定义的规则降级==

       

   8.   **==如果fallback和blockHandler都配置呢?==**]

        ![sentinel的的18](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的的18.png)

        **设置qps规则,阈值1**

        ![sentinel的的19](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的的19.png)

        ==测试:==

       ![sentinel的的20](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的的20.png)

        

        可以看到,当两个都同时生效时,==blockhandler优先生效==

   9.  **==@SentinelResource还有一个属性,exceptionsToIgnore==**

       ![image-20201112123921380](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201112123921380.png)

        **exceptionsToIgnore指定一个异常类,**

       ​					**表示如果当前方法抛出的是指定的异常,不降级,直接对用户抛出异常**

        ![image-20201112123710567](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201112123710567.png)

        

        

   



### sentinel整合ribbon+openFeign+fallback



1.  修改84模块,使其支持feign

    1. pom

       ```xml
    <dependency>
                   <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-openfeign</artifactId>
               </dependency>
       ```
     ```
   
     ```
   
   
   ```
   
   ```
   
2. 配置文件
   
   ```yaml
       #开启整合
   feign:
         sentinel:
       enabled: true
   ```

   ​    

    3. 主启动类,也要修改

       ```java
   @EnableFeignClients
       ```

       

    4. 创建远程调用pay模块的接口

       ```java
   @FeignClient(value = "nacos-payment-provider" ,fallback = PaymentFallbackService.class)
       public interface PaymentService {
       @GetMapping(value = "/paymentSQL/{id}")
           public CommonResult<Payment> paymentSQL(@PathVariable("id") Long id);
   }
       
       ```
   
       
   
    5. 创建这个接口的实现类,用于降级
   
       ```java
       @Component
       public class PaymentFallbackService implements PaymentService {
           @Override
           public CommonResult<Payment> paymentSQL(Long id) {
               return new CommonResult(404,"this is PaymentFallbackService");
           }
       }
       
       ```
   
       
   
    6. 再次修改接口,指定降级类
   
       ```java
       @FeignClient(value = "nacos-payment-provider" ,fallback = PaymentFallbackService.class)
       ```
   
       
   
    7. controller添加远程调用
   
       ```java
       
           @Resource
           private PaymentService paymentService;
       
           @GetMapping(value = "/consumer/paymentSQL/{id}")
           public CommonResult<Payment> paymentSQL(@PathVariable("id") Long id) {
               return paymentService.paymentSQL(id);
           }
       
       ```
   
       
   
    8.  测试
   
        启动9003,84
   
    9.   测试,如果关闭9003.看看84会不会降级
   
        ![image-20201112131848971](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201112131848971.png)
   
        
   
        **可以看到,正常降级了**
        
        

**熔断框架比较**

![sentinel的的31](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的的31.png)









### sentinel持久化规则

默认规则是临时存储的,重启sentinel就会消失

![sentinel的的32](D:\Desktop\studyDocument\springcloud下载的笔记\图片\sentinel的的32.png)

**这里以之前的8401为案例进行修改:**

1.  修改8401的pom

    ```xml
    
    <!-- SpringCloud ailibaba sentinel-datasource-nacos 持久化需要用到-->
    <dependency>
        <groupId>com.alibaba.csp</groupId>
        <artifactId>sentinel-datasource-nacos</artifactId>
    </dependency>
     
    ```

    

2. 修改配置文件:

   添加:dataSource

   ```yaml
   spring:
     application:
       name: cloudalibaba-sentinel-service
     cloud:
       nacos:
         discovery:
           #向Nacos注册自己
           server-addr: localhost:8848
       sentinel:
         transport:
           #配置Sentinel dashboard的地址，默认8080向dashboard注册自己
           dashboard: localhost:8080
           port: 8719
           #默认8719端口，假如被占用自动从8719开始一次+1扫描，直到找到未被占用的端口
         datasource:
           nacos:
             server-add: localhost:8848
             dataId: cloudalibaba-sentinel-service
             groupId: DEFAULT_GROUP
             data-type: json
             rule-type: flow
   ```

   

    **实际上就是指定,我们的规则要保证在哪个名称空间的哪个分组下**

    			这里没有指定namespace, 但是是可以指定的

   ​			**注意,这里的dataid要与8401的服务名一致**

3. **在nacos中创建一个配置文件,dataId就是上面配置文件中指定的**

   ==json中,这些属性的含义:==

   ```java
   [
       {
            "resource":"/retaLimit/byUrl",
            "limitApp":"default",
            "grade":1,
            "count":1,
            "strategy":0,
            "controlBehavior":0,
            "clusterMode":false    
       }
   ]
   ```

   ![image-20201112135505451](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201112135505451.png)

    

   

4.   启动8401:nacos，sentinel

     

     可以看到,直接读取到了规则

5.   关闭8401

​    

6.   此时重启8401,如果sentinel又可以正常读取到规则,那么证明持久化成功

    可以看到,又重新出现了

    

    























## Seata:

是一个分布式事务的解决方案,

**分布式事务中的一些概念,也是seata中的概念:**

​	![seala](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala.png)

![seala的2](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的2.png)



![seala的3](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的3.png)



### seata安装:

1. **下载安装seata的安装包**

   百度云

2. **修改file.conf**，先备份文件

   ![image-20201112142524619](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201112142524619.png)

    

   ![image-20201112142554157](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201112142554157.png)

   ​	

![image-20201112142611314](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201112142611314.png)

1. **mysql建库建表**

   1,上面指定了数据库为seata,所以创建一个数据库名为seata

   2,建表,在seata的安装目录下有一个db_store.sql,运行即可

2. **继续修改配置文件,修改registry.conf**

   配置seata作为微服务,指定注册中心

   ![image-20201112142638973](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201112142638973.png)

3. 启动

   先启动nacos，在bin题目下的bat

   在启动seata-server(运行安装目录下的,seata-server.bat)

   

**业务说明**

![seala的8](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的8.png)

下单--->库存--->账号余额



1.  创建三个数据库

    ![image-20201112205718351](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201112205718351.png)

2. 创建对应的表

   ```sql
   CREATE TABLE t_order(
       `id` BIGINT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
       `user_id` BIGINT(11) DEFAULT NULL COMMENT '用户id',
       `product_id` BIGINT(11) DEFAULT NULL COMMENT '产品id',
       `count` INT(11) DEFAULT NULL COMMENT '数量',
       `money` DECIMAL(11,0) DEFAULT NULL COMMENT '金额',
       `status` INT(1) DEFAULT NULL COMMENT '订单状态：0：创建中; 1：已完结'
   ) ENGINE=INNODB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8;
    
   SELECT * FROM t_order;
   ```

   ```sql
   CREATE TABLE t_storage(
       `id` BIGINT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
       `product_id` BIGINT(11) DEFAULT NULL COMMENT '产品id',
      `total` INT(11) DEFAULT NULL COMMENT '总库存',
       `used` INT(11) DEFAULT NULL COMMENT '已用库存',
       `residue` INT(11) DEFAULT NULL COMMENT '剩余库存'
   ) ENGINE=INNODB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;
    
   INSERT INTO seata_storage.t_storage(`id`,`product_id`,`total`,`used`,`residue`)
   VALUES('1','1','100','0','100');
    
    
   SELECT * FROM t_storage;
   
   
   ```

   

   ```sql
   CREATE TABLE t_account(
       `id` BIGINT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY COMMENT 'id',
       `user_id` BIGINT(11) DEFAULT NULL COMMENT '用户id',
       `total` DECIMAL(10,0) DEFAULT NULL COMMENT '总额度',
       `used` DECIMAL(10,0) DEFAULT NULL COMMENT '已用余额',
       `residue` DECIMAL(10,0) DEFAULT '0' COMMENT '剩余可用额度'
   ) ENGINE=INNODB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;
    
   INSERT INTO seata_account.t_account(`id`,`user_id`,`total`,`used`,`residue`) VALUES('1','1','1000','0','1000')
    
    
    
   SELECT * FROM t_account;
   
   ```

   

   

3. 创建回滚日志表,方便查看

   把conf下面的db_undo_log.sql，分别写进去各个数据库的里面，使用source的方法

   ```java
   db_undo_log.sql
   ```

   

   

   **注意==每个库都要执行一次==这个sql,生成回滚日志表**

4. ==每个业务都创建一个微服务,也就是要有三个微服务,订单,库存,账号==

   ​     ==订单==,

   # seata-order-service2001

   

   ![image-20201112212652051](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201112212652051.png)

   

   

   1. pom

      ```xml
      
          <dependencies>
              <!--nacos-->
              <dependency>
                  <groupId>com.alibaba.cloud</groupId>
                  <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
              </dependency>
              <!--seata-->
              <dependency>
                  <groupId>com.alibaba.cloud</groupId>
                  <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
                  <exclusions>
                      <exclusion>
                          <artifactId>seata-all</artifactId>
                          <groupId>io.seata</groupId>
                      </exclusion>
                  </exclusions>
              </dependency>
              <dependency>
                  <groupId>io.seata</groupId>
                  <artifactId>seata-all</artifactId>
                  <version>0.9.0</version>
              </dependency>
              <!--feign-->
              <dependency>
                  <groupId>org.springframework.cloud</groupId>
                  <artifactId>spring-cloud-starter-openfeign</artifactId>
              </dependency>
              <!--web-actuator-->
              <dependency>
                  <groupId>org.springframework.boot</groupId>
                  <artifactId>spring-boot-starter-web</artifactId>
              </dependency>
              <dependency>
                  <groupId>org.springframework.boot</groupId>
                  <artifactId>spring-boot-starter-actuator</artifactId>
              </dependency>
              <!--mysql-druid-->
              <dependency>
                  <groupId>mysql</groupId>
                  <artifactId>mysql-connector-java</artifactId>
                  <version>5.1.37</version>
              </dependency>
              <dependency>
                  <groupId>com.alibaba</groupId>
                  <artifactId>druid-spring-boot-starter</artifactId>
                  <version>1.1.10</version>
              </dependency>
              <dependency>
                  <groupId>org.mybatis.spring.boot</groupId>
                  <artifactId>mybatis-spring-boot-starter</artifactId>
                  <version>2.0.0</version>
              </dependency>
              <dependency>
                  <groupId>org.springframework.boot</groupId>
                  <artifactId>spring-boot-starter-test</artifactId>
                  <scope>test</scope>
              </dependency>
              <dependency>
                  <groupId>org.projectlombok</groupId>
                  <artifactId>lombok</artifactId>
                  <optional>true</optional>
              </dependency>
          </dependencies>
      
      
      ```

      

   2. 配置文件

      ```yaml
      server:
        port: 2001
      
      spring:
        application:
          name: seata-order-service
        cloud:
          alibaba:
            seata:
              #自定义事务组名称需要与seata-server中的对应
              tx-service-group: fsp_tx_group
          nacos:
            discovery:
              server-addr: localhost:8848
        datasource:
          driver-class-name: com.mysql.jdbc.Driver
          url: jdbc:mysql://localhost:3306/seata_order
          username: root
          password: 123456
      
      feign:
        hystrix:
          enabled: false
      
      logging:
        level:
          io:
            seata: info
      
      mybatis:
        mapperLocations: classpath:mapper/*.xml
      
      ```

      还要额外创建其他配置文件,创建一个file.conf:

       ```.conf
      transport {
        # tcp udt unix-domain-socket
        type = "TCP"
        #NIO NATIVE
        server = "NIO"
        #enable heartbeat
        heartbeat = true
        #thread factory for netty
        thread-factory {
          boss-thread-prefix = "NettyBoss"
          worker-thread-prefix = "NettyServerNIOWorker"
          server-executor-thread-prefix = "NettyServerBizHandler"
          share-boss-worker = false
          client-selector-thread-prefix = "NettyClientSelector"
          client-selector-thread-size = 1
          client-worker-thread-prefix = "NettyClientWorkerThread"
          # netty boss thread size,will not be used for UDT
          boss-thread-size = 1
          #auto default pin or 8
          worker-thread-size = 8
        }
        shutdown {
          # when destroy server, wait seconds
          wait = 3
        }
        serialization = "seata"
        compressor = "none"
      }
      
      service {
        vgroup_mapping.fsp_tx_group = "default"
        default.grouplist = "127.0.0.1:8091"
        enableDegrade = false
        disable = false
        max.commit.retry.timeout = "-1"
        max.rollback.retry.timeout = "-1"
        disableGlobalTransaction = false
      }
      
      
      client {
        async.commit.buffer.limit = 10000
        lock {
          retry.internal = 10
          retry.times = 30
        }
        report.retry.count = 5
        tm.commit.retry.count = 1
        tm.rollback.retry.count = 1
      }
      
      ## transaction log store
      store {
        ## store mode: file、db
        mode = "db"
      
        ## file store
        file {
          dir = "sessionStore"
      
          # branch session size , if exceeded first try compress lockkey, still exceeded throws exceptions
          max-branch-session-size = 16384
          # globe session size , if exceeded throws exceptions
          max-global-session-size = 512
          # file buffer size , if exceeded allocate new buffer
          file-write-buffer-cache-size = 16384
          # when recover batch read size
          session.reload.read_size = 100
          # async, sync
          flush-disk-mode = async
        }
      
        ## database store
        db {
          ## the implement of javax.sql.DataSource, such as DruidDataSource(druid)/BasicDataSource(dbcp) etc.
          datasource = "dbcp"
          ## mysql/oracle/h2/oceanbase etc.
          db-type = "mysql"
          driver-class-name = "com.mysql.jdbc.Driver"
          url = "jdbc:mysql://127.0.0.1:3306/seata"
          user = "root"
          password = "123456"
          min-conn = 1
          max-conn = 3
          global.table = "global_table"
          branch.table = "branch_table"
          lock-table = "lock_table"
          query-limit = 100
        }
      }
      lock {
        ## the lock store mode: local、remote
        mode = "remote"
      
        local {
          ## store locks in user's database
        }
      
        remote {
          ## store locks in the seata's server
        }
      }
      recovery {
        #schedule committing retry period in milliseconds
        committing-retry-period = 1000
        #schedule asyn committing retry period in milliseconds
        asyn-committing-retry-period = 1000
        #schedule rollbacking retry period in milliseconds
        rollbacking-retry-period = 1000
        #schedule timeout retry period in milliseconds
        timeout-retry-period = 1000
      }
      
      transaction {
        undo.data.validation = true
        undo.log.serialization = "jackson"
        undo.log.save.days = 7
        #schedule delete expired undo_log in milliseconds
        undo.log.delete.period = 86400000
        undo.log.table = "undo_log"
      }
      
      ## metrics settings
      metrics {
        enabled = false
        registry-type = "compact"
        # multi exporters use comma divided
        exporter-list = "prometheus"
        exporter-prometheus-port = 9898
      }
      
      support {
        ## spring
        spring {
          # auto proxy the DataSource bean
          datasource.autoproxy = false
        }
      }
      
      
      
       ```

      创建registry.conf:

      ```conf
      registry {
        # file 、nacos 、eureka、redis、zk、consul、etcd3、sofa
        type = "nacos"
      
        nacos {
          serverAddr = "localhost:8848"
          namespace = ""
          cluster = "default"
        }
        eureka {
          serviceUrl = "http://localhost:8761/eureka"
          application = "default"
          weight = "1"
        }
        redis {
          serverAddr = "localhost:6379"
          db = "0"
        }
        zk {
          cluster = "default"
          serverAddr = "127.0.0.1:2181"
          session.timeout = 6000
          connect.timeout = 2000
        }
        consul {
          cluster = "default"
          serverAddr = "127.0.0.1:8500"
        }
        etcd3 {
          cluster = "default"
          serverAddr = "http://localhost:2379"
        }
        sofa {
          serverAddr = "127.0.0.1:9603"
          application = "default"
          region = "DEFAULT_ZONE"
          datacenter = "DefaultDataCenter"
          cluster = "default"
          group = "SEATA_GROUP"
          addressWaitTime = "3000"
        }
        file {
          name = "file.conf"
        }
      }
      
      config {
        # file、nacos 、apollo、zk、consul、etcd3
        type = "file"
      
        nacos {
          serverAddr = "localhost"
          namespace = ""
        }
        consul {
          serverAddr = "127.0.0.1:8500"
        }
        apollo {
          app.id = "seata-server"
          apollo.meta = "http://192.168.1.204:8801"
        }
        zk {
          serverAddr = "127.0.0.1:2181"
          session.timeout = 6000
          connect.timeout = 2000
        }
        etcd3 {
          serverAddr = "http://localhost:2379"
        }
        file {
          name = "file.conf"
        }
      }
      
      
      
      
      ```

      ==实际上,就是要将seata中的我们之前修改的两个配置文件复制到这个项目下==

   3. **主启动类**

      ```java
      @EnableDiscoveryClient
      @EnableFeignClients
      @SpringBootApplication(exclude = DataSourceAutoConfiguration.class) //取消数据源自动配置，配置我们自己的数据源
      public class SeataOrderMainApp2001 {
          public static void main(String[] args) {
              SpringApplication.run(SeataOrderMainApp2001.class,args);
          }
      }
      
      ```

      

   4. **service层**

      ```xml
      public interface OrderService {
          void create(Order order);
      }
      
      ```
      
      ```xml
      @FeignClient(value = "seata-account-service")
      public interface AccountService {
       @PostMapping(value = "/account/decrease")
          CommonResult decrease(@RequestParam("userId") Long userId, @RequestParam("money") BigDecimal money);
      
      }
      
      ```
      
      ```xml
      @FeignClient(value = "seata-storage-service")
      public interface StorageService {
          @PostMapping(value = "/storage/decrease")
          CommonResult decrease(@RequestParam("productId") Long productId,@RequestParam("count") Integer count);
      }
      
      ```

      ```xml
      
      @Service
      @Slf4j
      public class OrderServiceImpl implements OrderService {
      
          @Resource
          private OrderDao orderDao;
          @Resource
          private StorageService storageService;
          @Resource
          private AccountService accountService;
          @Override
          public void create(Order order) {
              log.info("----------开始创建订单");
              orderDao.create(order);
              log.info("---------订单开始调用库存，做扣减");
           storageService.decrease(order.getProductId(),order.getCount());
              log.info("--------------订单微服务开始调用账户，做扣减,userID = " + order.getUserId() + "money = " + order.getMoney() );
              accountService.decrease(order.getUserId(),order.getMoney());
              log.info("--------------订单微服务开始调用账户，做扣减完成");
      
              //修改订单状态
              log.info("----------开始修改订单");
              orderDao.update(order.getUserId(),0);
              log.info("----------结束修改订单");
      
              log.info("----------订单结束了");
      
          }
      }
      
      ```
      
      
      
       
      
       
      
       
      
       
      
       
      
       
      
   5. **dao层,也就是接口**

      ```java
      @Mapper
      public interface OrderDao
      {
          //新建订单
          void create(Order order);
      
          //修改订单状态，从零改为1
          void update(@Param("userId") Long userId,@Param("status") Integer status);
      }
      ```
      
       ==在resource下创建mapper文件夹,编写mapper.xml==
      
      ```xml
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
      
      <mapper namespace="com.atlgq.springcloud.alibaba.dao.OrderDao">

          <resultMap id="BaseResultMap" type="com.atlgq.springcloud.alibaba.domain.Order">
           <id column="id" property="id" jdbcType="BIGINT"/>
              <result column="user_id" property="userId" jdbcType="BIGINT"/>
              <result column="product_id" property="productId" jdbcType="BIGINT"/>
              <result column="count" property="count" jdbcType="INTEGER"/>
              <result column="money" property="money" jdbcType="DECIMAL"/>
              <result column="status" property="status" jdbcType="INTEGER"/>
          </resultMap>
      
          <insert id="create">
              insert into t_order (id,user_id,product_id,count,money,status)
              values (null,#{userId},#{productId},#{count},#{money},0);
          </insert>
      
      
          <update id="update">
              update t_order set status = 1
              where user_id=#{userId} and status = #{status};
          </update>
      
      </mapper>
      
      
      
      
      ```
      
   6. **controller层**

      ```java
      @RestController
      public class OrderController {
      
          @Resource
          private OrderService orderService;
      
          @GetMapping(value = "/order/create")
          public CommonResult create(Order order){
              System.out.println(order);
              orderService.create(order);
              System.out.println("---------order");
              return new CommonResult(200,"订单创建成功");
          }
      }
      
      ```
      
      
      
   7. **entity类(也叫domain类)**

      ```java
      @Data
      @AllArgsConstructor
      @NoArgsConstructor
      public class CommonResult<T>
      {
          private Integer code;
          private String  message;
          private T       data;
      
          public CommonResult(Integer code, String message)
          {
              this(code,message,null);
          }
      }
      
      ```

      ```java
      
      @Data
      @AllArgsConstructor
      @NoArgsConstructor
      public class Order
      {
          private Long id;
      
          private Long userId;
      
          private Long productId;
      
          private Integer count;
      
          private BigDecimal money;
      
          private Integer status; //订单状态：0：创建中；1：已完结
      }
      
      
      ```

      

       

       

      

   8. config配置类

      ```java
      @Configuration
      @MapperScan({"com.atlgq.springcloud.alibaba.dao"})
      public class MyBatisConfig {
      
      }
      
      ```
      
      ```java
      
      ```

   @Configuration
      public class DataSourceProxyConfig {


          @Value("${mybatis.mapperLocations}")
          private String mapperLocations;

      

          @Bean
          @ConfigurationProperties(prefix = "spring.datasource")
          public DataSource druidDataSource(){
              return new DruidDataSource();
          }

      

          @Bean
          public DataSourceProxy dataSourceProxy(DataSource dataSource) {
              return new DataSourceProxy(dataSource);
          }

      

          @Bean
          public SqlSessionFactory sqlSessionFactoryBean(DataSourceProxy dataSourceProxy) throws Exception {
              SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
              sqlSessionFactoryBean.setDataSource(dataSourceProxy);
              sqlSessionFactoryBean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources(mapperLocations));
              sqlSessionFactoryBean.setTransactionFactory(new SpringManagedTransactionFactory());
              return sqlSessionFactoryBean.getObject();
          }

      }
      ```
      
   
   
   
   
   
   
   
   
   
     ==库存==,
   
   # **seata-order-service2002**
   
   ![image-20201112212745743](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201112212745743.png)
   
   
   
   1.    pom   
   
   ​```xml
   <dependencies>
           <!--nacos-->
           <dependency>
               <groupId>com.alibaba.cloud</groupId>
               <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
           </dependency>
           <!--seata-->
           <dependency>
               <groupId>com.alibaba.cloud</groupId>
               <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
               <exclusions>
                   <exclusion>
                       <artifactId>seata-all</artifactId>
                       <groupId>io.seata</groupId>
                   </exclusion>
               </exclusions>
           </dependency>
           <dependency>
               <groupId>io.seata</groupId>
               <artifactId>seata-all</artifactId>
               <version>0.9.0</version>
           </dependency>
           <!--feign-->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-openfeign</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
           <dependency>
               <groupId>org.mybatis.spring.boot</groupId>
               <artifactId>mybatis-spring-boot-starter</artifactId>
               <version>2.0.0</version>
           </dependency>
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>5.1.37</version>
           </dependency>
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>druid-spring-boot-starter</artifactId>
               <version>1.1.10</version>
           </dependency>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <optional>true</optional>
           </dependency>
       </dependencies>
      ```

   

   1. 配置文件

      ```yaml
      server:
        port: 2002
      
      spring:
        application:
          name: seata-storage-service
        cloud:
          alibaba:
            seata:
              tx-service-group: fsp_tx_group
          nacos:
            discovery:
              server-addr: localhost:8848
        datasource:
          driver-class-name: com.mysql.jdbc.Driver
          url: jdbc:mysql://localhost:3306/seata_storage
          username: root
          password: 123456
      
      logging:
        level:
          io:
            seata: info
      
      mybatis:
        mapperLocations: classpath:mapper/*.xml
      
      
      
      
      ```

      

   2. 主启动类

      ```java
      @SpringBootApplication(exclude = DataSourceAutoConfiguration.class)
      @EnableDiscoveryClient
      @EnableFeignClients
      public class SeataStorageServiceApplication2002
      {
          public static void main(String[] args)
          {
              SpringApplication.run(SeataStorageServiceApplication2002.class, args);
          }
      }
      ```

      

   3. service层

      ```java
      public interface StorageService {
      
          // 扣减库存
          void decrease(Long productId, Integer count);
          void insert();
      }
      
      
      ```

      ```java
      
      @Service
      public class StorageServiceImpl implements StorageService {
      
          private static final Logger LOGGER = LoggerFactory.getLogger(StorageServiceImpl.class);
      
          @Resource
          private StorageDao storageDao;
      
          // 扣减库存
          @Override
          public void decrease(Long productId, Integer count) {
              LOGGER.info("------->storage-service中扣减库存开始");
              storageDao.decrease(productId,count);
              LOGGER.info("------->storage-service中扣减库存结束");
          }
      
          @Override
          public void insert() {
              storageDao.insert();;
          }
      }
      
      
      ```

      

   4. dao层

      ```java
      @Mapper
      public interface StorageDao {
      
      
          //扣减库存信息
          void decrease(@Param("productId") Long productId, @Param("count") Integer count);
          void insert();
      }
      ```

      

   5. controller层

      ```java
      
      @RestController
      public class StorageController {
      
          @Autowired
          private StorageService storageService;
      
      
          //扣减库存
          @RequestMapping("/storage/decrease")
          public CommonResult decrease(Long productId, Integer count) {
              System.out.println("productId= " + productId + "count = " + count);
              storageService.decrease(productId, count);
              return new CommonResult(200,"扣减库存成功！");
          }
      
          @RequestMapping("/storage/insert")
          public CommonResult insert() {
              System.out.println("productId= " +   "count = "  );
              storageService.insert();
              return new CommonResult(200,"增加层高！");
          }
      }
      ```

      domain

      ```java
      
      @Data
      public class Storage {
      
          private Long id;
      
          // 产品id
          private Long productId;
      
          //总库存
          private Integer total;
      
      
          //已用库存
          private Integer used;
      
      
          //剩余库存
          private Integer residue;
      }
      
      ```

      ```java
      @Data
      @AllArgsConstructor
      @NoArgsConstructor
      public class CommonResult<T>
      {
          private Integer code;
          private String  message;
          private T       data;
      
          public CommonResult(Integer code, String message)
          {
              this(code,message,null);
          }
      }
      ```

      

       **config的和2001一样**

      

    ==账号==,

   # **seata-order-service2003**

   

   ![image-20201112212820647](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201112212820647.png)

   

   1. pom     

      ```xml
      <dependencies>
              <!--nacos-->
              <dependency>
                  <groupId>com.alibaba.cloud</groupId>
                  <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
              </dependency>
              <!--seata-->
              <dependency>
                  <groupId>com.alibaba.cloud</groupId>
                  <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
                  <exclusions>
                      <exclusion>
                          <artifactId>seata-all</artifactId>
                          <groupId>io.seata</groupId>
                      </exclusion>
                  </exclusions>
              </dependency>
              <dependency>
                  <groupId>io.seata</groupId>
                  <artifactId>seata-all</artifactId>
                  <version>0.9.0</version>
              </dependency>
              <!--feign-->
              <dependency>
                  <groupId>org.springframework.cloud</groupId>
                  <artifactId>spring-cloud-starter-openfeign</artifactId>
              </dependency>
              <dependency>
                  <groupId>org.springframework.boot</groupId>
                  <artifactId>spring-boot-starter-web</artifactId>
              </dependency>
              <dependency>
                  <groupId>org.springframework.boot</groupId>
                  <artifactId>spring-boot-starter-test</artifactId>
                  <scope>test</scope>
              </dependency>
              <dependency>
                  <groupId>org.mybatis.spring.boot</groupId>
                  <artifactId>mybatis-spring-boot-starter</artifactId>
                  <version>2.0.0</version>
              </dependency>
              <dependency>
                  <groupId>mysql</groupId>
                  <artifactId>mysql-connector-java</artifactId>
                  <version>5.1.37</version>
              </dependency>
              <dependency>
                  <groupId>com.alibaba</groupId>
                  <artifactId>druid-spring-boot-starter</artifactId>
                  <version>1.1.10</version>
              </dependency>
              <dependency>
                  <groupId>org.projectlombok</groupId>
                  <artifactId>lombok</artifactId>
                  <optional>true</optional>
              </dependency>
          </dependencies>
      
      ```

      

   2. 配置文件

      ```yaml
      server:
        port: 2003
      
      spring:
        application:
          name: seata-account-service
        cloud:
          alibaba:
            seata:
              tx-service-group: fsp_tx_group
          nacos:
            discovery:
              server-addr: localhost:8848
        datasource:
          driver-class-name: com.mysql.jdbc.Driver
          url: jdbc:mysql://localhost:3306/seata_account
          username: root
          password: 123456
      
      feign:
        hystrix:
          enabled: false
      
      logging:
        level:
          io:
            seata: info
      
      mybatis:
        mapperLocations: classpath:mapper/*.xml
      
      
      
      
      ```

      

   3. 主启动类

      ```java
      @SpringBootApplication(exclude = DataSourceAutoConfiguration.class)
      @EnableDiscoveryClient
      @EnableFeignClients
      public class SeataAccountMainApp2003
      {
          public static void main(String[] args)
          {
              SpringApplication.run(SeataAccountMainApp2003.class, args);
          }
      }
      ```

      

   4. service层

      ```java
      public interface AccountService {
      
          /**
           * 扣减账户余额
           */
          void decrease(@RequestParam("userId") Long userId, @RequestParam("money") BigDecimal money);
      }
      
      ```

      ```java
      
      /**
       * 账户业务实现类
       */
      @Service
      public class AccountServiceImpl implements AccountService {
      
          private static final Logger LOGGER = LoggerFactory.getLogger(AccountServiceImpl.class);
      
          @Resource
          private AccountDao accountDao;
      
          /**
           * 扣减账户余额
           */
          @Override
          public void decrease(Long userId, BigDecimal money) {
      
              LOGGER.info("------->account-service中扣减账户余额开始");
              try { TimeUnit.SECONDS.sleep(0); } catch (InterruptedException e) { e.printStackTrace(); }
              accountDao.decrease(userId,money);
              LOGGER.info("------->account-service中扣减账户余额结束");
          }
      }
      
      ```

      

   5. dao层

      ```java
      
      @Mapper
      public interface AccountDao {
      
          /**
           * 扣减账户余额
           */
          void decrease(@Param("userId") Long userId, @Param("money") BigDecimal money);
      }
      
      
      ```

      

   6. controller层

      ```java
      @RestController
      public class AccountController {
      
          @Resource
          private AccountService accountService;
      
          /**
           * 扣减账户余额
           */
          @PostMapping("/account/decrease")
          public CommonResult decrease(@RequestParam("userId") Long userId, @RequestParam("money") BigDecimal money){
              System.out.println("userid =" + userId + "money =" + money);
              accountService.decrease(userId,money);
              return new CommonResult(200,"扣减账户余额成功！");
          }
      }
      ```

      domain

      ```java
      
      @Data
      @AllArgsConstructor
      @NoArgsConstructor
      public class Account {
      
          private Long id;
      
          /**
           * 用户id
           */
          private Long userId;
      
          /**
           * 总额度
           */
          private BigDecimal total;
      
          /**
           * 已用额度
           */
          private BigDecimal used;
      
          /**
           * 剩余额度
           */
          private BigDecimal residue;
      }
      
      ```

      ```java
      
      @Data
      @AllArgsConstructor
      @NoArgsConstructor
      public class CommonResult<T>
      {
          private Integer code;
          private String  message;
          private T       data;
      
          public CommonResult(Integer code, String message)
          {
              this(code,message,null);
          }
      }
      ```

      config和2001一样

      

   7. 

   8. 

5.   **全局创建完成后,首先测试不加seata**

     

     ![seala的14](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的14.png)

     ![seala的13](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的13.png)

​    

​    

​     

6. 使用seata:

   **在==订单模块==的serviceImpl类中的==create方法==添加启动分布式事务的注解**

   ```java
   
   @Service
   @Slf4j
   public class OrderServiceImpl implements OrderService {
   
       @Resource
       private OrderDao orderDao;
       @Resource
       private StorageService storageService;
       @Resource
       private AccountService accountService;
   
       @Override
       @GlobalTransactional(name = "fsp-create-order",rollbackFor = Exception.class)
       public void create(Order order) {
           log.info("----------开始创建订单");
           orderDao.create(order);
           log.info("---------订单开始调用库存，做扣减");
           storageService.decrease(order.getProductId(),order.getCount());
           log.info("--------------订单微服务开始调用账户，做扣减,userID = " + order.getUserId() + "money = " + order.getMoney() );
           accountService.decrease(order.getUserId(),order.getMoney());
           log.info("--------------订单微服务开始调用账户，做扣减完成");
   
           //修改订单状态
           log.info("----------开始修改订单");
           orderDao.update(order.getUserId(),0);
           log.info("----------结束修改订单");
   
           log.info("----------订单结束了");
   
       }
   }
   
   ```

    

7.   此时在测试

    发现,发生异常后,直接回滚了,前面的修改操作都回滚了，不会修改数据库任何值

 



### setat原理:

![seala的15](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的15.png)

![seala的16](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的16.png)



**seata提供了四个模式:**

![seala的17](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的17.png)

![seala的18](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的18.png)



==第一阶段:==

![seala的20](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的20.png)

![seala的19](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的19.png)



==二阶段之提交==:

![seala的21](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的21.png)



==二阶段之回滚:==

![seala的22](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的22.png)

![seala的23](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的23.png)





==断点==:

![seala的24](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的24.png)

**可以看到,他们的xid全局事务id是一样的,证明他们在一个事务下**

![seala的25](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的25.png)

**before 和 after的原理就是**

![seala的26](D:\Desktop\studyDocument\springcloud下载的笔记\图片\seala的26.png)

**在更新数据之前,先解析这个更新sql,然后查询要更新的数据,进行保存**

访问的话需要开启nacos和seata

```java
localhost:8848/nacos/
```

```java
在seata的bin文件下面.bat
```



```java
http://localhost:2001/order/create?userId=1&productId=1&count=10&money=100
```



```java
http://localhost:2002/storage/decrease?productId=1&count=10
```



```java
http://localhost:2003/account/decrease?userId=1&money=123
```

# **####################高级篇分割线############################**



























































































































































