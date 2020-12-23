# ###########零基础分割线###########



# 2,服务注册与发现



## 6,Eureka:

> 前面我们没有服务注册中心,也可以服务间调用,为什么还要服务注册?
>
> 当服务很多时,单靠代码手动管理是很麻烦的,需要一个公共组件,统一管理多服务,包括服务是否正常运行,等
>
> Eureka用于**==服务注册==**,目前官网**已经停止更新**

### **单机版eureka:**

#### **1,创建项目**

```java
cloud_eureka_server_7001
```

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

## 比如此时pay模块加入eureka:

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



##### 3,添加修改配置文件:

```yml
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



##### 4,pay8001模块重启,就可以注册到eureka中了

```yml
eureka:
  instance:
    hostname: localhost #eureka服务端的实例名称
很重要，是入住服务中心的名称，要统一
```

![image-20201219214228584](Images\image-20201219214228584.png)

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

> 1,就是pay模块启动时,注册自己,并且自身信息也放入eureka
> 2.order模块,首先也注册自己,放入信息,当要调用pay时,先从eureka拿到pay的调用地址
> 3.通过HttpClient调用
>  	并且还会缓存一份到本地,每30秒更新一次

**集群构建原理:**

​		互相注册,互相观望

#### **构建新erueka项目**

名字:

```java
cloud_eureka_server_7002
```

##### 1,pom文件:

​		粘贴7001的即

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



##### 2,配置文件:

​		在写配置文件前,修改一下主机的hosts文件（）

![image-20201214160252398](Images\image-20201214160252398.png)

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

相互守望，相互注册

![image-20201214160309458](Images\image-20201214160309458.png)



#### 将pay,order模块注册到eureka集群中:

##### 1,只需要修改配置文件即可:

```yml
	service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
      #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址
```

##### 2,两个模块都修改上面的都一样即可

​			然后启动两个模块

​			要先启动7001,7002,然后是pay模块8001,然后是order(80)



### 微服务集群配置

#### 0,创建新模块,8002

​	名称:

```java
 cloud_payment_8002
```

#### 1,pom文件,复制8001的

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

#### 2,配置文件复制8001的

​		端口修改一下,改为8002

​		服务名称不用改,用一样的



```yaml
server:
  port: 8002

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

eureka:
  client:
    register-with-eureka: true
    #表示向服务总心注册自己
    fetch-registry: true
    #true表示从eurekaServer注册中心抓取已有信息，默认为true，单节点是无所谓的，集群必须设置为true的话，才能够配合ribbon使用负载均衡
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
      #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址
  instance:
    instance-id: payment8002
    prefer-ip-address: true

mybatis:
  mapper-locations: classpath*:mapper/*.xml
  type-aliases-package: com.atlgq.springcloud.entities
```

#### 3.主启动类,复制8001的名称改为8002

#### 4,mapper,service,controller都复制一份

> 在controller中获取到端口号，然后
>
> 可以用来看访问的是哪一个端口
>
> ​		然后就启动服务即可
>
> ​		此时访问order模块,发现并没有负载均衡到两个pay,模块中,而是只访问8001
>
> ​		虽然我们是使用RestTemplate访问的微服务,但是也可以负载均衡的

#### 5、负载均衡，是加在order的层面上的

**注意这样还不可以,需要让RestTemplate开启负载均衡注解,还可以指定负载均衡算法,默认轮询**

因为我们在这里把OrderMain里面的数据写死，所以无法负载均衡，要修改

```java
//    private  static final String PAYMENT_URL="http://localhost:8001";
    private  static final String PAYMENT_URL="http://CLOUD-PAYMENT-SERVICE";
```

还要在config中添加@LoadBalanced，这步的目的是把RestTemplate注入到容器中可以使用

![image-20201214160453714](Images\image-20201214160453714.png)

​						如果Ribbon和Eureka整合后，Consumer可以直接调用服务而不用关心地址和端口号，且该服务还有负载功能了。



### ############################2020年11月6日##################################





### 4,修改服务主机名和ip在eureka的web上显示

比如修改pay_8001模块，修改服务名称

#### 1,修改配置文件:instance是和client对齐的

```yml
instance:
    instance-id: payment8001
```

![image-20201214160706669](Images\image-20201214160706669.png)

```java
http://localhost:8002/actuator/health
用来检查服务是否成功挂载
```

#### 2、显示ip地址

![image-20201219214713223](Images\image-20201219214713223.png)



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

![image-20201219214758639](Images\image-20201219214758639.png)





### 6,Eureka自我保护:

​	

- Eureka自我保护的产生原因：

> Eureka在运行期间会统计心跳失败的比例，在15分钟内是否低于85%,如果出现了低于的情况，Eureka Server会将当前的实例注册信息保护起来，同时提示一个警告，一旦进入保护模式，Eureka Server将会尝试保护其服务注册表中的信息，不再删除服务注册表中的数据。也就是不会注销任何微服务。因为可能是因为网络延迟的原因导致没有规定时间内发送心跳，eureka为了保护微服务，并不会马上把这个微服务给清除出去，属于CAP的AP分支



#### 	1、怎么禁止自我保护

**eureka服务端配置:**		**设置接受心跳时间间隔**

```yaml
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
  server:
    enable-self-preservation: false #这是出厂默认开启保护机制，这次为false是关闭保护机制
    eviction-interval-timer-in-ms: 2000 #设置心跳时间间隔，如果这段时间内没有发送心跳，那么移除这个服务
```



修改成功后访问http://eureka7001.com:7001/会出现

![image-20201219214942383](Images\image-20201219214942383.png)

![image-20201219214956462](Images\image-20201219214956462.png)





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

> 我们在zk上注册的node是临时节点,当我们的服务一定时间内没有发送心跳
>   	那么zk就会`将这个服务的node删除了



**这里测试,就不写service与dao什么的了**





### 3,创建order消费模块注册到zk

#### 1,创建项目

```bash
名字: cloud_order_zk_80
```



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

![image-20201219215807029](Images\image-20201219215807029.png)

这个connect-string指定多个zk地址即可，中间用逗号隔开，跟eureka一样

connect-string: 192.128.123.124,192.138.124.115







## 8,Consul:



### 1,按照consul

需要下载一个安装包

官网搜索下载

启动是一个命令行界面,需要输入consul agen-dev启动，这是启动视频的网址

```java
https://learn.hashicorp.com/tutorials/consul/get-started-install
```

在当前目录下键入cmd，输入consul如果有输出就代表正确

![image-20201219220255203](Images\image-20201219220255203.png)

在cmd输入

```bash
consul agent -dev  #可以启动服务，开发模式启动
```

```bash
localhost:8500   #可以访问
```

### 2,创建新的pay模块,8006

#### 1,项目名字

```java
cloud-providerconsule-payment8006
```

#### 2,pom依赖

```xml

    <dependencies>
        
          <!--SpringCloud consul-server-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
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

```java
@SpringBootApplication
@EnableDiscoveryClient
public class PaymentMain8006 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8006.class,args);
    }
}

```



#### 5,controller

```java
@RestController
@Slf4j
public
class PaymentController {

    @Value("${server.port}")
    private String serverPort;

    @GetMapping(value = "/payment/consul")
    public String paymentzk(){
        return "springcloud with consul : " + serverPort + "\t" + UUID.randomUUID().toString();
    }
}
```



#### 6,启动服务，先开启consul

```java
consul agent -dev
```



### 3,创建新order模块order80

```java
cloud-comsumerconsul-order80
```

#### 1,pom文件(和payment一样)

```xml

    <dependencies> 
          <!--SpringCloud consul-server-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
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

#### 4,使用Ribbon，RestTemplate注册，配种Bean

配置类注册

```java

@Configuration
public class ApplicationContextConfig {
    @Bean
//    @LoadBalanced //使用 @LoadBalanced注解赋予RestTemplate负载均衡的能力
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}

```



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



#### 6,启动服务,测试，访问

```bash
localhost:8500
```

![image-20201219220843735](Images\image-20201219220843735.png)





## 9,三个注册中心的异同:

> CAP:CAP理论关注的是数据，而不是整体系统的设计，三者只能满足两者
>
> C:Consistency（强一致性）
>
> A:Availability（可用性）
>
> P:Partition tolerance（分区容错性）



# 3,服务调用



## 10,Ribbon负载均衡:

**Ribbon目前也进入维护,基本上不准备更新了**

![image-20201219222710404](Images\image-20201219222710404.png)

**进程内LB(本地负载均衡)**

![image-20201219222729585](Images\image-20201219222729585.png)



**集中式LB(服务端负载均衡)**

**区别**

**Ribbon就是负载均衡+RestTemplate**

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
	//这是手动配置是ribbon的
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

> RestTemplate的:
> 	xxxForObject()方法,返回的是响应体中的数据
>     xxxForEntity()方法.返回的是entity对象,这个对象不仅仅包含响应体数据,还包含响应体信息(状态码等)





#### Ribbon常用负载均衡算法:

**IRule接口,Riboon使用该接口,根据特定算法从所有服务中,选择一个服务,**

**Rule接口有7个实现类,每个实现类代表一个负载均衡算法**

#### 负载均衡算法

![image-20201219223005036](Images\image-20201219223005036.png)





#### 使用Ribbon:

> **==这里使用eureka的那一套服务，在服务中心为Eureka的orader80包下操作==**
>
> **==也就是不能放在主启动类所在的包及子包下，官网里面说不能放在@ComponentScan扫描的包下面，就是不能放在主启动类的包里面，所以我们就新建一个包com.atlgq.myrule==**

##### 1,修改order80模块

##### 2,额外创建一个包，包下有一个MyRule的类，配置类

![image-20201219223152097](Images\image-20201219223152097.png)

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

##### 4,在主启动类上加一个注解@RibbonClient(name = "CLOUD-PAYMENT-SERVICE",configuration = MySelRule.class)，

##### name指名自己要访问的服务，然后configuration指定的是自己自定义的轮转规则，默认的负载均衡算法是轮转

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

**表示,访问CLOUD_PAYMENT_SERVICE的服务时,使用我们自定义的负载均衡算法**

测试，端口号是随即出现的

```java
访问：http://localhost/consumer/payment/get/1 
```





#### 手写负载均衡算法:

##### 1,ribbon的轮询算法原理

![image-20201219223343075](Images\image-20201219223343075.png)

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

在eureka的80的config下去掉，去掉自动配置的负载均衡，我们要自己手写一个

@LoadBalanced



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

    //这个方法很重要，所以直接写死，在这里获取访问的次数并且返回
    private final int getAndIncrement(){
        int current;
        int next;
        do{
            //获取当前访问的次数,为0
            current = atomicInteger.get();
            //如果next大于整数的最大值，那么为0，否则为current + 1，此时next为1，即第一次，因为next记录的是我们要访问的次数，这个次数是一直往上递增的
            next = current > Integer.MAX_VALUE ? 0 : current + 1;
            //此时current为0，next为1，atomicInter.get()与current比较值相同，所以把current设置为1，返回true,如果数值被修改了，那么返回false，这是一种类似于锁的操作，所以用!true跳出循环
            //此时return 1 ， 下一次以此类推，
            //这是一个CAP算法，保证了事务性，比较current和this.atomicInteger的值，如果值相等的话，那么就为真，把next的值赋给current，就是我们现在访问的次数。(!this.atomicInteger.compareAndSet(current,next))的值为false，这个时候就会跳出循环。如果这个时候如果别的数据访问过来，current的值就会被修改，那么是不会跳出这个循环，重新把current赋值0。这样就保证了操作的原子性。
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



# #################2020.11.07###############





## 11,OpenFeign

![image-20201219224854312](Images\image-20201219224854312.png)

**是一个声明式的web客户端,只需要创建一个接口,添加注解即可完成微服务之间的调用**

![image-20201219224822130](Images\image-20201219224822130.png)

**就是A要调用B,Feign就是在A中创建一个一模一样的B对外提供服务的的接口,我们调用这个接口,就可以服务到B**



### **Feign与OpenFeign区别**

![image-20201219224805515](Images\image-20201219224805515.png)



### 使用OpenFeign

```java
之前的服务间调用,我们使用的是ribbon+RestTemplate
		现在改为使用Feign
```

#### 1,新建一个order项目,用于feign测试

```java
cloud-consumer-feign-order80
```

#### 2,pom文件

```xml

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

```

#### 3,配置文件yaml

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

#### 5,fegin需要调用的其他的服务的接口，新建service包，里面是接口

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

![image-20201219225231081](Images\image-20201219225231081.png)



**Feign默认使用ribbon实现负载均衡**



### OpenFeign超时机制:

> ==OpenFeign默认等待时间是1秒,超过1秒,直接报错==

#### 1,设置超时时间,修改配置文件:

因为OpenFeign的底层是ribbon进行负载均衡,所以它的超时时间是由ribbon控制

##### 	1、现在controller里面写一个超时的访问，8001

这个时候是访问不到数据的，因为我们的访问等待时间太久了。

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

##### 	2、此时访问是出错的，修改cloud-consumerfegin-order80的yml文件

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

![image-20201219225915879](Images\image-20201219225915879.png)



**OpenFeign的日志级别有:**
![image-20201219225906742](Images\image-20201219225906742.png)





#### 	1,使用OpenFeign的日志:

order-consumer-openfeign-order80中添加OpenFeign的日志类

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

![image-20201219231344589](Images\image-20201219231344589.png)



#### ######################基础分割线#####################