*##############中级篇分割线######################**





# 10,Spring CloudAlibaba:

![image-20201220200433909](Images\image-20201220200433909.png)

> 之所以有Spring CloudAlibaba,是因为Spring Cloud Netflix项目进入维护模式，也就是,就不是不更新了,不会开发新组件了，所以,某些组件都有代替版了,比如Ribbon由Loadbalancer代替,等等
>

==支持的功能==

![image-20201220200421249](Images\image-20201220200421249.png)

几乎可以将之前的Spring Cloud代替



==具体组件==:

![image-20201220200619940](Images\image-20201220200619940.png)



## Nacos:

**服务注册和配置中心的组合**

> Nacos=erueka+config+bus

### 安装Nacos:

需要java8  和 Mavne

**1,到github上下载安装包**,Nacos官方文档，百度网盘有

```java
Nacos下载地址，手册
https://nacos.io/zh-cn/index.html
Nacos官方文档
https://spring-cloud-alibaba-group.github.io/github-pages/greenwich/spring-cloud-alibaba.html#_spring_cloud_alibaba_nacos_discovery
```

​		解压安装包

**2,启动Nacos**

> ​	在bin下,进入cmd
>
> ​	./startup.cmd
>
> ​	如果安装失败，那么尝试换一下jdk的版本，我就是版本出问题

**3,访问Nacos**

> ​		Nacos默认监听8848
>
> ```bash
> localhost:8848/nacos
> ```
>
> ​		账号密码:默认都是nacos





### 使用Nacos:

新建pay模块

​		**现在不需要额外的服务注册模块了,Nacos单独启动了**

```java
cloudalibaba-provider-payment9001
```

#### 1,pom

父项目管理alibaba的依赖:,添加

父项目的pom

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



9001的pom

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

![image-20201220205317731](Images\image-20201220205317731.png)





### 创建其他Pay模块

​		额外在创建

```java
cloudalibaba-provider-payment9002
```

​		直接复制上面的即可

### 创建order模块

```java
cloudalibaba-consumer-order83
```

#### 1,pom

> **为什么Nacos支持负载均衡?**
>
> ​				Nacos直接集成了Ribon,所以有负载均衡

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



> **这个server-url的作用是,我们在controller,需要使用RestTempalte远程调用9001,**
>
> ​		**这里是指定9001的地址**



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

​	因为Naocs要使用Ribbon进行负载均衡,那么就需要使用RestTemplate，新建立一个config的包

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

启动83访问9001,9002,可以看到,实现了负载均衡

![image-20201220205756567](Images\image-20201220205756567.png)

![image-20201220205843081](Images\image-20201220205843081.png)

![image-20201220205851185](Images\image-20201220205851185.png)

![image-20201220210142267](Images\image-20201220210142267.png)

![image-20201220210152562](Images\image-20201220210152562.png)





### Nacos与其他服务注册的对比

Nacos它既可以支持CP,也可以支持AP,可以切换

![image-20201220210127300](Images\image-20201220210127300.png)

==下面这个curl命令,就是切换模式==

![image-20201220210110780](Images\image-20201220210110780.png)

```bash
#需要在环境变量配置$NACOS_SERVER
curl -X PUT "$NACOS_SERVER:8848/nacos/v1/ns/operator/switches?entry=serverMode&value=CP"
#使用localhost
curl -X PUT "localhost:8848/nacos/v1/ns/operator/switches?entry=serverMode&value=CP"
```

![image-20201220210807778](Images\image-20201220210807778.png)

### 使用Nacos作为配置中心:

![image-20201220210953450](Images\image-20201220210953450.png)







**==需要创建配置中心的客户端模块==**

```java
cloudalibaba-config-nacos-client3377
```

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

> 可以看到,这里也添加了@RefreshScope，不能和Resource一起使用
> 之前在Config配置中心,也是添加这个注解实现动态刷新的	

#### 5、在nacos添加注册文件

```bash
nacos-config-client-dev.yaml
```

![image-20201220212015021](Images\image-20201220212015021.png)



#### 6、测试，启动3377

访问,可以实现实时动态刷新，只要nacos上面有修改，那么3377也会修改

```java
http://localhost:3377/config/info
```

![image-20201220212104133](Images\image-20201220212104133.png)







#### 5,在Nacos添加配置信息:

==**Nacos的配置规则:**==

**配置规则,就是我们在客户端如何指定读取配置文件,配置文件的命名的规则**

默认的命名方式:

> prefix:
> 		默认就是当前服务的服务名称
>  		也可以通过spring.cloud.necos.config.prefix配置
> spring.profile.active:
> 		就是我们在application.yml中指定的,当前是开发环境还是测试等环境
>     这个可以不配置,如果不配置,那么前面的 -  也会没有
> file-extension
>      就是当前文件的格式(后缀),目前只支持yml和properties

==在web UI上创建配置文件:==

注意,DataId就是配置文件名字:

​		名字一定要按照上面的==规则==命名,否则客户端会读取不到配置文件



#### 6,注意默认就开启了自动刷新

> 此时我们修改了配置文件
>
> 客户端是可以立即更新的
>
> ​			因为Nacos支持Bus总线,会自动发送命令更新所有客户端





### Nacos配置中心之分类配置:



![image-20201220212648301](Images\image-20201220212648301.png)

![image-20201220212639565](Images\image-20201220212639565.png)





NameSpace默认有一个:public名称空间

这三个类似java的: 包名 + 类名 + 方法名

![image-20201220212631288](Images\image-20201220212631288.png)





#### 1,配置不同DataId:

修改配置中心3377下的application.yaml里面的

```yaml
spring:
  profiles:
    active: dev #表示这是开发版本
```

![image-20201220212853417](Images\image-20201220212853417.png)

会和我们bootstrap.yaml文件中的name拼接在

```bash
nacos-config-client-dev.yaml
```



根据拼接规则会有不同的访问结果

​	==通过配置文件,实现多环境的读取:==

新建一个test的配置文件

![image-20201220213052521](Images\image-20201220213052521.png)

> 此时,改为dev,就会读取dev的配置文件,改为test,就会读取test的配置文件

#### 2,配置不同的GroupID:

直接在新建配置文件时指定组，在config下面增加group的分组，指定要访问的分组

![image-20201220213244428](Images\image-20201220213244428.png)



==在客户端配置,使用指定组的配置文件:==

![image-20201220213439362](Images\image-20201220213439362.png)

**这两个配置文件都要修改**

![image-20201220213457370](Images\image-20201220213457370.png)

​	

重启服务,即可

访问：

```java
http://localhost:3377/config/info
```

![image-20201220213540658](Images\image-20201220213540658.png)



#### 配置不同的namespace:

![image-20201220213705908](Images\image-20201220213705908.png)

![image-20201220213746171](Images\image-20201220213746171.png)

==客户端配置使用不同名称空间:==

![image-20201220214131630](Images\image-20201220214131630.png)

![image-20201220214241674](Images\image-20201220214241674.png)

![image-20201220214301371](Images\image-20201220214301371.png)



**要通过命名空间id指定**

访问

```java
http://localhost:3377/config/info
```

![image-20201220215907722](Images\image-20201220215907722.png)

# #####################2020.11.10#######################





### Nacos集群和持久化配置:

![image-20201220223040691](Images\image-20201220223040691.png)

> nacos一定要配置集群，三台以上。
>
> Nacos默认有自带嵌入式数据库,derby,但是如果做集群模式的话,就不能使用自己的数据库,不然每个节点一个数据库,那么数据就不统一了,需要使用外部的mysql**
>



#### 1,windows环境下，单机版,切换mysql数据库:

##### 1、导入sql脚本

> 将nacos切换到使用我们自己的mysql数据库
>
> nacos默认自带了一个sql文件,在nacos安装目录下conf里面，nacos-mysql.sql
>
> 将它放到我们的mysql执行,先创建一个数据库

```sql
create database nacos_config;
```

登录mysql，执行，在刚建好的数据库里面

```sql
source D:\Program Files\Program Files\SpringCloud\nacos-server-1.2.1\nacos\conf/nacos-mysql.sql
```

![image-20201220223323474](Images\image-20201220223323474.png)

##### 2,修改配置文件

> Nacos安装目录下的conf/application.properties,添加:

```properties
####################################2020.12.20 nacos的的derby转移到mysql#############
spring.datasource.platform=mysql

db.num=1
db.url.0=jdbc:mysql://127.0.0.1:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=123456
```



##### 3,此时可以重启nacos

那么就会改为使用我们自己的mysql

![image-20201220224308743](Images\image-20201220224308743.png)

在数据库当中可以查询到我们的配置

![image-20201220224259758](Images\image-20201220224259758.png)





#### Linux上配置Nacos集群+Mysql数据库

==官方架构图:==

![image-20201220224547180](Images\image-20201220224547180.png)

**需要一个Nginx作为VIP**



##### 1,下载安装Nacos的Linux版安装包

> linux下启动后默认为集群模式，要通过**nginx代理才能进到nacos的登录界面**。
> 默认启动直接进入到解压后的bin目录执行，不走代理的话，启动时添加参数，就是进到解压后的bin目录中，单机版，执行，登录用户名：nacos 密码：nacos

单机启动nacos

```java
bash startup.sh -m standalone
```

Nacos安装包在我百度网盘，解压在linux的usr/local/springcloud/nacos里面

```java
tar -zvxf 文件名
```

安装好mysql

> 配置好相关的账号密码root 123456 , mysql 5.7

创建数据库

```java
create database nacos_config;
```

##### 2,进入安装目录,现在执行自带的sql文件

​			进入mysql,执行sql文件，使用刚创建好的数据库

```java
source /home/ubuntu/usr/local/springcloud/nacos/conf/nacos-mysql.sql
```

![image-20201220225421703](Images\image-20201220225421703.png)

##### 3.修改配置文件,切换为我们的mysql/conf文件下的

```bash
application.properties
```



​			就是上面windos版要修改的几个属性

```java
####################################2020.11.11 nacos的的derby转移到mysql#############
spring.datasource.platform=mysql

db.num=1
db.url.0=jdbc:mysql://127.0.0.1:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=123456
```



##### 4,修改cluster.conf

```bash
/home/ubuntu/usr/local/springcloud/nacos/conf
```



指定哪几个节点是Nacos集群，要先创建一个文件出来

```java
cp cluster.conf.example cluster.conf
vim cluster.conf
```

> ​	这里使用3333,4444,5555作为三个Nacos节点监听的端口，通过
>
> ```bash
> hostname -I
> ```
>
> 可以获取192.168.162.128，每台机子的都不一样

```java
192.168.162.128:3333
192.168.162.128:4444
192.168.162.128:5555
```

![image-20201221094213856](Images\image-20201221094213856.png)

![image-20201221094240992](Images\image-20201221094240992.png)





##### 5,修改startup.sh

> ​	我们这里就不配置在不同节点上了,就放在一个节点上，修改startup.sh，先复制一份备份.bk
>
> ​	既然要在一个节点上启动不同Nacos实例,就要修改startup.sh,使其根据不同端口启动不同Nacos实例
>
> ```bash
> /home/ubuntu/usr/local/springcloud/nacos/bin/startup.sh
> ```

> 修改startup.sh，增加p，修改nohup
>
> ```sh
> while getopts ":m:f:s:p:" opt
> do
>     case $opt in
>         m)  
>             MODE=$OPTARG;;
>         f)  
>             FUNCTION_MODE=$OPTARG;; 
>         s) 
>             SERVER=$OPTARG;;
>         p)
>                 PORT=$OPTARG;;
>         ?)
>         echo "Unknown parameter"
>         exit 1;;
>     esac
> done
> ```
>
> ![image-20201220231806995](Images\image-20201220231806995.png)
>
> 修改另一部分
>
> ```sh
> nohup $JAVA -Dserver.port=${PORT}${JAVA_OPT} nacos.nacos >> ${BASE_DIR}/logs/start.out 2>&1 &
> ```
>
> 
>
> ![image-20201220231322371](Images\image-20201220231322371.png)
>
> 



> 来启动3333端口，但是这个时候先不启动
>
> 可以看到,这个脚本就是通过jvm启动nacos，先添加一个**p** 
>
> ​		所以我们最后修改的就是,**nohup java -Dserver.port=${PORT}**



##### 6,配置Nginx:

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
		server 192.168.162.128:3333;
		server 192.168.162.128:4444;
		server 192.168.162.128:5555;
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

![image-20201221094358925](Images\image-20201221094358925.png)



##### 7,启动Nacos:

在nacos的bin目录下，得用单实例的方法去启动才可以注册进去集群，否则报错

```java
bash startup.sh -p 3333 -m standalone
bash startup.sh -p 4444 -m standalone
bash startup.sh -p 5555 -m standalone
```



##### 8,启动nginx

在/usr/sbin下执行

```java
sudo ./nginx
```



##### 9,测试:

> ​		在windows访问
>
> ```bash
> http://192.168.162.128:1111/nacos/#/login
> ```
>
> ​		如果可以进入nacos的web界面,就证明安装成功了

![image-20201221094454118](Images\image-20201221094454118.png)

在这里能看到我们之前配置的文件，代表持久化配置是没有问题的。



9,将微服务9002注册到Nacos集群:

把注册的网址改了就行

![image-20201221105426442](Images\image-20201221105426442.png)



10,进入Nacos的web界面

​		可以看到,已经注册成功

```java
http://192.168.116.128:1111/nacos/
```

![image-20201221105527684](Images\image-20201221105527684.png)







## Sentinel:

实现熔断与限流,就是Hystrix

![image-20201221105322968](Images\image-20201221105322968.png)

![image-20201221105335839](Images\image-20201221105335839.png)





### ==使用sentinel:==



#### 1,下载sentinel的jar包

百度云有

```java
https://github.com/alibaba/Sentinel/releases/tag/1.7.0
```

​	

#### 2,运行sentinel

​		由于是一个jar包,所以可以直接java -jar运行，在sentinel的包键入cmd

```java
java -jar *.jar
```

​		**注意,默认sentinel占用8080端口**

#### 3,访问sentinel

```bash
localhost:8080
```



### 微服务整合sentinel:

#### 1,启动Nacos，要把微服务注册进去Nacos

#### 2,新建一个项目,8401,主要用于配置sentinel,

```java
cloudalibaba-sentinel-service8401
```

##### 1、pom

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

##### 2、配置文件

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



##### 3、主启动类

```java
@SpringBootApplication
@EnableDiscoveryClient
public class FollowLimitMain8401 {
    public static void main(String[] args) {
        SpringApplication.run(FollowLimitMain8401.class,args);
    }
}

```



##### 4、controller

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

   ```bash
   http://localhost:8401/testA
   ```

   

   

6. 可以看到.已经开始监听了

​    ![image-20201221112134550](Images\image-20201221112134550.png)



### sentinel的流控规则

流量限制控制规则

![image-20201221112238831](Images\image-20201221112238831.png)



==流控模式==:

#### 1、直接快速失败

==直接失败的效果:==

> QPS:每秒请求数，配置QPS是代表每秒只允许一个访问，超过的就会报错，请求还没有到达就被拦截下。

![image-20201221112612496](Images\image-20201221112612496.png)

![image-20201221112701273](Images\image-20201221112701273.png)

![image-20201221112729353](Images\image-20201221112729353.png)



#### 2、线程数:

> 比如a请求过来,处理很慢,在一直处理,此时b请求又过来了,此时因为a占用一个线程,此时要处理b请求就只有额外开启一个线程,那么就会报错，相当于银行里里面只有一个窗口，但是很多人来办理业务，那么就需要等待。

> 此时我们使用postman去访问2000次的testA，A设置了sleep(2000)；那么调用别的窗口去访问就

![image-20201221113844779](Images\image-20201221113844779.png)





#### 3、关联:

> 当关联的资源到达阈值的时候，就限流自己。别人惹事，自己买单。
>
> 跟A关联的资源B到达阈值之后，就限流A自己。B惹事，A挂了。

![image-20201221140608794](Images\image-20201221140608794.png)

使用postman高发密集的访问testB

![image-20201221140903950](Images\image-20201221140903950.png)

这个时候A挂了

![image-20201221140840590](Images\image-20201221140840590.png)



> ==应用场景:  比如**支付接口**达到阈值,就要限流下**订单的接口**,防止一直有订单==

​    

#### 4、链路:

多个请求调用同一个微服务

#### 5、预热Warm up:

> 含义：
> 		即预热/冷启动方式。当系统长期处于低水位的情况下，当流量突然增加时，直接把系统拉升到高水位可能会瞬间把系统压垮。通过“冷启动”，让通过的流量缓慢增加，在一定的时间内逐渐增加到阈值上限，给冷系统一个预热的时间，避免冷系统被压垮。
> 		阈值 / coldFactor（默认值为3），经过预热时长后才会到达阈值，即请求的QPS从threshold / 3 开始，经预热市场逐渐升至设定的QPS阈值。

​	![image-20201221141730726](Images\image-20201221141730726.png)



![image-20201221141927033](Images\image-20201221141927033.png)

刚开始高访问是不行的，5秒后面开始慢慢可以。

 ==应用场景==



#### 7、排队等待:

> 设置含义：每次一秒的QPS请求数达到之后，超过的话就排队等待，等待的超时间为20000毫秒（20秒）

![image-20201221142256343](Images\image-20201221142256343.png)





### 降级规则:

**就是熔断降级**

![image-20201221142714195](Images\image-20201221142714195.png)

![image-20201221142742416](Images\image-20201221142742416.png)

![image-20201221142729419](Images\image-20201221142729419.png)





#### 1,RT配置:

![image-20201221142753578](Images\image-20201221142753578.png)

![image-20201221143034904](Images\image-20201221143034904.png)



新增一个请求方法用于测试

```java
@GetMapping(value = "/testD")
    public String testD(){

        try {
            sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "testD ， 测试RD";
    }
```

==配置RT:==

​				这里配置的PT,默认是秒级的平均响应时间

> 要求200ms内要处理完成这个请求，如果没有处理完成，那么就接下来1s中内窗口期不可用。

![image-20201221143153364](Images\image-20201221143153364.png)

默认计算平均时间是: 1秒类进入5个请求,并且响应的平均值超过阈值(这里的200ms),就报错]

​			1秒5请求是Sentinel默认设置的

==测试==

![image-20201221145754509](Images\image-20201221145754509.png)

![image-20201221145738984](Images\image-20201221145738984.png)

**默认熔断后.就直接抛出异常**







#### 2,异常比例:

![image-20201221145958977](Images\image-20201221145958977.png)

修改请求方法

```java
 @GetMapping(value = "/testD")
    public String testD(){

//        try {
//            sleep(1000);
//        } catch (InterruptedException e) {
//            e.printStackTrace();
//        }
        System.out.println("testD ，异常比例");
        int a = 10 / 0;
        return "testD ， 测试RD";
    }
```



配置:

![image-20201221150206661](Images\image-20201221150206661.png)



==如果没触发熔断,这正常抛出异常==:

![image-20201221150225589](Images\image-20201221150225589.png)

==触发熔断==:

![image-20201221150232935](Images\image-20201221150232935.png)





#### 3, 异常数:

![image-20201221150424508](Images\image-20201221150424508.png)

添加测试方法

```java
  @GetMapping(value = "/testE")
    public String testE(){

        System.out.println("testE ，异常数");
        int a = 10 / 0;
        return "testE ， 异常数测试";
    }
```



一分钟之内,有5个请求发送异常,进入熔断

![image-20201221150606042](Images\image-20201221150606042.png)

![image-20201221150622946](Images\image-20201221150622946.png)





### 热点规则:

![image-20201221150752347](Images\image-20201221150752347.png)



比如:

```bash
localhost:8080/aa?name=aa
localhost:8080/aa?name=bb
```



​			加入两个请求中,带有参数aa的请求访问频次非常高,我们就现在name==aa的请求,但是bb的不限制



==如何自定义降级方法,而不是默认的抛出异常?==



**使用@SentinelResource直接实现降级方法,它等同Hystrix的@HystrixCommand**





==定义热点规则:==

在controller的代码

```java
//上面是请求地址
@GetMapping(value = "/testHotKey")
//这里是资源名，配置热点的时候用
    @SentinelResource(value = "testHotKey",blockHandler = "deal_testHotKey")
    public String testHotKey(@RequestParam(value = "p1",required = false) String p1,
                             @RequestParam(value = "p2",required = false) String p2)
    {
        return "-----------testHotKey";
    }

    public String deal_testHotKey(String p1, String p2, BlockException exception){
        //自定义fallback
        return "deal_testHotKey，/(ㄒoㄒ)/~~";
    }
```

在sentinel配置热点

![image-20201221151528792](Images\image-20201221151528792.png)

这个参数索引指的是control的传入参数，比如0指的就是p1，1指的就是p2

![image-20201221151346815](Images\image-20201221151346815.png)

**此时我们访问/testHotkey并且带上才是p1**

```bash
http://localhost:8401/testHotKey?p1=a&p2=b
```

​			如果qps大于1,就会触发我们定义的降级方法

![image-20201221151607622](Images\image-20201221151607622.png)

**但是我们的参数是P2,就没有问题**

![image-20201221151735789](Images\image-20201221151735789.png)



只有带了p1,才可能会触发热点限流

#### 2,设置热点规则中的其他选项:

基本类型

![image-20201221152119177](Images\image-20201221152119177.png)

**需求:**

![image-20201221152035865](Images\image-20201221152035865.png)



==测试==

![image-20201221152226961](Images\image-20201221152226961.png)

![image-20201221152235904](Images\image-20201221152235904.png)



**注意:**

参数类型只支持,8种基本类型+String类





==注意:==

如果我们程序出现异常,是不会走blockHander的降级方法的**,因为这个方法只配置了热点规则**,没有配置限流规则

我们这里配置的降级方法是sentinel针对热点规则配置的

只有触发热点规则才会降级

![image-20201221152418825](Images\image-20201221152418825.png)



### 3,系统规则:

系统自适应限流:
			**从整体维度对应用入口进行限流**

对整体限流,比如设置qps到达100,这里限流会限制整个系统不可以

![image-20201221152643307](Images\image-20201221152643307.png)

![image-20201221152655270](Images\image-20201221152655270.png)

==测试==:

设置规则

![image-20201221152739004](Images\image-20201221152739004.png)

![image-20201221152804745](Images\image-20201221152804745.png)





### @SentinelResource注解:

**用于配置降级等功能**

#### 1,环境搭建

#### 1、为8401添加依赖

添加我们自己的commone包的依赖

```xml
<dependency><!-- 引用自己定义的api通用包，可以使用Payment支付Entity -->
            <groupId>com.atlgq.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
        </dependency>
```



#### 2、额外创建一个controller类

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



 

#### 3、配置限流

**注意,我们这里配置规则,资源名指定的是@SentinelResource注解value的值,**

**这样也是可以的,也就是不一定要指定访问路径**

![image-20201221153316923](Images\image-20201221153316923.png)



#### 4、测试.

```bash
http://localhost:8401/byResource
```



可以看到已经进入降级方法了

![image-20201221153816329](Images\image-20201221153816329.png)

#### 5、此时我们关闭8401服务

在sentinel上定义的规则都是临时的

可以看到,这些定义的规则是临时的,关闭服务,规则就没有了



**可以看到,上面配置的降级方法,又出现Hystrix遇到的问题了**

> 1、系统默认的，没有体现我们自己的业务要求
>
> 2、依照现有的条件，我们自定义的处理方法又和业务代码耦合在一起，不直观
>
> 3、每个业务方法都要添加一个兜底，代码膨胀
>
> 4、全局统一的处理方法没有体现





### 4、自定义限流处理逻辑:

#### 1、单独创建一个类

用于处理限流，里面的方法需要为静态方法

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



#### 2、在controller中

指定使用自定义类中的方法作为降级方法

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



#### 3、Sentinel中定义流控规则

这里资源名,是以url指定,也可以使用@SentinelResource注解value的值指定

![image-20201221154318347](Images\image-20201221154318347.png)



#### 4、测试

```bash
http://localhost:8401/rateLimit/byUrl1
```

![image-20201221154352606](Images\image-20201221154352606.png)




#### @SentinelResource注解的其他属性:

![image-20201221154513978](Images\image-20201221154513978.png)

![image-20201221154521091](Images\image-20201221154521091.png)









### 服务熔断:

#### 1、启动nacos和sentinel

#### 2、新建两个pay模块  9003和9004

```java
cloudalibaba-provider-payment9003
cloudalibaba-provider-payment9004
```



#### 3、pom

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



#### 4、配置文件

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



#### 5、主启动类 

```java
@SpringBootApplication
@EnableDiscoveryClient
public class PaymentMain9003 {

    public static void main(String[] args) {
        SpringApplication.run(PaymentMain9003.class,args);
    }
}
 

```

#### 6、controller

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



#### 7、 然后启动9003.9004



#### 8、新建一个order-84消费者模块:

```java
cloudailbaba-comsumer-nacos-order84
```





#### 9、pom

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

#### 10、配置文件

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



#### 11、主启动类

```java
@SpringBootApplication
@EnableDiscoveryClient
```



#### 12、配置类

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



#### 13、controller

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



#### 14、为业务方法添加fallback来指定降级方法

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



#### 15、重启order84

测试:

> 在我们的controller方法中添加一个a = 10 / 0;

![image-20201221160049362](Images\image-20201221160049362.png)

![image-20201221160006831](Images\image-20201221160006831.png)



> ​		所以,fallback是用于管理异常的,当业务方法发生异常,可以降级到指定方法。注意,我们这里并没有使用sentinel配置任何规则，但是却降级成功,就是因为fallback是用于管理异常的,当业务方法发生异常,可以降级到指定方法
>

 

### 为业务方法添加blockHandler,看看是什么效果

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



#### 重启84,访问业务方法:



>  可以看到，直接报错了,并没有降级，也就是说,blockHandler只对sentinel定义的规则降级
>

 

#### 如果fallback和blockHandler都配置呢

我们先把前面的 a = 10 / 0 ; 注释掉，正常运行。

设置qps规则,阈值1

![image-20201221160737592](Images\image-20201221160737592.png)

#### 测试

```bash
http://localhost:84/consumer/fallback/1
```

 ![image-20201221160809056](Images\image-20201221160809056.png)

 可以看到,当两个都同时生效时,==blockhandler优先生效==



### @SentinelResource还有一个属性,exceptionsToIgnore

![image-20201221161039232](Images\image-20201221161039232.png)

>  exceptionsToIgnore指定一个异常类，表示如果当前方法抛出的是指定的异常,不降级,直接对用户抛出异常



## sentinel整合ribbon+openFeign+fallback



### 1、修改84模块,使其支持feign

#### 1、pom

```xml
<dependency>
            <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
```



#### 2、配置文件

```yaml
#开启整合
feign:
  sentinel:
    enabled: true
```

​    

#### 3、主启动类,也要添加

```java
@EnableFeignClients
```



#### 4、创建远程调用pay模块的接口

```java
@FeignClient(value = "nacos-payment-provider" ,fallback = PaymentFallbackService.class)
public interface PaymentService {
@GetMapping(value = "/paymentSQL/{id}")
    public CommonResult<Payment> paymentSQL(@PathVariable("id") Long id);
}

```



#### 5、创建这个接口的实现类,用于降级

```java
@Component
public class PaymentFallbackService implements PaymentService {
    @Override
    public CommonResult<Payment> paymentSQL(Long id) {
        return new CommonResult(404,"this is PaymentFallbackService");
    }
}

```



#### 6、再次修改接口,指定降级类

```java
@FeignClient(value = "nacos-payment-provider" ,fallback = PaymentFallbackService.class)
```



#### 7、controller添加远程调用

```java
    @Resource
    private PaymentService paymentService;

    @GetMapping(value = "/consumer/paymentSQL/{id}")
    public CommonResult<Payment> paymentSQL(@PathVariable("id") Long id) {
        return paymentService.paymentSQL(id);
    }

```



#### 8、测试

启动9003,84

#### 9、测试,如果关闭9003.看看84会不会降级

![image-20201221161259343](Images\image-20201221161259343.png)



**可以看到,正常降级了**



**熔断框架比较**

![image-20201221161444144](Images\image-20201221161444144.png)







## sentinel持久化规则

![image-20201221161629223](Images\image-20201221161629223.png)

默认规则是临时存储的,重启sentinel就会消失

**这里以之前的8401为案例进行修改:**

### 1、修改8401的pom

```xml
<!-- SpringCloud ailibaba sentinel-datasource-nacos 持久化需要用到-->
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
</dependency> 
```

### 2、修改配置文件:

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
        #这是持久化要添加的datasource
      datasource:
        nacos:
          server-add: localhost:8848
          dataId: cloudalibaba-sentinel-service
          groupId: DEFAULT_GROUP
          data-type: json
          rule-type: flow
```

> 实际上就是指定,我们的规则要保证在哪个名称空间的哪个分组下
>
> 这里没有指定namespace, 但是是可以指定的
>
> 注意,这里的dataid要与8401的服务名一致

### 3、在nacos中创建一个配置文件,dataId就是上面配置文件中指定的

![image-20201221162155349](Images\image-20201221162155349.png)

json中,这些属性的含义

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



### 4、启动8401:nacos，sentinel

可以看到,直接读取到了规则

![image-20201221162747798](Images\image-20201221162747798.png)

![image-20201221162800606](Images\image-20201221162800606.png)

### 5、关闭8401

### 6、此时重启8401

如果sentinel又可以正常读取到规则,那么证明持久化成功，可以看到,又重新出现了



# Seata:

是一个分布式事务的解决方案,

![image-20201221164010755](Images\image-20201221164010755.png)

**分布式事务中的一些概念,也是seata中的概念:**

![image-20201221164155961](Images\image-20201221164155961.png)

![image-20201221164203570](Images\image-20201221164203570.png)



## seata安装

### 1、下载安装seata的安装包

百度云

### 2、**修改file.conf**，先备份文件

![image-20201221164531423](Images\image-20201221164531423.png)

 ![image-20201221164659562](Images\image-20201221164659562.png)



### 3、mysql建库建表

#### 1,上面指定了数据库为seata,所以创建一个数据库名为seata

#### 2,建表,在seata的安装目录下有一个db_store.sql,运行即可

```sql
source D:\Program Files\Program Files\SpringCloud\seata-server-0.9.0\seata\conf\db_store.sql
```

![image-20201221164933202](Images\image-20201221164933202.png)

#### 4、修改registry.conf

配置seata作为微服务,指定注册中心

![image-20201221165211644](Images\image-20201221165211644.png)

#### 		5、启动

> 先启动nacos，在bin目录下的bat
>
> 在启动seata-server(运行安装目录下的,seata-server.bat)

![image-20201221165714634](Images\image-20201221165714634.png)

**业务说明**

![image-20201221165824895](Images\image-20201221165824895.png)

下单--->库存--->账号余额



##### 			1、创建三个数据库

```sql
create database seata_order;
create database seata_storage;
create database seata_account;
```



##### 			2、创建对应的表

```sql
use seata_order;
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
use seata_storage;
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
use seata_account;
CREATE TABLE t_account(
    `id` BIGINT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY COMMENT 'id',
    `user_id` BIGINT(11) DEFAULT NULL COMMENT '用户id',
    `total` DECIMAL(10,0) DEFAULT NULL COMMENT '总额度',
    `used` DECIMAL(10,0) DEFAULT NULL COMMENT '已用余额',
    `residue` DECIMAL(10,0) DEFAULT '0' COMMENT '剩余可用额度'
) ENGINE=INNODB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;
 
INSERT INTO seata_account.t_account(`id`,`user_id`,`total`,`used`,`residue`) VALUES('1','1','1000','0','1000');
 
 
 
SELECT * FROM t_account;

```



##### 		3、创建回滚日志表,方便查看

把conf下面的db_undo_log.sql，分别写进去各个数据库的里面，使用source的方法

```java
db_undo_log.sql
```

```sql
--分别在三个数据库下
source D:\Program Files\Program Files\SpringCloud\seata-server-0.9.0\seata\conf\db_undo_log.sql
```

注意:每个库都要执行一次这个sql,生成回滚日志表

##### 4、完整数据库

![image-20201221170859360](Images\image-20201221170859360.png)

##### 		每个业务都创建一个微服务,也就是要有三个微服务,订单,库存,账号

​    

# seata-order-service2001

## 			1、pom

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

## 				2、配置文件

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

## 			3、file.conf

> 还要额外创建其他配置文件,创建一个file.conf,
>
> 这个文件是从conf下拷贝过来的

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
#更改的部分，和之前该配置文件里面的相对应
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

## 			4、创建registry.conf:

> 也是从conf中拷贝过来的

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

> 实际上,就是要将seata中的我们之前修改的两个配置文件复制到这个项目下

## 5、主启动类

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



## 6、service层

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

### Impl实现类

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



 ![image-20201221171607326](Images\image-20201221171607326.png)

 

## 7、dao层,也就是接口

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

## 8、在resource下创建mapper文件夹,编写mapper.xml

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

## 9、controller层

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



## 10、entity类(也叫domain类)

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



 

 



## 11、config配置类

```java
@Configuration
@MapperScan({"com.atlgq.springcloud.alibaba.dao"})
public class MyBatisConfig {

}

```

```java
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









#   库存

# seata-order-service2002



## 1、pom   

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



## 2、配置文件

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



## 3、主启动类

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



## 4、service层

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



## 5、dao层

```java
@Mapper
public interface StorageDao {


    //扣减库存信息
    void decrease(@Param("productId") Long productId, @Param("count") Integer count);
    void insert();
}
```



## 6、controller层

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

## 7、domain

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



##  8、config的和2001一样



#  账号

# seata-order-service2003





## 1、pom     

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



## 2、配置文件

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



## 3、主启动类

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



## 4、service层

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



## 4、dao层

```java
@Mapper
public interface AccountDao {

    /**
     * 扣减账户余额
     */
    void decrease(@Param("userId") Long userId, @Param("money") BigDecimal money);
}


```



## 5、controller层

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

## 6、domain

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

## 7、config和2001一样





## 8、全局创建完成后,首先测试不加seata







​    

​    

​    

## 9、使用seata:

> 在订单模块的serviceImpl类中的create方法添加启动分布式事务的注解
>
> @GlobalTransactional(name = "fsp-create-order",rollbackFor = Exception.class)
>
> 对fsp-create-order内元素进行管理，这个由resources下面的file.conf配置

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

 

##### 10、此时在测试

发现,发生异常后,直接回滚了,前面的修改操作都回滚了，不会修改数据库任何值

在orderController添加order数据订单

```java
//设置order订单。
        order.setUserId(1L);
        order.setProductId(1L);
        order.setCount(1);
        BigDecimal money = BigDecimal.valueOf(99);
        order.setMoney(money);
        order.setStatus(1);
```



```bash
http://localhost:2001/order/create
```

![image-20201222093919538](Images\image-20201222093919538.png)

![image-20201222093942217](Images\image-20201222093942217.png)

> 这个时候如果进行订单创建的时候中途除了问题，所有的事件都是会回滚的，不会对数据库产生任何的改变，这就是seata的作用。我在测试的时候出现了添加订单时候product_id是t_sotrage数据库没有的，但是还是创建成功，我后面新添加了对数据创建的判断。
>
> ![image-20201222094129083](Images\image-20201222094129083.png)
>
> 这个时候如果插入不存在数据的话是会报错的。
>
> 我想过给t_order表格添加一个外键，添加t_storage对t_order 中product_id的约束，但是它们不是处于一个数据库的表格，似乎不能够添加外键。所以就采取了判断的方法。





### setat原理:





**seata提供了四个模式:**





第一阶段:



![image-20201222094704461](Images\image-20201222094704461.png)

二阶段之提交

![image-20201222094811223](Images\image-20201222094811223.png)



二阶段之回滚:



![image-20201222094650727](Images\image-20201222094650727.png)



断点



**可以看到,他们的xid全局事务id是一样的,证明他们在一个事务下**



**在更新数据之前,先解析这个更新sql,然后查询要更新的数据,进行保存**

访问的话需要开启nacos和seata

#### 测试链接

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

# **################高级篇分割线####################**